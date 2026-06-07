# S-463: Overcharging occurs because records exceed capped limit

## Metadata

| Field | Details |
|---|---|
| Project | Reflector V3 |
| Platform | Code4rena |
| Severity | Medium |
| Status | Valid — Sufficient — Fixed |
| Code4rena Handle | CoMManDO |
| Submission ID | S-463 |
| Public Report Mapping | M-01 |
| Contest Date | 27 Oct 2025 — 11 Nov 2025 |
| Report Date | 2 Feb 2026 |
| Ecosystem / Stack | Stellar, Soroban, Rust, DeFi Oracle |
| Affected Files | `beam-contract/src/lib.rs`, `oracle/src/prices.rs` |
| Affected Function | `prices()` |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-10-reflector-v3) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-10-reflector-v3) |
| Original Submission | [S-463](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-463) |
| Affected Code | [lib.rs#L173](https://github.com/code-423n4/2025-10-reflector/blob/main/beam-contract/src/lib.rs#L173), [prices.rs#L214](https://github.com/code-423n4/2025-10-reflector/blob/main/oracle/src/prices.rs#L214) |

## Summary

The `prices()` function charged users based on the requested number of records, but the underlying oracle capped returned records to 20.

As a result, callers could pay for more records than they could ever receive.

## Vulnerability Details

The `prices()` function charged according to the user-provided `records` value:

```rust
pub fn prices(e: &Env, caller: Address, asset: Asset, records: u32) -> Option<Vec<PriceData>> {
    caller.require_auth();
    charge_invocation_fee(e, &caller, InvocationComplexity::Price, records);
    PriceOracleContractBase::prices(e, asset, records)
}
```

However, the downstream price loading logic capped records:

```rust
// limit the number of returned records to 20
records = records.min(20);
```

This created a billing mismatch:

- Fee calculation used the requested number of records
- Oracle response returned at most 20 records

## Impact

Users could be charged for records that would never be returned.

For example:

- User requests 100 records
- Fee is calculated for 100 records
- Oracle returns at most 20 records

The extra fee is paid without receiving additional data, causing systematic overcharging.

## Root Cause

The root cause was charging based on requested records instead of chargeable records.

The chargeable record count should have matched the protocol’s maximum returned record count.

## Recommended Mitigation

Charge for `min(records, 20)` or charge based on the actual number of returned records.

Suggested fix:

```rust
let records_to_charge = records.min(20);

charge_invocation_fee(
    e,
    &caller,
    InvocationComplexity::Price,
    records_to_charge
);
```

## References

- [Official Code4rena Report](https://code4rena.com/reports/2025-10-reflector-v3)
- [Code4rena Contest Page](https://code4rena.com/audits/2025-10-reflector-v3)
- [Original Submission S-463](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-463)
- [Affected Code: lib.rs#L173](https://github.com/code-423n4/2025-10-reflector/blob/main/beam-contract/src/lib.rs#L173)
- [Record Cap: prices.rs#L214](https://github.com/code-423n4/2025-10-reflector/blob/main/oracle/src/prices.rs#L214)
