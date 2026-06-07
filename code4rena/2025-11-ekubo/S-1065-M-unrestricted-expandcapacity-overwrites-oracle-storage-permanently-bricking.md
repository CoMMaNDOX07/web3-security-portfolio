# S-1065: Unrestricted expandCapacity overwrites oracle storage, permanently bricking

## Metadata

| Field | Details |
|---|---|
| Project | Ekubo |
| Platform | Code4rena |
| Severity | Medium |
| Status | Valid — Sufficient — Fixed |
| Code4rena Handle | CoMManDO |
| Submission ID | S-1065 |
| Public Report Mapping | M-01 |
| Contest Date | 19 Nov 2025 — 10 Dec 2025 |
| Report Date | 12 Jan 2026 |
| Ecosystem / Stack | EVM, Solidity, AMM, Oracle |
| Affected File | `src/extensions/Oracle.sol` |
| Affected Functions | `expandCapacity()`, `maybeInsertSnapshot()` |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-11-ekubo) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-11-ekubo) |
| Original Submission | [S-1065](https://code4rena.com/audits/2025-11-ekubo/submissions/S-1065) |
| Affected Code | [Oracle.sol#L213](https://github.com/code-423n4/2025-11-ekubo/blob/main/src/extensions/Oracle.sol#L213), [Oracle.sol#L97](https://github.com/code-423n4/2025-11-ekubo/blob/main/src/extensions/Oracle.sol#L97) |

## Summary

The oracle extension used a manual storage layout where token `Counts` data and snapshot data were stored directly in raw storage slots derived from token addresses and snapshot indexes.

The `expandCapacity()` function was externally callable and accepted a user-controlled `token` address and `minCapacity`. When called with specific values, it could write to raw storage slots that overlapped with oracle metadata for other tokens.

This could corrupt oracle state and cause affected pools or oracle reads to become unusable.

## Vulnerability Details

The vulnerable function was:

```solidity
function expandCapacity(address token, uint32 minCapacity) external returns (uint32 capacity) {
    Counts c;
    assembly ("memory-safe") {
        c := sload(token)
    }

    if (c.capacity() < minCapacity) {
        for (uint256 i = c.capacity(); i < minCapacity; i++) {
            assembly ("memory-safe") {
                // Simply initialize the slot, it will be overwritten when the index is reached
                sstore(or(shl(32, token), i), 1)
            }
        }

        c = createCounts({
            _index: c.index(),
            _count: c.count(),
            _capacity: minCapacity,
            _lastTimestamp: c.lastTimestamp()
        });

        assembly ("memory-safe") {
            sstore(token, c)
        }
    }

    capacity = c.capacity();
}
```

The oracle manually stored:

```text
Counts(token) at slot:
token
```

and snapshots at:

```text
Snapshot(token, index) at slot:
(token << 32) | index
```

The dangerous write in `expandCapacity()` was:

```solidity
sstore(or(shl(32, token), i), 1)
```

Because `token` and `minCapacity` were user-controlled, an external caller could cause the function to initialize many raw storage slots.

## Attack Scenario

A particularly dangerous case is calling:

```solidity
oracle.expandCapacity(address(0), M);
```

When `token == address(0)`:

```text
shl(32, token) == 0
or(shl(32, token), i) == i
```

So the loop becomes equivalent to:

```solidity
sstore(i, 1);
```

for all indexes in the selected range.

If another token’s `Counts` metadata is stored at one of those slots, it can be overwritten with raw value `1`.

After corruption, loading `Counts` for the affected token can produce invalid state, for example:

```text
index = 1
count = 0
capacity = 0
lastTimestamp = 0
```

This breaks assumptions in `maybeInsertSnapshot()`.

## maybeInsertSnapshot Failure

`maybeInsertSnapshot()` assumes the oracle state has a valid non-zero count:

```solidity
Counts c;
assembly ("memory-safe") {
    c := sload(token)
}

uint32 timePassed = uint32(block.timestamp) - c.lastTimestamp();
if (timePassed == 0) return;

uint32 index = c.index();

Snapshot last;
assembly ("memory-safe") {
    last := sload(or(shl(32, token), index))
}

uint32 count = c.count();
uint32 capacity = c.capacity();

bool isLastIndex = index == count - 1;
bool incrementCount = isLastIndex && capacity > count;

if (incrementCount) count++;

index = (index + 1) % count;
```

If corrupted state causes:

```text
count = 0
```

then this operation reverts:

```solidity
index = (index + 1) % count;
```

because it attempts modulo by zero.

## Impact

For affected tokens or pools, oracle snapshot insertion can become permanently broken.

This can cause:

- Swaps with non-zero amounts to revert when oracle hooks are triggered
- Liquidity updates with non-zero liquidity deltas to revert
- Oracle functionality to become unusable for affected pools
- Corruption of historical oracle data
- Downstream oracle consumers to receive corrupted or unusable data

Because the corrupted value is written directly to storage, remediation can be difficult without an explicit migration or repair mechanism.

## Root Cause

The root cause is unsafe manual storage slot derivation.

The contract stores different data types in storage regions that are not cryptographically separated:

```text
Counts slot = token
Snapshot slot = (token << 32) | index
```

This allows storage collisions between token metadata and snapshot data.

The issue is made worse by `expandCapacity()` being externally callable and using user-controlled input to write to raw storage slots.

## Recommended Mitigation

Avoid raw overlapping storage regions.

A safer design is to use standard Solidity mappings:

```solidity
mapping(address => Counts) internal counts;
mapping(address => mapping(uint256 => Snapshot)) internal snapshots;
```

If manual storage is required, use disjoint storage namespaces with `keccak256`.

Example pattern:

```solidity
function getCountsSlot(address token) private pure returns (bytes32 slot) {
    assembly ("memory-safe") {
        mstore(0x00, token)
        mstore(0x20, "EKUBO_ORACLE_COUNTS")
        slot := keccak256(0x00, 0x40)
    }
}

function getSnapshotSlot(address token, uint32 index) private pure returns (bytes32 slot) {
    assembly ("memory-safe") {
        mstore(0x00, token)
        mstore(0x20, index)
        mstore(0x40, "EKUBO_ORACLE_SNAPSHOT")
        slot := keccak256(0x00, 0x60)
    }
}
```

The key idea is that `Counts` and `Snapshot` storage must live in separate, collision-resistant namespaces.

## Proof of Concept Summary

The submitted PoC demonstrated the following:

1. Deploy a token at a small address.
2. Initialize an oracle-enabled pool using that token.
3. Confirm that the token’s `Counts` state is valid.
4. Call `expandCapacity(address(0), 2)`.
5. The call writes raw value `1` into low-numbered storage slots.
6. The token’s `Counts` state becomes corrupted.
7. After time advances, `beforeSwap()` triggers `maybeInsertSnapshot()`.
8. `maybeInsertSnapshot()` attempts modulo by zero and reverts.
9. The affected pool/oracle path becomes unusable.

## References

- [Official Code4rena Report](https://code4rena.com/reports/2025-11-ekubo)
- [Code4rena Contest Page](https://code4rena.com/audits/2025-11-ekubo)
- [Original Submission S-1065](https://code4rena.com/audits/2025-11-ekubo/submissions/S-1065)
- [Affected Code: Oracle.sol#L213](https://github.com/code-423n4/2025-11-ekubo/blob/main/src/extensions/Oracle.sol#L213)
- [Affected Code: Oracle.sol#L97](https://github.com/code-423n4/2025-11-ekubo/blob/main/src/extensions/Oracle.sol#L97)
