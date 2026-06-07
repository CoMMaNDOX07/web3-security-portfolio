# S-102: Unrestricted ERC20 sweep allows router balance drain

## Metadata

| Field | Details |
|---|---|
| Project | Sequence: Transaction Rails |
| Platform | Code4rena |
| Severity | Low |
| Status | Valid |
| Code4rena Handle | CoMManDO |
| Submission ID | S-102 |
| Contest Date | 11 Nov 2025 — 17 Nov 2025 |
| Report Date | 17 Dec 2025 |
| Ecosystem / Stack | EVM, Solidity, Multichain Transaction Rails |
| Affected File | `src/TrailsRouter.sol` |
| Affected Functions | `injectSweepAndCall()`, `injectAndCall()`, `_injectAndExecuteCall()` |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-11-sequence-transaction-rails) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-11-sequence-transaction-rails) |
| Original Submission | [S-102](https://code4rena.com/audits/2025-11-sequence-transaction-rails/submissions/S-102) |
| Affected Code | [TrailsRouter.sol#L116](https://github.com/code-423n4/2025-11-sequence/blob/main/src/TrailsRouter.sol#L116), [TrailsRouter.sol#L129](https://github.com/code-423n4/2025-11-sequence/blob/main/src/TrailsRouter.sol#L129), [TrailsRouter.sol#L362](https://github.com/code-423n4/2025-11-sequence/blob/main/src/TrailsRouter.sol#L362) |

## Summary

The ERC20 flow swept the caller’s full token balance into the router, but unused tokens could remain stranded on the router contract.

Because `injectAndCall()` was public and used the router’s own token balance without access control, anyone could later drain leftover ERC20 tokens.

## Vulnerability Details

The sweep flow transfers the caller’s full ERC20 balance into the router:

```solidity
callerBalance = _getBalance(token, msg.sender);
if (callerBalance == 0) revert NoTokensToSweep();

_safeTransferFrom(token, msg.sender, address(this), callerBalance);
```

Then the router executes the target call:

```solidity
_injectAndExecuteCall(token, target, callData, amountOffset, placeholder, callerBalance);
```

However, the target may not use the full approved amount. Any unused balance can remain on the router.

The public `injectAndCall()` function then allows anyone to use the router’s self-balance:

```solidity
function injectAndCall(
    address token,
    address target,
    bytes calldata callData,
    uint256 amountOffset,
    bytes32 placeholder
) public payable {
    uint256 callerBalance = _getSelfBalance(token);
    ...
    _injectAndExecuteCall(token, target, callData, amountOffset, placeholder, callerBalance);
}
```

For ERC20 tokens, `_injectAndExecuteCall()` approves the target for the full router balance:

```solidity
IERC20 erc20 = IERC20(token);
SafeERC20.forceApprove(erc20, target, callerBalance);

(bool success, bytes memory result) = target.call(callData);
```

This allows leftover router balances to become globally drainable.

## Impact

If a previous user leaves ERC20 tokens on the router, an attacker can call `injectAndCall()` and drain those leftover tokens.

This can happen when:

- The router sweeps the user’s full ERC20 balance
- The target only pulls part of the approved balance
- The remainder stays on the router
- An attacker later drains the remainder through the public router function

This creates a loss-of-funds risk for users whose tokens remain on the router.

## Proof of Concept Summary

The submitted PoC demonstrated the following flow:

1. Victim approves the router.
2. Victim calls `injectSweepAndCall()`.
3. Router sweeps the victim’s full token balance.
4. Target pulls only a small portion.
5. Leftover tokens remain on the router.
6. Attacker calls public `injectAndCall()`.
7. Router approves attacker-controlled target for the leftover balance.
8. Attacker drains the leftover ERC20 tokens.

## Root Cause

The issue is caused by combining two unsafe behaviors:

1. Sweeping the caller’s entire ERC20 balance instead of an explicit amount.
2. Allowing a public function to spend the router’s self-balance without authorization.

## Recommended Mitigation

Recommended fixes include:

- Pull an explicit user-specified amount instead of sweeping the full balance.
- Return any leftover tokens to the original caller after execution.
- Restrict `injectAndCall()` with `onlyDelegatecall`.
- Avoid using the router’s full self-balance in public functions.
- Enforce atomic spend or refund behavior.

Example safer pattern:

```solidity
uint256 balanceBefore = IERC20(token).balanceOf(address(this));

_safeTransferFrom(token, msg.sender, address(this), amount);

_injectAndExecuteCall(token, target, callData, amountOffset, placeholder, amount);

uint256 leftover = IERC20(token).balanceOf(address(this)) - balanceBefore;
if (leftover > 0) {
    IERC20(token).safeTransfer(msg.sender, leftover);
}
```

## References

- [Official Code4rena Report](https://code4rena.com/reports/2025-11-sequence-transaction-rails)
- [Code4rena Contest Page](https://code4rena.com/audits/2025-11-sequence-transaction-rails)
- [Original Submission S-102](https://code4rena.com/audits/2025-11-sequence-transaction-rails/submissions/S-102)
- [Affected Code: TrailsRouter.sol#L116](https://github.com/code-423n4/2025-11-sequence/blob/main/src/TrailsRouter.sol#L116)
- [Affected Code: TrailsRouter.sol#L129](https://github.com/code-423n4/2025-11-sequence/blob/main/src/TrailsRouter.sol#L129)
- [Affected Code: TrailsRouter.sol#L362](https://github.com/code-423n4/2025-11-sequence/blob/main/src/TrailsRouter.sol#L362)
