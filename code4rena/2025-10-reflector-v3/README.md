# Reflector V3 — Code4rena Audit

## Overview

| Field | Details |
|---|---|
| Platform | Code4rena |
| Project | Reflector V3 |
| Contest Date | 27 Oct 2025 — 11 Nov 2025 |
| Report Date | 2 Feb 2026 |
| Ecosystem / Stack | Stellar, Soroban, Rust, DeFi Oracle |
| Code4rena Handle | CoMManDO |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-10-reflector-v3) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-10-reflector-v3) |
| Scope Repository | [code-423n4/2025-10-reflector](https://github.com/code-423n4/2025-10-reflector) |

## My Valid Submissions

| Submission | Severity | Title | Status | Public Report Mapping | Link |
|---|---|---|---|---|---|
| S-458 | High | Missing admin check allows unauthorized cost reconfiguration | Valid — Sufficient — Fixed | H-01 | [View](./S-458-H-missing-admin-check-allows-unauthorized-cost-reconfiguration.md) |
| S-453 | High | Missing admin check allows unauthorized fee changes | Valid — Sufficient — Fixed | H-01 | [View](./S-453-H-missing-admin-check-allows-unauthorized-fee-changes.md) |
| S-457 | Medium | Incorrect TWAP fee scaling allows underpayment exploitation | Valid — Sufficient — Fixed | M-04 | [View](./S-457-M-incorrect-twap-fee-scaling-allows-underpayment-exploitation.md) |
| S-459 | Medium | Unbounded fee scaling causes user overcharge risk | Valid — Sufficient — Fixed | M-01 | [View](./S-459-M-unbounded-fee-scaling-causes-user-overcharge-risk.md) |
| S-463 | Medium | Overcharging occurs because records exceed capped limit | Valid — Sufficient — Fixed | M-01 | [View](./S-463-M-overcharging-occurs-because-records-exceed-capped-limit.md) |
| S-464 | Medium | x_prices overcharges requested records exceeding cap | Valid — Sufficient — Fixed | M-01 | [View](./S-464-M-x-prices-overcharges-requested-records-exceeding-cap.md) |
| S-462 | Low | Divisor scaling causes zero panic and incorrect floor | Valid — Sufficient — Fixed | Low / Individual Submission | [View](./S-462-L-divisor-scaling-causes-zero-panic-and-incorrect-floor.md) |

## Public Report Mapping

The official Code4rena report grouped related submissions into public issue groups:

| Public Report ID | Title | Related My Submissions |
|---|---|---|
| H-01 | `set_invocation_costs_config()` fails to authorize admin allowing anyone to set invocation costs | S-458, S-453 |
| M-01 | Systematic overcharge in `prices` and `x_prices`: fee charged for requested records while return is capped at 20 | S-459, S-463, S-464 |
| M-04 | `twap()` under-charges for multi-period queries due to hardcoded `periods = 1` | S-457 |
| Low / Individual | `fixed_div_floor()` divisor scaling can cause zero panic and incorrect floor semantics | S-462 |

## Summary

During this audit, I submitted multiple valid findings related to:

- Missing admin authorization on invocation cost configuration
- Incorrect fee charging logic for oracle price queries
- Undercharging for TWAP calculations
- Overcharging when requested records exceed the protocol cap
- Integer division edge cases in fixed-point math

Some submissions were grouped together in the final public Code4rena report because they shared the same root cause or affected the same vulnerability class.

## Notes

- The official public report is used as the main public reference.
- Individual Code4rena submission links may require login.
- All findings documented here are based on public report information and my own submitted reports.
