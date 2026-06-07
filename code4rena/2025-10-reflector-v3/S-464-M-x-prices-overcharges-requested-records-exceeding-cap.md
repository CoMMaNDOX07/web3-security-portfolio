# S-464: x_prices overcharges requested records exceeding cap

## Metadata

| Field | Details |
|---|---|
| Project | Reflector V3 |
| Platform | Code4rena |
| Severity | Medium |
| Status | Valid — Sufficient — Fixed |
| Code4rena Handle | CoMManDO |
| Submission ID | S-464 |
| Public Report Mapping | M-01 |
| Contest Date | 27 Oct 2025 — 11 Nov 2025 |
| Report Date | 2 Feb 2026 |
| Ecosystem / Stack | Stellar, Soroban, Rust, DeFi Oracle |
| Affected Files | `beam-contract/src/lib.rs`, `oracle/src/prices.rs` |
| Affected Function | `x_prices()` |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-10-reflector-v3) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-10-reflector-v3) |
| Original Submission | [S-464](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-464) |
| Affected Code | [lib.rs#L270](https://github.com/code-423n4/2025-10-reflector/blob/main/beam-contract/src/lib.rs#L270), [prices.rs#L211](https://github.com/code-423n4/2025-10-reflector/blob/main/oracle/src/prices.rs#L211) |

## Summary

The `x_prices()` function charged users for the number of records requested, not the number of records that could actually be returned.

Because returned records were capped downstream, callers could be overcharged when requesting more than 20 records.

## Vulnerability Details

The `x_prices()` function charged using the raw `records` value:

```rust
pub fn x_prices(
    e: &Env,
    caller: Address,
    base_asset: Asset,
    quote_asset: Asset,
    records: u32,
) -> Option<Vec<PriceData>> {
    caller.require_auth();
    charge_invocation_fee(e, &caller, InvocationComplexity::CrossPrice, records);
    PriceOracleContractBase::x_prices(e, base_asset, quote_asset, records)
}
```

The downstream loading logic capped the number of returned records:

```rust
let mut prices = Vec::new(e);
let resolution = settings::get_resolution(e) as u64;

// limit the number of returned records to 20
records = records.min(20);
```

This meant the fee could scale with a large requested number even though the returned data was capped.

## Impact

A caller requesting a large number of cross-price records could be charged far more than the value of the returned data.

Example:

- Caller requests 500 records
- Contract charges for 500 records
- Oracle can return at most 20 records

This causes systematic user overcharging.

## Root Cause

The root cause was charging based on requested records instead of the effective capped record count.

The fee calculation and returned data limit were inconsistent.

## Recommended Mitigation

Clamp the charged record count before calling `charge_invocation_fee()`.

Suggested fix:

```rust
let records_to_charge = records.min(20);

charge_invocation_fee(
    e,
    &caller,
    InvocationComplexity::CrossPrice,
    records_to_charge
);
```

Alternatively, calculate the fee based on the actual returned vector length.

## References

- [Official Code4rena Report](https://code4rena.com/reports/2025-10-reflector-v3)
- [Code4rena Contest Page](https://code4rena.com/audits/2025-10-reflector-v3)
- [Original Submission S-464](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-464)
- [Affected Code: lib.rs#L270](https://github.com/code-423n4/2025-10-reflector/blob/main/beam-contract/src/lib.rs#L270)
- [Record Cap: prices.rs#L211](https://github.com/code-423n4/2025-10-reflector/blob/main/oracle/src/prices.rs#L211)
