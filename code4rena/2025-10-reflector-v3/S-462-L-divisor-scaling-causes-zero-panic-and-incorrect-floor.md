# S-462: Divisor scaling causes zero panic and incorrect floor

## Metadata

| Field | Details |
|---|---|
| Project | Reflector V3 |
| Platform | Code4rena |
| Severity | Low |
| Status | Valid — Sufficient — Fixed |
| Code4rena Handle | CoMManDO |
| Submission ID | S-462 |
| Public Report Mapping | Low / Individual Submission |
| Contest Date | 27 Oct 2025 — 11 Nov 2025 |
| Report Date | 2 Feb 2026 |
| Ecosystem / Stack | Stellar, Soroban, Rust, DeFi Oracle |
| Affected File | `oracle/src/prices.rs` |
| Affected Function | `fixed_div_floor()` |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-10-reflector-v3) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-10-reflector-v3) |
| Original Submission | [S-462](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-462) |
| Affected Code | [prices.rs#L328](https://github.com/code-423n4/2025-10-reflector/blob/main/oracle/src/prices.rs#L328) |

## Summary

The `fixed_div_floor()` function scaled the divisor down by a power of ten.

This could reduce the divisor to zero and cause a division-by-zero panic. Even when it did not panic, it changed the intended floor division semantics.

## Vulnerability Details

The vulnerable logic was:

```rust
pub fn fixed_div_floor(dividend: i128, divisor: i128, decimals: u32) -> i128 {
    if dividend <= 0 || divisor <= 0 {
        panic!("invalid division arguments")
    }

    let ashift = core::cmp::min(38 - dividend.ilog10(), decimals);
    let bshift = core::cmp::max(decimals - ashift, 0);

    let mut vdividend = dividend;
    let mut vdivisor = divisor;

    if ashift > 0 {
        vdividend *= 10_i128.pow(ashift);
    }

    if bshift > 0 {
        vdivisor /= 10_i128.pow(bshift);
    }

    vdividend / vdivisor
}
```

The issue is caused by this divisor downscaling:

```rust
vdivisor /= 10_i128.pow(bshift);
```

If `divisor < 10^bshift`, then `vdivisor` becomes zero. The final division then panics.

Additionally, integer division after downscaling the divisor is not equivalent to computing:

```text
floor((dividend * 10^decimals) / divisor)
```

This can produce incorrect floor results.

## Impact

The issue could lead to:

- Division-by-zero panic
- Unexpected transaction failure
- Incorrect fixed-point division output
- Biased or inaccurate price calculations

Because this helper is part of oracle pricing math, incorrect floor semantics can lead to inaccurate calculated values.

## Root Cause

The root cause was scaling down the divisor instead of safely scaling the numerator or using a wider/safe intermediate calculation.

Downscaling the divisor can destroy precision and can reduce the divisor to zero.

## Recommended Mitigation

Do not downscale the divisor.

Safer approaches include:

- Scaling only the numerator in safe chunks
- Using checked arithmetic
- Using a wider intermediate representation
- Reworking the calculation to preserve floor semantics without reducing the divisor to zero

The function should preserve the intended result:

```text
floor((dividend * 10^decimals) / divisor)
```

without allowing the divisor to become zero.

## References

- [Official Code4rena Report](https://code4rena.com/reports/2025-10-reflector-v3)
- [Code4rena Contest Page](https://code4rena.com/audits/2025-10-reflector-v3)
- [Original Submission S-462](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-462)
- [Affected Code: prices.rs#L328](https://github.com/code-423n4/2025-10-reflector/blob/main/oracle/src/prices.rs#L328)
