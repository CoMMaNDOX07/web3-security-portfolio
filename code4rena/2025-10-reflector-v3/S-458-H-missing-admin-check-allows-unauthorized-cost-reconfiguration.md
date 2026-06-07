# S-458: Missing admin check allows unauthorized cost reconfiguration

## Metadata

| Field | Details |
|---|---|
| Project | Reflector V3 |
| Platform | Code4rena |
| Severity | High |
| Status | Valid — Sufficient — Fixed |
| Code4rena Handle | CoMManDO |
| Submission ID | S-458 |
| Public Report Mapping | H-01 |
| Contest Date | 27 Oct 2025 — 11 Nov 2025 |
| Report Date | 2 Feb 2026 |
| Ecosystem / Stack | Stellar, Soroban, Rust, DeFi Oracle |
| Affected File | `beam-contract/src/lib.rs` |
| Affected Function | `set_invocation_costs_config()` |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-10-reflector-v3) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-10-reflector-v3) |
| Original Submission | [S-458](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-458) |
| Affected Code | [lib.rs#L404](https://github.com/code-423n4/2025-10-reflector/blob/main/beam-contract/src/lib.rs#L404) |

## Summary

The `set_invocation_costs_config()` function allowed invocation cost configuration to be changed without checking that the caller was the admin.

The function comment stated that admin authorization was required, but the implementation directly called `set_costs_config()` without enforcing the admin check.

## Vulnerability Details

The vulnerable function was:

```rust
// Update costs configuration per each invocation category
// Requires admin authorization
pub fn set_invocation_costs_config(e: &Env, config: Vec<u64>) {
    set_costs_config(e, &config);
}
```

The issue is that the function updates sensitive pricing configuration but does not call the admin authorization guard before writing the new config.

Because of this, any caller could invoke the function and modify the cost configuration.

## Impact

An unauthorized caller could arbitrarily reconfigure invocation costs.

This could allow:

- Setting invocation costs to zero
- Disabling or weakening fee collection
- Setting costs to extremely high values
- Causing denial of service for users who rely on oracle reads
- Breaking the intended economic model of the oracle access layer

Because the affected configuration controls how callers are charged for oracle invocations, unauthorized access to this function directly breaks fee enforcement.

## Root Cause

The root cause was missing access control in `set_invocation_costs_config()`.

The function should have enforced admin authorization before calling:

```rust
set_costs_config(e, &config);
```

## Recommended Mitigation

Add the admin authorization check before updating the invocation cost configuration.

Suggested fix:

```rust
use oracle::auth;

pub fn set_invocation_costs_config(e: &Env, config: Vec<u64>) {
    auth::panic_if_not_admin(e);
    set_costs_config(e, &config);
}
```

## References

- [Official Code4rena Report](https://code4rena.com/reports/2025-10-reflector-v3)
- [Code4rena Contest Page](https://code4rena.com/audits/2025-10-reflector-v3)
- [Original Submission S-458](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-458)
- [Affected Code: lib.rs#L404](https://github.com/code-423n4/2025-10-reflector/blob/main/beam-contract/src/lib.rs#L404)
