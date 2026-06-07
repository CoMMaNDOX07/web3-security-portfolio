# S-104: Public injectAndCall allows full router asset drain

## Metadata

| Field | Details |
|---|---|
| Project | Sequence: Transaction Rails |
| Platform | Code4rena |
| Severity | Low |
| Status | Valid |
| Code4rena Handle | CoMManDO |
| Submission ID | S-104 |
| Contest Date | 11 Nov 2025 — 17 Nov 2025 |
| Report Date | 17 Dec 2025 |
| Ecosystem / Stack | EVM, Solidity, Multichain Transaction Rails |
| Affected File | `src/TrailsRouter.sol` |
| Affected Functions | `injectAndCall()`, `_injectAndExecuteCall()` |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-11-sequence-transaction-rails) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-11-sequence-transaction-rails) |
| Original Submission | [S-104](https://code4rena.com/audits/2025-11-sequence-transaction-rails/submissions/S-104) |
| Affected Code | [TrailsRouter.sol#L129](https://github.com/code-423n4/2025-11-sequence/blob/main/src/TrailsRouter.sol#L129), [TrailsRouter.sol#L358](https://github.com/code-423n4/2025-11-sequence/blob/main/src/TrailsRouter.sol#L358) |

## Summary

`injectAndCall()` was publicly callable and used the router contract’s own ETH or ERC20 balance as the amount to spend.

Because it was not restricted to delegatecall usage, any external caller could trigger the router to spend or approve the router’s full balance.

## Vulnerability Details

The public function used the router’s self-balance:

```solidity
function injectAndCall(
    address token,
    address target,
    bytes calldata callData,
    uint256 amountOffset,
    bytes32 placeholder
) public payable {
    uint256 callerBalance = _getSelfBalance(token);

    if (callerBalance == 0) {
        if (token == address(0)) {
            revert NoEthAvailable();
        } else {
            revert NoTokensToSweep();
        }
    }

    _injectAndExecuteCall(token, target, callData, amountOffset, placeholder, callerBalance);
}
```

For ETH, `_injectAndExecuteCall()` forwards the full router balance to the target:

```solidity
(bool success, bytes memory result) = target.call{value: callerBalance}(callData);
```

For ERC20 tokens, it approves the target for the full router token balance:

```solidity
IERC20 erc20 = IERC20(token);
SafeERC20.forceApprove(erc20, target, callerBalance);

(bool success, bytes memory result) = target.call(callData);
```

The issue is that any caller could trigger this function directly and choose an arbitrary target.

## Impact

Any ETH or ERC20 tokens accidentally or temporarily held by the router could be drained by an arbitrary external caller.

Attack scenarios include:

- Router holds ETH from a mistaken direct transfer
- Router holds leftover ERC20 tokens from a previous operation
- Attacker calls `injectAndCall()`
- Router forwards ETH or approves ERC20 spending to the attacker-controlled target

This exposes router-held balances to theft.

## Proof of Concept Summary

The submitted PoC demonstrated two cases:

1. Router-held ETH could be drained by directly calling `injectAndCall(address(0), target, ...)`.
2. Router-held ERC20 tokens could be drained by calling `injectAndCall(token, target, ...)`, where the target pulls the approved amount from the router.

## Root Cause

The root cause is that `injectAndCall()` is public and spends the router’s own balance without access control or delegatecall restriction.

## Recommended Mitigation

Recommended fixes include:

- Add `onlyDelegatecall` to `injectAndCall()`
- Remove the public entry point if not required
- Route usage through `handleSequenceDelegateCall`
- If a public variant is required, use only `msg.value` or an explicit amount pulled from `msg.sender`
- Never spend the router’s full self-balance based only on a public caller’s request

Example mitigation:

```solidity
function injectAndCall(
    address token,
    address target,
    bytes calldata callData,
    uint256 amountOffset,
    bytes32 placeholder
) public payable onlyDelegatecall {
    ...
}
```

## References

- [Official Code4rena Report](https://code4rena.com/reports/2025-11-sequence-transaction-rails)
- [Code4rena Contest Page](https://code4rena.com/audits/2025-11-sequence-transaction-rails)
- [Original Submission S-104](https://code4rena.com/audits/2025-11-sequence-transaction-rails/submissions/S-104)
- [Affected Code: TrailsRouter.sol#L129](https://github.com/code-423n4/2025-11-sequence/blob/main/src/TrailsRouter.sol#L129)
- [Affected Code: TrailsRouter.sol#L358](https://github.com/code-423n4/2025-11-sequence/blob/main/src/TrailsRouter.sol#L358)
