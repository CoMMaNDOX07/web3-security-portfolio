# S-112: Events misreport deposits for rebasing tokens

## Metadata

| Field | Details |
|---|---|
| Project | Sequence: Transaction Rails |
| Platform | Code4rena |
| Severity | Low |
| Status | Valid |
| Code4rena Handle | CoMManDO |
| Submission ID | S-112 |
| Contest Date | 11 Nov 2025 — 17 Nov 2025 |
| Report Date | 17 Dec 2025 |
| Ecosystem / Stack | EVM, Solidity, Multichain Transaction Rails |
| Affected File | `src/TrailsIntentEntrypoint.sol` |
| Affected Function | `depositToIntent()` |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-11-sequence-transaction-rails) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-11-sequence-transaction-rails) |
| Original Submission | [S-112](https://code4rena.com/audits/2025-11-sequence-transaction-rails/submissions/S-112) |
| Affected Code | [TrailsIntentEntrypoint.sol#L139](https://github.com/code-423n4/2025-11-sequence/blob/main/src/TrailsIntentEntrypoint.sol#L139) |

## Summary

`depositToIntent()` assumed that transferring `amount` tokens from the user to the intent address always delivered exactly `amount`.

This assumption is unsafe for fee-on-transfer, deflationary, or rebasing tokens. The recipient may receive less than the requested transfer amount, but the contract still emits `IntentDeposit` using the full requested amount.

## Vulnerability Details

The vulnerable logic transfers tokens and then emits the requested amount:

```solidity
IERC20(token).safeTransferFrom(user, intentAddress, amount);

// Pay fee if specified
if (feeAmount > 0 && feeCollector != address(0)) {
    IERC20(token).safeTransferFrom(user, feeCollector, feeAmount);
    emit FeePaid(user, token, feeAmount, feeCollector);
}

emit IntentDeposit(user, intentAddress, amount);
```

The issue is that `amount` represents the requested transfer amount, not the actual amount received by `intentAddress`.

For standard ERC20 tokens, this may be correct. However, for fee-on-transfer or rebasing tokens:

- The user may transfer `amount`
- The recipient may receive less than `amount`
- The event still reports the full `amount`

This creates a mismatch between on-chain token balances and emitted accounting data.

## Impact

Events can misreport the actual deposited amount.

This can affect:

- Off-chain indexers
- Frontends
- Analytics systems
- Intent accounting systems
- Integrations that credit users based on `IntentDeposit`

If downstream systems rely on the event amount, users may be credited for more tokens than were actually received.

## Proof of Concept Summary

The submitted PoC demonstrated the issue using a fee-on-transfer ERC20 token.

Example flow:

1. User deposits `100` tokens.
2. Token charges a 1% transfer fee.
3. `intentAddress` receives only `99` tokens.
4. The contract emits `IntentDeposit(user, intentAddress, 100)`.
5. Off-chain systems may treat the deposit as `100`, even though the actual received amount is `99`.

## Root Cause

The root cause is emitting the requested transfer amount instead of measuring the actual balance delta received by the intent address.

## Recommended Mitigation

Measure the recipient balance before and after the transfer, then emit the actual received amount.

Suggested fix pattern:

```solidity
uint256 balanceBefore = IERC20(token).balanceOf(intentAddress);

IERC20(token).safeTransferFrom(user, intentAddress, amount);

uint256 balanceAfter = IERC20(token).balanceOf(intentAddress);
uint256 actualReceived = balanceAfter - balanceBefore;

emit IntentDeposit(user, intentAddress, actualReceived);
```

The same balance-delta accounting should be applied to the permit-based deposit path if applicable.

## References

- [Official Code4rena Report](https://code4rena.com/reports/2025-11-sequence-transaction-rails)
- [Code4rena Contest Page](https://code4rena.com/audits/2025-11-sequence-transaction-rails)
- [Original Submission S-112](https://code4rena.com/audits/2025-11-sequence-transaction-rails/submissions/S-112)
- [Affected Code: TrailsIntentEntrypoint.sol#L139](https://github.com/code-423n4/2025-11-sequence/blob/main/src/TrailsIntentEntrypoint.sol#L139)
