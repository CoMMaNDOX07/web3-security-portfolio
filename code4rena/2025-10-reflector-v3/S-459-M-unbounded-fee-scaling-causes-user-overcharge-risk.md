# S-459: Unbounded fee scaling causes user overcharge risk

## Metadata

| Field | Details |
|---|---|
| Project | Reflector V3 |
| Platform | Code4rena |
| Severity | Medium |
| Status | Valid — Sufficient — Fixed |
| Code4rena Handle | CoMManDO |
| Submission ID | S-459 |
| Public Report Mapping | M-01 |
| Contest Date | 27 Oct 2025 — 11 Nov 2025 |
| Report Date | 2 Feb 2026 |
| Ecosystem / Stack | Stellar, Soroban, Rust, DeFi Oracle |
| Affected Files | `beam-contract/src/cost.rs`, `oracle/src/prices.rs` |
| Affected Logic | Invocation fee scaling and capped record loading |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-10-reflector-v3) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-10-reflector-v3) |
| Original Submission | [S-459](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-459) |
| Affected Code | [cost.rs#L84](https://github.com/code-423n4/2025-10-reflector/blob/main/beam-contract/src/cost.rs#L84), [prices.rs#L214](https://github.com/code-423n4/2025-10-reflector/blob/main/oracle/src/prices.rs#L214) |

## Summary

The fee calculation scaled with the raw `periods` value supplied by the caller, while the underlying oracle data loader capped returned records to 20.

This mismatch could cause users to be charged for more records than the protocol could actually return.

## Vulnerability Details

The fee calculation scaled based on the `periods` input:

```rust
// charge additional per each loaded period
if periods > 1 {
    let period_modifier = costs
        .get(InvocationComplexity::NModifier as u32)
        .unwrap_or_default() as i128;

    if period_modifier > 0 {
        cost = cost * (SCALE + (periods - 1) as i128 * period_modifier) / SCALE;
    }
}
```

However, the underlying price loading logic capped returned records:

```rust
// limit the number of returned records to 20
records = records.min(20);
```

This means a caller could request a large number of records and be charged based on that large number, while the contract would only process or return up to 20 records.

## Impact

Users could be overcharged when requesting more than 20 records.

For example:

- A user requests 500 records
- The fee calculation charges for 500 periods
- The oracle returns at most 20 records

This results in unfair and systematic overcharging.

## Root Cause

The root cause was inconsistent treatment of the `records` / `periods` value.

The fee calculation used the raw caller-provided value, while the data retrieval logic silently capped the value to 20.

## Recommended Mitigation

Clamp the value used for fee calculation to the same limit used by the price loading logic.

Suggested fix:

```rust
let periods = periods.min(20);
```

The protocol should ensure that users are charged only for the maximum number of records that can actually be returned.

## References

- [Official Code4rena Report](https://code4rena.com/reports/2025-10-reflector-v3)
- [Code4rena Contest Page](https://code4rena.com/audits/2025-10-reflector-v3)
- [Original Submission S-459](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-459)
- [Affected Code: cost.rs#L84](https://github.com/code-423n4/2025-10-reflector/blob/main/beam-contract/src/cost.rs#L84)
- [Record Cap: prices.rs#L214](https://github.com/code-423n4/2025-10-reflector/blob/main/oracle/src/prices.rs#L214)
