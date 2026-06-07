# S-108: injectSweepAndCall miscalculates received tokens, causing reverts

## Metadata

| Field | Details |
|---|---|
| Project | Sequence: Transaction Rails |
| Platform | Code4rena |
| Severity | Low |
| Status | Valid |
| Code4rena Handle | CoMManDO |
| Submission ID | S-108 |
| Contest Date | 11 Nov 2025 — 17 Nov 2025 |
| Report Date | 17 Dec 2025 |
| Ecosystem / Stack | EVM, Solidity, Multichain Transaction Rails |
| Affected File | `src/TrailsRouter.sol` |
| Affected Functions | `injectSweepAndCall()`, `_injectAndExecuteCall()` |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-11-sequence-transaction-rails) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-11-sequence-transaction-rails) |
| Original Submission | [S-108](https://code4rena.com/audits/2025-11-sequence-transaction-rails/submissions/S-108) |
| Affected Code | [TrailsRouter.sol#L107](https://github.com/code-423n4/2025-11-sequence/blob/main/src/TrailsRouter.sol#L107), [TrailsRouter.sol#L362](https://github.com/code-423n4/2025-11-sequence/blob/main/src/TrailsRouter.sol#L362) |

## Summary

`injectSweepAndCall()` used the sender’s full token balance as the amount to inject into calldata and approve to the target.

This is unsafe for fee-on-transfer or rebasing tokens because the router may receive less than the sender’s original balance after the transfer.

## Vulnerability Details

The function calculates `callerBalance` from the sender’s balance:

```solidity
callerBalance = _getBalance(token, msg.sender);
if (callerBalance == 0) revert NoTokensToSweep();

_safeTransferFrom(token, msg.sender, address(this), callerBalance);
```

It then passes `callerBalance` into `_injectAndExecuteCall()`:

```solidity
_injectAndExecuteCall(token, target, callData, amountOffset, placeholder, callerBalance);
```

Later, the router approves the target for the same `callerBalance`:

```solidity
IERC20 erc20 = IERC20(token);
SafeERC20.forceApprove(erc20, target, callerBalance);

(bool success, bytes memory result) = target.call(callData);
emit BalanceInjectorCall(token, target, placeholder, callerBalance, amountOffset, success, result);

if (!success) revert TargetCallFailed(result);
```

The issue is that `callerBalance` is the pre-transfer amount, not the actual amount received by the router.

For fee-on-transfer tokens:

- Sender balance may be `1000`
- Router transfers `1000`
- Router receives only `900`
- Placeholder is replaced with `1000`
- Target is approved for `1000`
- Target may try to pull `1000` and revert because the router only holds `900`

## Impact

The mismatch can cause downstream target calls to revert or behave incorrectly.

This can break router flows for fee-on-transfer, deflationary, or rebasing tokens.

It can also create griefing or denial-of-service conditions where the router consistently passes an amount larger than what it actually received.

## Proof of Concept Summary

The submitted PoC used a fee-on-transfer ERC20 token and a target contract that pulls the approved amount from the router.

The router:

1. Reads the caller’s full token balance.
2. Transfers that full amount from the caller.
3. Receives less due to transfer fees.
4. Injects the original full amount into calldata.
5. Approves the target for the original full amount.
6. The target attempts to pull the full amount and reverts.

## Root Cause

The root cause is using the sender’s pre-transfer balance instead of the router’s actual received amount.

## Recommended Mitigation

For ERC20 tokens, calculate the actual received amount using a balance delta.

Suggested fix pattern:

```solidity
uint256 balanceBefore = IERC20(token).balanceOf(address(this));

_safeTransferFrom(token, msg.sender, address(this), callerBalance);

uint256 balanceAfter = IERC20(token).balanceOf(address(this));
uint256 actualReceived = balanceAfter - balanceBefore;

_injectAndExecuteCall(token, target, callData, amountOffset, placeholder, actualReceived);
```

The approval inside `_injectAndExecuteCall()` should also use the actual received amount.

## References

- [Official Code4rena Report](https://code4rena.com/reports/2025-11-sequence-transaction-rails)
- [Code4rena Contest Page](https://code4rena.com/audits/2025-11-sequence-transaction-rails)
- [Original Submission S-108](https://code4rena.com/audits/2025-11-sequence-transaction-rails/submissions/S-108)
- [Affected Code: TrailsRouter.sol#L107](https://github.com/code-423n4/2025-11-sequence/blob/main/src/TrailsRouter.sol#L107)
- [Affected Code: TrailsRouter.sol#L362](https://github.com/code-423n4/2025-11-sequence/blob/main/src/TrailsRouter.sol#L362)
