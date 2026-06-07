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
| Code4rena | 2 | 7 | 11 | 20 |
| Sherlock | 0 | 0 | 0 | 0 |
| Cantina | 0 | 0 | 0 | 0 |
| hackenproof | 0 | 0 | 0 | 0 |
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
| 2025-11 | Code4rena | Sequence: Transaction Rails | EVM / Solidity / Multichain | Low | [S-112: Events misreport deposits for rebasing tokens](./code4rena/2025-11-sequence-transaction-rails/S-112-L-events-misreport-deposits-for-rebasing-tokens.md) | [Official Report](https://code4rena.com/reports/2025-11-sequence-transaction-rails), [Submission S-112](https://code4rena.com/audits/2025-11-sequence-transaction-rails/submissions/S-112) |
| 2025-11 | Code4rena | Sequence: Transaction Rails | EVM / Solidity / Multichain | Low | [S-108: injectSweepAndCall miscalculates received tokens, causing reverts](./code4rena/2025-11-sequence-transaction-rails/S-108-L-injectsweepandcall-miscalculates-received-tokens-causing-reverts.md) | [Official Report](https://code4rena.com/reports/2025-11-sequence-transaction-rails), [Submission S-108](https://code4rena.com/audits/2025-11-sequence-transaction-rails/submissions/S-108) |
| 2025-11 | Code4rena | Sequence: Transaction Rails | EVM / Solidity / Multichain | Low | [S-104: Public injectAndCall allows full router asset drain](./code4rena/2025-11-sequence-transaction-rails/S-104-L-public-injectandcall-allows-full-router-asset-drain.md) | [Official Report](https://code4rena.com/reports/2025-11-sequence-transaction-rails), [Submission S-104](https://code4rena.com/audits/2025-11-sequence-transaction-rails/submissions/S-104) |
| 2025-11 | Code4rena | Sequence: Transaction Rails | EVM / Solidity / Multichain | Low | [S-102: Unrestricted ERC20 sweep allows router balance drain](./code4rena/2025-11-sequence-transaction-rails/S-102-L-unrestricted-erc20-sweep-allows-router-balance-drain.md) | [Official Report](https://code4rena.com/reports/2025-11-sequence-transaction-rails), [Submission S-102](https://code4rena.com/audits/2025-11-sequence-transaction-rails/submissions/S-102) |
| 2025-11 | Code4rena | Swafe | Rust / Partisia / Account Recovery | Medium | [S-545: recover_backups returns stored backups, ignores marked recoveries](./code4rena/2025-11-swafe/S-545-M-recover-backups-returns-stored-backups-ignores-marked-recoveries.md) | [Official Report](https://code4rena.com/reports/2025-11-swafe), [Submission S-545](https://code4rena.com/audits/2025-11-swafe/submissions/S-545) |
| 2025-11 | Code4rena | Swafe | Rust / Partisia / Account Recovery | Low | [S-548: Threshold uses unverified shares, breaking recovery correctness](./code4rena/2025-11-swafe/S-548-L-threshold-uses-unverified-shares-breaking-recovery-correctness.md) | [Official Report](https://code4rena.com/reports/2025-11-swafe), [Submission S-548](https://code4rena.com/audits/2025-11-swafe/submissions/S-548) |
| 2025-11 | Code4rena | Swafe | Rust / Partisia / Account Recovery | Low | [S-547: Old backups undecryptable after multiple key rotations](./code4rena/2025-11-swafe/S-547-L-old-backups-undecryptable-after-multiple-key-rotations.md) | [Official Report](https://code4rena.com/reports/2025-11-swafe), [Submission S-547](https://code4rena.com/audits/2025-11-swafe/submissions/S-547) |
| 2025-11 | Code4rena | Swafe | Rust / Partisia / Account Recovery | Low | [S-546: Old backups break after multiple key rotations](./code4rena/2025-11-swafe/S-546-L-old-backups-break-after-multiple-key-rotations.md) | [Official Report](https://code4rena.com/reports/2025-11-swafe), [Submission S-546](https://code4rena.com/audits/2025-11-swafe/submissions/S-546) |
| 2025-11 | Code4rena | Swafe | Rust / Partisia / Account Recovery | Low | [S-538: Unauthorized email association overwrite allows recovery hijack](./code4rena/2025-11-swafe/S-538-L-unauthorized-email-association-overwrite-allows-recovery-hijack.md) | [Official Report](https://code4rena.com/reports/2025-11-swafe), [Submission S-538](https://code4rena.com/audits/2025-11-swafe/submissions/S-538) |
| 2025-11 | Code4rena | Ekubo | EVM / Solidity / AMM / Oracle | Medium | [S-1065: Unrestricted expandCapacity overwrites oracle storage, permanently bricking](./code4rena/2025-11-ekubo/S-1065-M-unrestricted-expandcapacity-overwrites-oracle-storage-permanently-bricking.md) | [Official Report](https://code4rena.com/reports/2025-11-ekubo), [Submission S-1065](https://code4rena.com/audits/2025-11-ekubo/submissions/S-1065) |
| 2025-11 | Code4rena | Garden | Multichain / EVM / Solidity / Bitcoin Bridge | Low | [S-156: transfer to contracts can permanently lock ETH](./code4rena/2025-11-garden/S-156-L-transfer-to-contracts-can-permanently-lock-eth.md) | [Official Report](https://code4rena.com/reports/2025-11-garden), [Submission S-156](https://code4rena.com/audits/2025-11-garden/submissions/S-156) |
| 2026-03 | Code4rena | Chainlink Payment Abstraction V2 | EVM / Solidity / Chainlink | Medium | [M-01: Medium finding — details withheld](./code4rena/2026-03-chainlink-payment-abstraction-v2/M-01-private-medium-finding-details-withheld.md) | [Official Audit Page](https://code4rena.com/audits/2026-03-chainlink-payment-abstraction-v2) |
| HackenProof | Aptos Network | Aptos / Move / Layer 1 | Duplicate / Also Found | 3 | Details withheld due to program disclosure rules |
## Platforms

This portfolio may include findings from:

- Code4rena
- Sherlock
- Cantina
- hackenproof
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

Some bug bounty reports are listed only as sanitized portfolio entries. Technical details, PoCs, affected code, and vulnerability titles are withheld unless public disclosure is explicitly allowed by the program owner.
