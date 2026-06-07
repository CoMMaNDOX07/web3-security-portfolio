# My Web3 Security Findings Portfolio

This repository documents my validated smart contract security findings from public Web3 audits, competitive audit contests, and bug bounty platforms.

The purpose of this repository is to keep a professional public record of my security research work, including High, Medium, Low, and QA findings where disclosure is allowed.

## About Me

I am a Web3 security researcher focused on smart contract auditing, blockchain protocols, and decentralized finance security.

- Code4rena handle: `CoMMaNDO`
- Hackenproof handle: `CoMManDOO`
- GitHub: `@CoMMaNDOX07`
- Focus areas:
  - smart contracts
  - DeFi protocols
  - Access control issues
  - Accounting bugs
  - Logic vulnerabilities
  - Oracle and pricing issues
  - Liquidation and collateral mechanisms
  - Reward distribution issues

## Portfolio Summary

| Platform | High | Medium | Low / QA | Total |
|---|---:|---:|---:|---:|
| Code4rena | 2 | 4 | 2 | 8 |
| Sherlock | 0 | 0 | 0 | 0 |
| Cantina | 0 | 0 | 0 | 0 |
| Other | 0 | 0 | 0 | 0 |

## Findings

| Date | Platform | Project | Ecosystem | Severity | Finding | References |
|---|---|---|---|---|---|---|
| 2025-10 | Code4rena | Covenant | Solidity / DeFi | Low | [L-01: Undercollateralized aToken may be incorrectly priced as positive](./code4rena/2025-10-covenant/L-01-undercollateralized-atoken-may-be-incorrectly-priced-as-positive.md) | [Official Report](https://code4rena.com/reports/2025-10-covenant), [Submission](https://code4rena.com/audits/2025-10-covenant/submissions/S-554) |
| 2025-10 | Code4rena | Reflector V3 | Stellar / Soroban / Rust | High | [S-458: Missing admin check allows unauthorized cost reconfiguration](./code4rena/2025-10-reflector-v3/S-458-H-missing-admin-check-allows-unauthorized-cost-reconfiguration.md) | [Official Report](https://code4rena.com/reports/2025-10-reflector-v3), [Submission S-458](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-458) |
| 2025-10 | Code4rena | Reflector V3 | Stellar / Soroban / Rust | High | [S-453: Missing admin check allows unauthorized fee changes](./code4rena/2025-10-reflector-v3/S-453-H-missing-admin-check-allows-unauthorized-fee-changes.md) | [Official Report](https://code4rena.com/reports/2025-10-reflector-v3), [Submission S-453](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-453) |
| 2025-10 | Code4rena | Reflector V3 | Stellar / Soroban / Rust | Medium | [S-457: Incorrect TWAP fee scaling allows underpayment exploitation](./code4rena/2025-10-reflector-v3/S-457-M-incorrect-twap-fee-scaling-allows-underpayment-exploitation.md) | [Official Report](https://code4rena.com/reports/2025-10-reflector-v3), [Submission S-457](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-457) |
| 2025-10 | Code4rena | Reflector V3 | Stellar / Soroban / Rust | Medium | [S-459: Unbounded fee scaling causes user overcharge risk](./code4rena/2025-10-reflector-v3/S-459-M-unbounded-fee-scaling-causes-user-overcharge-risk.md) | [Official Report](https://code4rena.com/reports/2025-10-reflector-v3), [Submission S-459](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-459) |
| 2025-10 | Code4rena | Reflector V3 | Stellar / Soroban / Rust | Medium | [S-463: Overcharging occurs because records exceed capped limit](./code4rena/2025-10-reflector-v3/S-463-M-overcharging-occurs-because-records-exceed-capped-limit.md) | [Official Report](https://code4rena.com/reports/2025-10-reflector-v3), [Submission S-463](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-463) |
| 2025-10 | Code4rena | Reflector V3 | Stellar / Soroban / Rust | Medium | [S-464: x_prices overcharges requested records exceeding cap](./code4rena/2025-10-reflector-v3/S-464-M-x-prices-overcharges-requested-records-exceeding-cap.md) | [Official Report](https://code4rena.com/reports/2025-10-reflector-v3), [Submission S-464](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-464) |
| 2025-10 | Code4rena | Reflector V3 | Stellar / Soroban / Rust | Low | [S-462: Divisor scaling causes zero panic and incorrect floor](./code4rena/2025-10-reflector-v3/S-462-L-divisor-scaling-causes-zero-panic-and-incorrect-floor.md) | [Official Report](https://code4rena.com/reports/2025-10-reflector-v3), [Submission S-462](https://code4rena.com/audits/2025-10-reflector-v3/submissions/S-462) |
## Platforms

This portfolio may include findings from:

- Code4rena
- Sherlock
- Cantina
- Other public or private audit work where disclosure is allowed

## Disclosure Policy

All findings documented in this repository are based on public information, public audit reports, or information I am allowed to disclose.

I do not publish:

- Private sponsor information
- Private judging comments
- Non-public source code
- Confidential audit material
- Reports belonging to other researchers as if they are mine

## Notes

External audit platforms may change, migrate, or become unavailable over time. For that reason, this repository keeps my own summaries and references for long-term portfolio purposes.
