# L-01: Undercollateralized aToken may be incorrectly priced as positive

## Metadata

| Field | Details |
|---|---|
| Project | Covenant |
| Platform | Code4rena |
| Severity | Low |
| Status | Valid — Primary — Sufficient — Fixed |
| Code4rena Handle | CoMManDO |
| Finding ID | F-367 |
| Submission ID | S-554 |
| Contest Date | 22 Oct 2025 — 3 Nov 2025 |
| Report Date | 17 Nov 2025 |
| Ecosystem / Stack | Solidity, DeFi |
| Affected File | `LatentSwapLogic.sol` |
| Affected Function | `_calculateTokenPrices()` |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-10-covenant) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-10-covenant) |
| Original Submission | [S-554](https://code4rena.com/audits/2025-10-covenant/submissions/S-554) |
| Affected Code | [LatentSwapLogic.sol#L1195](https://github.com/code-423n4/2025-10-covenant/blob/main/src/lex/latentswap/libraries/LatentSwapLogic.sol#L1195) |

## Summary

The aToken pricing logic could incorrectly return a positive price during undercollateralized market conditions.

In `_calculateTokenPrices()`, the code attempts to set the aToken price to zero when the market is undercollateralized and leverage tokens exist. However, it checks `marketState.dexAmounts[LVRG] > 0`.

This is problematic because when the market becomes undercollateralized, `dexAmounts[LVRG]` is set to zero earlier in the market-state calculation. Therefore, the condition can evaluate to false even when aTokens still exist, causing the logic to calculate and return a non-zero aToken price.

## Vulnerability Details

The vulnerable logic was:

```solidity
tokenPrices.aTokenPrice = (marketState.underCollateralized && marketState.dexAmounts[LVRG] > 0)
    ? 0
    : tokenPrices.baseTokenPrice.mulDiv(
        _calcRatio(lexParams, marketState, AssetType.LEVERAGE, AssetType.BASE),
        FixedPoint.WAD
    );
```

The intended behavior is:

- If the market is undercollateralized
- And leverage/aTokens still exist
- Then the aToken price should be zero

However, the implementation checks:

```solidity
marketState.dexAmounts[LVRG] > 0
```

During undercollateralization, the market-state calculation explicitly sets:

```solidity
marketState.dexAmounts[LVRG] = 0;
marketState.underCollateralized = true;
```

Because of this, the zero-price branch may not execute. The code can then fall through to the normal pricing calculation and return a positive aToken price.

## Impact

During undercollateralization, aTokens should be treated as having zero value when debt holders own the remaining collateral value.

The current check can incorrectly report a positive aToken price, which may misrepresent the value of leverage tokens to users, integrators, frontends, or other systems relying on the protocol’s reported token prices.

This does not directly drain funds, but it can lead to incorrect price reporting and misleading protocol state representation.

## Root Cause

The root cause is using `dexAmounts[LVRG]` to determine whether leverage/aTokens exist.

This value is not reliable for this check because it is set to zero when the market is undercollateralized.

The correct existence check should use the aToken supply:

```solidity
marketState.supplyAmounts[LVRG] > 0
```

## Recommended Mitigation

Replace the `dexAmounts[LVRG]` check with `supplyAmounts[LVRG]`.

Suggested fix:

```solidity
tokenPrices.aTokenPrice = (marketState.underCollateralized && marketState.supplyAmounts[LVRG] > 0)
    ? 0
    : tokenPrices.baseTokenPrice.mulDiv(
        _calcRatio(lexParams, marketState, AssetType.LEVERAGE, AssetType.BASE),
        FixedPoint.WAD
    );
```

This ensures that the pricing logic checks whether aTokens actually exist, instead of checking the DEX-side amount that is intentionally zeroed during undercollateralization.

## References

- [Official Code4rena Report](https://code4rena.com/reports/2025-10-covenant)
- [Code4rena Contest Page](https://code4rena.com/audits/2025-10-covenant)
- [Original Submission S-554](https://code4rena.com/audits/2025-10-covenant/submissions/S-554)
- [Affected Code: LatentSwapLogic.sol](https://github.com/code-423n4/2025-10-covenant/blob/main/src/lex/latentswap/libraries/LatentSwapLogic.sol#L1195)
