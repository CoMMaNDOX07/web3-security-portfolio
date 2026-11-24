# CoMMaNDO

### Blockchain Protocol & Smart Contract Security Researcher

I am an independent security researcher specializing in vulnerability research across blockchain implementations, virtual machines, smart contract compilers, and decentralized protocols.

My research spans Ethereum execution and consensus clients, TON Blockchain core, Solidity/EVM smart contracts, Rust-based protocols, and cryptographic systems.

[![Ethereum](https://img.shields.io/badge/Ethereum-Security_Research-627EEA?style=flat-square&logo=ethereum&logoColor=white)](./responsible-disclosures/ethereum/README.md)
[![TON](https://img.shields.io/badge/TON-Security_Research-0098EA?style=flat-square)](./responsible-disclosures/ton/README.md)
[![Code4rena](https://img.shields.io/badge/Code4rena-Competitive_Audits-6C47FF?style=flat-square)](./code4rena/README.md)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Contact-0077B5?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/hamza-nasser-3a35a8246/)

**Open to opportunities in Blockchain Protocol Security, Web3 Security Research, and Smart Contract Auditing.**

[Research Highlights](#research-highlights) · [Bug Bounties](#blockchain-security-research) · [Public Findings](#selected-public-findings) · [Audit Archive](#competitive-audit-archive) · [Contact](#contact)

---

## Research Highlights

| 40 | 14 | 7 |
|:---:|:---:|:---:|
| Validated Reports & Submissions | Rewarded Bug Bounty Reports | Competitive Security Audits |

- **Ethereum Foundation:** Official bug bounty leaderboard recognition and 2 rewarded Erigon security reports.
- **Ethereum Ecosystem:** 8 validated security reports across Erigon, Vyper, and Teku.
- **TON Blockchain:** 12 privately validated and rewarded security reports.
- **Code4rena:** 20 valid submissions across 7 competitive security audits.
- **Research Coverage:** Blockchain clients, virtual machines, compilers, decentralized finance, and cryptographic systems.

Validated report and submission counts include related submissions and previously reported vulnerabilities. They do not necessarily represent unique vulnerabilities.

---

## Blockchain Security Research

| Project | Research Domain | Technology | Validated Reports | Portfolio |
|---|---|---|---:|---|
| **TON Blockchain** | Blockchain core and virtual machine security | C++ / TVM | 12 | [Research](./responsible-disclosures/ton/README.md) |
| **Erigon** | Ethereum execution-layer security | Go | 5 | [Research](./responsible-disclosures/ethereum/README.md) |
| **Vyper** | Smart contract compiler security | Python / Vyper | 2 | [Research](./responsible-disclosures/ethereum/README.md) |
| **Teku** | Ethereum consensus-layer security | Java | 1 | [Research](./responsible-disclosures/ethereum/README.md) |
| **Total** | | | **20** | |

### Rewarded Bug Bounty Research

| Program | Rewarded Reports | References |
|---|---:|---|
| Ethereum Foundation — Erigon | 2 | [Official Recognition](https://github.com/ethereum/ethereum-org-website/pull/19745) |
| TON Security Bug Bounty | 12 | [Research Record](./responsible-disclosures/ton/README.md) |
| **Total** | **14** | |

### Ethereum Foundation Recognition

Publicly recognized in Ethereum's official execution-layer bug bounty leaderboard under the researcher identity **CoMMaNDO**.

[View Official Ethereum Bug Bounty Recognition](https://github.com/ethereum/ethereum-org-website/pull/19745)

Individual report validation and reward outcomes are based on private bounty records.

[Full Responsible Disclosure Portfolio](./responsible-disclosures/README.md)

---

## Competitive Security Audits

| Platform | High | Medium | Low | Valid Submissions |
|---|---:|---:|---:|---:|
| Code4rena | 2 | 7 | 11 | **20** |

[Complete Code4rena Submission Archive](./code4rena/README.md)

## Selected Public Findings

The following official audit reports credit CoMMaNDO among the researchers who identified these issues.

| Project | Severity | Security Finding | References |
|---|---|---|---|
| Reflector V3 | **High** | Missing authorization on invocation-fee configuration | [Write-up](./code4rena/2025-10-reflector-v3/S-453-H-missing-admin-check-allows-unauthorized-fee-changes.md) · [Official Report H-01](https://code4rena.com/reports/2025-10-reflector-v3) |
| Reflector V3 | **Medium** | Incorrect fee accounting for capped oracle records | [Write-up](./code4rena/2025-10-reflector-v3/S-459-M-unbounded-fee-scaling-causes-user-overcharge-risk.md) · [Official Report M-01](https://code4rena.com/reports/2025-10-reflector-v3) |
| Reflector V3 | **Medium** | Incorrect TWAP fee scaling | [Write-up](./code4rena/2025-10-reflector-v3/S-457-M-incorrect-twap-fee-scaling-allows-underpayment-exploitation.md) · [Official Report M-04](https://code4rena.com/reports/2025-10-reflector-v3) |
| Ekubo | **Medium** | Oracle storage-key collision | [Write-up](./code4rena/2025-11-ekubo/S-1065-M-unrestricted-expandcapacity-overwrites-oracle-storage-permanently-bricking.md) · [Official Report M-01](https://code4rena.com/reports/2025-11-ekubo) |
| Swafe | **Medium** | Incorrect recovery backup selection | [Write-up](./code4rena/2025-11-swafe/S-545-M-recover-backups-returns-stored-backups-ignores-marked-recoveries.md) · [Official Report M-02](https://code4rena.com/reports/2025-11-swafe) |

## Competitive Audit Archive

| Project | Ecosystem | High | Medium | Low | Portfolio |
|---|---|---:|---:|---:|---|
| Chainlink Payment Abstraction V2 | Solidity / EVM | 0 | 1 | 0 | [View](./code4rena/2026-03-chainlink-payment-abstraction-v2/README.md) |
| Ekubo | Solidity / EVM | 0 | 1 | 0 | [View](./code4rena/2025-11-ekubo/README.md) |
| Garden | Bitcoin Bridge / EVM | 0 | 0 | 1 | [View](./code4rena/2025-11-garden/README.md) |
| Swafe | Rust / Partisia | 0 | 1 | 4 | [View](./code4rena/2025-11-swafe/README.md) |
| Sequence: Transaction Rails | Solidity / EVM | 0 | 0 | 4 | [View](./code4rena/2025-11-sequence-transaction-rails/README.md) |
| Reflector V3 | Rust / Stellar / Soroban | 2 | 4 | 1 | [View](./code4rena/2025-10-reflector-v3/README.md) |
| Covenant | Solidity / EVM | 0 | 0 | 1 | [View](./code4rena/2025-10-covenant/README.md) |
| **Total** | | **2** | **7** | **11** | **20** |

Counts represent valid individual submissions. Related submissions may correspond to the same vulnerability in an official audit report.

---

## Technical Research Experience

### Blockchain Infrastructure & Execution Environments

| Research Domain | Codebases & Technologies |
|---|---|
| Execution-layer client security | Erigon / Go |
| Consensus-layer client security | Teku / Java |
| Blockchain core security | TON / C++ |
| Virtual machine security | TON Virtual Machine (TVM) |
| Smart contract compiler security | Vyper / Python |
| Layer-1 blockchain security | Aptos Network |

### Smart Contracts & Decentralized Protocols

| Research Domain | Research Coverage |
|---|---|
| Smart Contract Security | Solidity / EVM |
| Rust-based Smart Contracts | Stellar / Soroban |
| DeFi Security | AMMs, asset accounting, fee mechanisms |
| Oracle Security | Pricing, storage correctness, TWAP |
| Cross-chain Security | Bridges and transaction routing |
| Cryptographic Systems | Secret sharing, key rotation, account recovery |
| Protocol Logic | State transitions, authorization, economic invariants |

[Additional HackenProof Research](./hackenproof/README.md)

Additional Aptos research is recorded separately and is not included in the headline validated or rewarded report counts.

---

## Research Methodology

- Manual source code review and vulnerability analysis.
- Blockchain protocol and state-transition analysis.
- Security invariant and business logic analysis.
- Virtual machine and compiler correctness research.
- Cross-component security analysis.
- Proof-of-concept development in controlled environments.
- Root cause analysis and remediation recommendations.
- Responsible vulnerability disclosure.

---

## Research Profiles

| Platform | Researcher Identity |
|---|---|
| GitHub | [CoMMaNDOX07](https://github.com/CoMMaNDOX07) |
| Code4rena | CoMMaNDO |
| Ethereum Foundation Bug Bounty | [Official Recognition](https://github.com/ethereum/ethereum-org-website/pull/19745) |
| HackenProof | CoMManDOO |
| LinkedIn | [Professional Profile](https://www.linkedin.com/in/hamza-nasser-3a35a8246/) |

---

## Responsible Disclosure

This portfolio documents publicly disclosed security research and aggregate records of confidential research where publication is permitted.

Private vulnerability titles, technical details, severity classifications, proofs of concept, and reward amounts are withheld where required by applicable disclosure terms.

Further professional verification is subject to the relevant program's authorization.

[Evidence, Attribution & Disclosure Policy](./DISCLOSURE.md)

## Contact

Open to professional opportunities in blockchain protocol security, Web3 security research, blockchain security engineering, and smart contract auditing.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/hamza-nasser-3a35a8246/)
