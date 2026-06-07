# Code4rena Findings

This folder contains my validated findings from Code4rena competitive audits.

I use this section to document my Code4rena security research work, including High, Medium, Low, and QA findings where disclosure is allowed.

## Summary

| Severity | Count |
|---|---:|
| High | 2 |
| Medium | 4 |
| Low / QA | 2 |
| Total | 8 |

## Findings

| Date | Project | Ecosystem | Severity | Finding | Official Report |
|---|---|---|---|---|---|
| 2025-10 | Covenant | Solidity / DeFi | Low | [L-01: Undercollateralized aToken may be incorrectly priced as positive](./2025-10-covenant/L-01-undercollateralized-atoken-may-be-incorrectly-priced-as-positive.md) | [Code4rena Report](https://code4rena.com/reports/2025-10-covenant) |
| 2025-10 | Reflector V3 | Stellar / Soroban / Rust | High | [S-458: Missing admin check allows unauthorized cost reconfiguration](./2025-10-reflector-v3/S-458-H-missing-admin-check-allows-unauthorized-cost-reconfiguration.md) | [Code4rena Report](https://code4rena.com/reports/2025-10-reflector-v3), [Submission](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-458) |
| 2025-10 | Reflector V3 | Stellar / Soroban / Rust | High | [S-453: Missing admin check allows unauthorized fee changes](./2025-10-reflector-v3/S-453-H-missing-admin-check-allows-unauthorized-fee-changes.md) | [Code4rena Report](https://code4rena.com/reports/2025-10-reflector-v3), [Submission](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-453) |
| 2025-10 | Reflector V3 | Stellar / Soroban / Rust | Medium | [S-457: Incorrect TWAP fee scaling allows underpayment exploitation](./2025-10-reflector-v3/S-457-M-incorrect-twap-fee-scaling-allows-underpayment-exploitation.md) | [Code4rena Report](https://code4rena.com/reports/2025-10-reflector-v3), [Submission](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-457) |
| 2025-10 | Reflector V3 | Stellar / Soroban / Rust | Medium | [S-459: Unbounded fee scaling causes user overcharge risk](./2025-10-reflector-v3/S-459-M-unbounded-fee-scaling-causes-user-overcharge-risk.md) | [Code4rena Report](https://code4rena.com/reports/2025-10-reflector-v3), [Submission](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-459) |
| 2025-10 | Reflector V3 | Stellar / Soroban / Rust | Medium | [S-463: Overcharging occurs because records exceed capped limit](./2025-10-reflector-v3/S-463-M-overcharging-occurs-because-records-exceed-capped-limit.md) | [Code4rena Report](https://code4rena.com/reports/2025-10-reflector-v3), [Submission](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-463) |
| 2025-10 | Reflector V3 | Stellar / Soroban / Rust | Medium | [S-464: x_prices overcharges requested records exceeding cap](./2025-10-reflector-v3/S-464-M-x-prices-overcharges-requested-records-exceeding-cap.md) | [Code4rena Report](https://code4rena.com/reports/2025-10-reflector-v3), [Submission](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-464) |
| 2025-10 | Reflector V3 | Stellar / Soroban / Rust | Low | [S-462: Divisor scaling causes zero panic and incorrect floor](./2025-10-reflector-v3/S-462-L-divisor-scaling-causes-zero-panic-and-incorrect-floor.md) | [Code4rena Report](https://code4rena.com/reports/2025-10-reflector-v3), [Submission](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-462) |
## Notes

All findings listed here are based on public Code4rena reports or information I am allowed to disclose.

Some valid submissions may map to the same public Code4rena issue group because Code4rena groups duplicate or related findings in the final report. I count valid submissions in this portfolio and also include public report mappings inside each project folder.
