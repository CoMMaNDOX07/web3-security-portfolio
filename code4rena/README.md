# Code4rena Findings

This folder contains my validated findings from Code4rena competitive audits.

I use this section to document my Code4rena security research work, including High, Medium, Low, and QA findings where disclosure is allowed.

## Summary

| Severity | Count |
|---|---:|
| High | 2 |
| Medium | 6 |
| Low / QA | 10 |
| Total | 18 |

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
| 2025-11 | Sequence: Transaction Rails | EVM / Solidity / Multichain | Low | [S-112: Events misreport deposits for rebasing tokens](./2025-11-sequence-transaction-rails/S-112-L-events-misreport-deposits-for-rebasing-tokens.md) | [Code4rena Report](https://code4rena.com/reports/2025-11-sequence-transaction-rails), [Submission](https://code4rena.com/audits/2025-11-sequence-transaction-rails/submissions/S-112) |
| 2025-11 | Sequence: Transaction Rails | EVM / Solidity / Multichain | Low | [S-108: injectSweepAndCall miscalculates received tokens, causing reverts](./2025-11-sequence-transaction-rails/S-108-L-injectsweepandcall-miscalculates-received-tokens-causing-reverts.md) | [Code4rena Report](https://code4rena.com/reports/2025-11-sequence-transaction-rails), [Submission](https://code4rena.com/audits/2025-11-sequence-transaction-rails/submissions/S-108) |
| 2025-11 | Sequence: Transaction Rails | EVM / Solidity / Multichain | Low | [S-104: Public injectAndCall allows full router asset drain](./2025-11-sequence-transaction-rails/S-104-L-public-injectandcall-allows-full-router-asset-drain.md) | [Code4rena Report](https://code4rena.com/reports/2025-11-sequence-transaction-rails), [Submission](https://code4rena.com/audits/2025-11-sequence-transaction-rails/submissions/S-104) |
| 2025-11 | Sequence: Transaction Rails | EVM / Solidity / Multichain | Low | [S-102: Unrestricted ERC20 sweep allows router balance drain](./2025-11-sequence-transaction-rails/S-102-L-unrestricted-erc20-sweep-allows-router-balance-drain.md) | [Code4rena Report](https://code4rena.com/reports/2025-11-sequence-transaction-rails), [Submission](https://code4rena.com/audits/2025-11-sequence-transaction-rails/submissions/S-102) |
| 2025-11 | Swafe | Rust / Partisia / Account Recovery | Medium | [S-545: recover_backups returns stored backups, ignores marked recoveries](./2025-11-swafe/S-545-M-recover-backups-returns-stored-backups-ignores-marked-recoveries.md) | [Code4rena Report](https://code4rena.com/reports/2025-11-swafe), [Submission](https://code4rena.com/audits/2025-11-swafe/submissions/S-545) |
| 2025-11 | Swafe | Rust / Partisia / Account Recovery | Low | [S-548: Threshold uses unverified shares, breaking recovery correctness](./2025-11-swafe/S-548-L-threshold-uses-unverified-shares-breaking-recovery-correctness.md) | [Code4rena Report](https://code4rena.com/reports/2025-11-swafe), [Submission](https://code4rena.com/audits/2025-11-swafe/submissions/S-548) |
| 2025-11 | Swafe | Rust / Partisia / Account Recovery | Low | [S-547: Old backups undecryptable after multiple key rotations](./2025-11-swafe/S-547-L-old-backups-undecryptable-after-multiple-key-rotations.md) | [Code4rena Report](https://code4rena.com/reports/2025-11-swafe), [Submission](https://code4rena.com/audits/2025-11-swafe/submissions/S-547) |
| 2025-11 | Swafe | Rust / Partisia / Account Recovery | Low | [S-546: Old backups break after multiple key rotations](./2025-11-swafe/S-546-L-old-backups-break-after-multiple-key-rotations.md) | [Code4rena Report](https://code4rena.com/reports/2025-11-swafe), [Submission](https://code4rena.com/audits/2025-11-swafe/submissions/S-546) |
| 2025-11 | Swafe | Rust / Partisia / Account Recovery | Low | [S-538: Unauthorized email association overwrite allows recovery hijack](./2025-11-swafe/S-538-L-unauthorized-email-association-overwrite-allows-recovery-hijack.md) | [Code4rena Report](https://code4rena.com/reports/2025-11-swafe), [Submission](https://code4rena.com/audits/2025-11-swafe/submissions/S-538) |
| 2025-11 | Ekubo | EVM / Solidity / AMM / Oracle | Medium | [S-1065: Unrestricted expandCapacity overwrites oracle storage, permanently bricking](./2025-11-ekubo/S-1065-M-unrestricted-expandcapacity-overwrites-oracle-storage-permanently-bricking.md) | [Code4rena Report](https://code4rena.com/reports/2025-11-ekubo), [Submission](https://code4rena.com/audits/2025-11-ekubo/submissions/S-1065) |
## Notes

All findings listed here are based on public Code4rena reports or information I am allowed to disclose.

Some valid submissions may map to the same public Code4rena issue group because Code4rena groups duplicate or related findings in the final report. I count valid submissions in this portfolio and also include public report mappings inside each project folder.
