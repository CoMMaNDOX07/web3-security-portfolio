# S-453: Missing admin check allows unauthorized fee changes

## Metadata

| Field | Details |
|---|---|
| Project | Reflector V3 |
| Platform | Code4rena |
| Severity | High |
| Status | Valid — Sufficient — Fixed |
| Code4rena Handle | CoMManDO |
| Submission ID | S-453 |
| Public Report Mapping | H-01 |
| Contest Date | 27 Oct 2025 — 11 Nov 2025 |
| Report Date | 2 Feb 2026 |
| Ecosystem / Stack | Stellar, Soroban, Rust, DeFi Oracle |
| Affected Files | `beam-contract/src/lib.rs`, `oracle/src/auth.rs` |
| Affected Function | `set_invocation_costs_config()` |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-10-reflector-v3) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-10-reflector-v3) |
| Original Submission | [S-453](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-453) |
| Affected Code | [lib.rs#L404](https://github.com/code-423n4/2025-10-reflector/blob/main/beam-contract/src/lib.rs#L404), [auth.rs#L21](https://github.com/code-423n4/2025-10-reflector/blob/main/oracle/src/auth.rs#L21) |

## Summary

The protocol had a correct admin authorization helper, but the sensitive `set_invocation_costs_config()` function did not call it.

As a result, an unauthorized user could change the invocation fee configuration.

## Vulnerability Details

The admin guard was implemented in the oracle authentication module:

```rust
pub fn panic_if_not_admin(e: &Env) {
    let admin = get_admin(e);
    if admin.is_none() {
        panic_with_error!(e, Error::Unauthorized);
    }
    admin.unwrap().require_auth()
}
```

However, the invocation cost configuration function skipped this guard:

```rust
pub fn set_invocation_costs_config(e: &Env, config: Vec<u64>) {
    set_costs_config(e, &config);
}
```

The comment above the function stated that admin authorization was required, but the code did not enforce it.

## Impact

Any caller could change the invocation fee configuration.

This could lead to:

- Fee bypass by setting costs to zero
- Denial of service by setting costs to extremely high values
- Unauthorized manipulation of the oracle billing model
- Broken assumptions for consumers relying on predictable invocation costs

## Root Cause

The root cause was inconsistent use of the existing authorization helper.

The project already had `panic_if_not_admin()`, but `set_invocation_costs_config()` failed to call it before writing new cost configuration.

## Recommended Mitigation

Call the admin authorization guard before updating the configuration.

Suggested fix:

```rust
pub fn set_invocation_costs_config(e: &Env, config: Vec<u64>) {
    oracle::auth::panic_if_not_admin(e);
    set_costs_config(e, &config);
}
```

## References

- [Official Code4rena Report](https://code4rena.com/reports/2025-10-reflector-v3)
- [Code4rena Contest Page](https://code4rena.com/audits/2025-10-reflector-v3)
- [Original Submission S-453](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-453)
- [Affected Code: lib.rs#L404](https://github.com/code-423n4/2025-10-reflector/blob/main/beam-contract/src/lib.rs#L404)
- [Admin Guard: auth.rs#L21](https://github.com/code-423n4/2025-10-reflector/blob/main/oracle/src/auth.rs#L21)
