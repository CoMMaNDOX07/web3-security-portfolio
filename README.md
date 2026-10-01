# CoMMaNDO

**Web3 Security Researcher - Smart Contracts & Blockchain Protocols**

I research security issues in smart contracts and blockchain systems, with work spanning Solidity/EVM, Rust-based protocols, and TON. This portfolio brings together my competitive audit submissions and responsible disclosure work.

**Open to Web3 Security Researcher and Smart Contract Security roles.**

[Selected findings](#selected-public-findings) · [Code4rena archive](./code4rena/README.md) · [Responsible disclosure](./responsible-disclosures/README.md) · [LinkedIn](https://www.linkedin.com/in/hamza-nasser-3a35a8246/)

## Results at a glance

| Research track | Recorded outcome | Portfolio |
|---|---|---|
| Code4rena competitive audits | **20 valid submissions** across **7 contests**: 2 High, 7 Medium, 11 Low | [Browse submissions](./code4rena/README.md) |
| TON responsible disclosure | **12 validated and rewarded reports**; details kept private | [Research record](./responsible-disclosures/ton/README.md) |
| HackenProof — Aptos Network | **3 duplicate reports**, tracked separately | [Additional research](./hackenproof/aptos-network/README.md) |


## Selected public findings

The official reports below credit **CoMMaNDO** among the researchers who also found these issues. These are selected issue groups, not extra findings added to the totals above.

| Project | Severity | Research contribution | Evidence |
|---|---|---|---|
| Reflector V3 | High | Missing authorization on invocation-fee configuration | [My write-up](./code4rena/2025-10-reflector-v3/S-453-H-missing-admin-check-allows-unauthorized-fee-changes.md) · [Official report: H-01](https://code4rena.com/reports/2025-10-reflector-v3) |
| Reflector V3 | Medium | Incorrect fee scaling for multi-period TWAP queries | [My write-up](./code4rena/2025-10-reflector-v3/S-457-M-incorrect-twap-fee-scaling-allows-underpayment-exploitation.md) · [Official report: M-04](https://code4rena.com/reports/2025-10-reflector-v3) |
| Swafe | Medium | Recovery logic reads the wrong backup collection | [My write-up](./code4rena/2025-11-swafe/S-545-M-recover-backups-returns-stored-backups-ignores-marked-recoveries.md) · [Official report: M-02](https://code4rena.com/reports/2025-11-swafe) |
| Ekubo | Medium | Overlapping oracle storage namespaces | [My write-up](./code4rena/2025-11-ekubo/S-1065-M-unrestricted-expandcapacity-overwrites-oracle-storage-permanently-bricking.md) · [Official report: M-01](https://code4rena.com/reports/2025-11-ekubo) |

## Competitive audit archive

The project pages preserve my individual submissions, recorded outcomes, and available public references. Participation in a competitive audit is not a claim to have conducted the project's entire audit independently.

| Contest month | Project | Reviewed ecosystem | Valid submissions |
|---|---|---|---:|
| 2026-03 | [Chainlink Payment Abstraction V2](./code4rena/2026-03-chainlink-payment-abstraction-v2/README.md) | Solidity / EVM | 1 |
| 2025-11 | [Garden](./code4rena/2025-11-garden/README.md) | Solidity / EVM | 1 |
| 2025-11 | [Ekubo](./code4rena/2025-11-ekubo/README.md) | Solidity / EVM | 1 |
| 2025-11 | [Swafe](./code4rena/2025-11-swafe/README.md) | Rust / Partisia | 5 |
| 2025-11 | [Sequence: Transaction Rails](./code4rena/2025-11-sequence-transaction-rails/README.md) | Solidity / EVM | 4 |
| 2025-10 | [Reflector V3](./code4rena/2025-10-reflector-v3/README.md) | Rust / Stellar / Soroban | 7 |
| 2025-10 | [Covenant](./code4rena/2025-10-covenant/README.md) | Solidity / EVM | 1 |

## Research focus

| Area | Examples in this portfolio |
|---|---|
| Smart contract authorization and asset accounting | Reflector V3, Sequence, Covenant |
| Oracle pricing, fees, and storage correctness | Reflector V3, Ekubo |
| Recovery state and key-management logic | Swafe |
| Blockchain protocol security | TON responsible disclosure research; report-level details withheld |

## Contact

For Web3 security research roles and professional enquiries, contact me on [LinkedIn](https://www.linkedin.com/in/hamza-nasser-3a35a8246/).

**Research identities:** Code4rena: `CoMMaNDO` · GitHub: `CoMMaNDOX07` · HackenProof: `CoMManDOO`.

## Evidence and disclosure

Public write-ups link to original reports or submissions where available; some submission pages require login. Private evidence is shared only where the relevant program permits it. See [counting, attribution, and disclosure notes](./DISCLOSURE.md).
