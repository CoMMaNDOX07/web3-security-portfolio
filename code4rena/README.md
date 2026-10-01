# Code4rena — Competitive Audit Submissions

[Back to portfolio](../README.md)

This archive records **20 valid submissions across 7 contests**, under the handle **CoMMaNDO**. Figures reflect the outcomes recorded in my submission records.

## Submission totals

| Final severity | Valid submissions |
|---|---:|
| High | 2 |
| Medium | 7 |
| Low | 11 |
| **Total** | **20** |


## Project index

| Month | Project | High | Medium | Low | Total | Public reference |
|---|---|---:|---:|---:|---:|---|
| 2026-03 | [Chainlink Payment Abstraction V2](./2026-03-chainlink-payment-abstraction-v2/README.md) | 0 | 1 | 0 | 1 | [Audit page](https://code4rena.com/audits/2026-03-chainlink-payment-abstraction-v2) |
| 2025-11 | [Garden](./2025-11-garden/README.md) | 0 | 0 | 1 | 1 | [Final report](https://code4rena.com/reports/2025-11-garden) |
| 2025-11 | [Ekubo](./2025-11-ekubo/README.md) | 0 | 1 | 0 | 1 | [Final report](https://code4rena.com/reports/2025-11-ekubo) |
| 2025-11 | [Swafe](./2025-11-swafe/README.md) | 0 | 1 | 4 | 5 | [Final report](https://code4rena.com/reports/2025-11-swafe) |
| 2025-11 | [Sequence: Transaction Rails](./2025-11-sequence-transaction-rails/README.md) | 0 | 0 | 4 | 4 | [Final report](https://code4rena.com/reports/2025-11-sequence-transaction-rails) |
| 2025-10 | [Reflector V3](./2025-10-reflector-v3/README.md) | 2 | 4 | 1 | 7 | [Final report](https://code4rena.com/reports/2025-10-reflector-v3) |
| 2025-10 | [Covenant](./2025-10-covenant/README.md) | 0 | 0 | 1 | 1 | [Final report](https://code4rena.com/reports/2025-10-covenant) |

## Individual submissions

The original submission IDs and existing filenames are preserved. Display titles below are shortened for navigation; original report titles remain in the individual write-ups.

<details>
<summary><strong>Expand all 20 submission entries</strong></summary>

| Project | Submission | Severity | Portfolio entry |
|---|---|---|---|
| Chainlink Payment Abstraction V2 | Private entry | Medium | [Confidential Medium submission](./2026-03-chainlink-payment-abstraction-v2/M-01-private-medium-finding-details-withheld.md) |
| Garden | S-156 | Low | [ETH recovery compatibility with contract recipients](./2025-11-garden/S-156-L-transfer-to-contracts-can-permanently-lock-eth.md) |
| Ekubo | S-1065 | Medium | [Oracle storage-key collision](./2025-11-ekubo/S-1065-M-unrestricted-expandcapacity-overwrites-oracle-storage-permanently-bricking.md) |
| Swafe | S-545 | Medium | [Incorrect recovery-backup selection](./2025-11-swafe/S-545-M-recover-backups-returns-stored-backups-ignores-marked-recoveries.md) |
| Swafe | S-548 | Low | [Threshold checks after share verification](./2025-11-swafe/S-548-L-threshold-uses-unverified-shares-breaking-recovery-correctness.md) |
| Swafe | S-547 | Low | [Historical key lookup after key rotation](./2025-11-swafe/S-547-L-old-backups-undecryptable-after-multiple-key-rotations.md) |
| Swafe | S-546 | Low | [Historical backup decryption after rotation](./2025-11-swafe/S-546-L-old-backups-break-after-multiple-key-rotations.md) |
| Swafe | S-538 | Low | [Email recovery-association replacement](./2025-11-swafe/S-538-L-unauthorized-email-association-overwrite-allows-recovery-hijack.md) |
| Sequence: Transaction Rails | S-112 | Low | [Deposit events and received-token accounting](./2025-11-sequence-transaction-rails/S-112-L-events-misreport-deposits-for-rebasing-tokens.md) |
| Sequence: Transaction Rails | S-108 | Low | [Received-token mismatch in router calls](./2025-11-sequence-transaction-rails/S-108-L-injectsweepandcall-miscalculates-received-tokens-causing-reverts.md) |
| Sequence: Transaction Rails | S-104 | Low | [Public access to router-held balances](./2025-11-sequence-transaction-rails/S-104-L-public-injectandcall-allows-full-router-asset-drain.md) |
| Sequence: Transaction Rails | S-102 | Low | [Unspent router balances and sweep behavior](./2025-11-sequence-transaction-rails/S-102-L-unrestricted-erc20-sweep-allows-router-balance-drain.md) |
| Reflector V3 | S-458 | High | [Missing authorization on cost configuration](./2025-10-reflector-v3/S-458-H-missing-admin-check-allows-unauthorized-cost-reconfiguration.md) |
| Reflector V3 | S-453 | High | [Missing authorization on fee configuration](./2025-10-reflector-v3/S-453-H-missing-admin-check-allows-unauthorized-fee-changes.md) |
| Reflector V3 | S-457 | Medium | [TWAP query fee undercharging](./2025-10-reflector-v3/S-457-M-incorrect-twap-fee-scaling-allows-underpayment-exploitation.md) |
| Reflector V3 | S-459 | Medium | [Fee scaling exceeds the record limit](./2025-10-reflector-v3/S-459-M-unbounded-fee-scaling-causes-user-overcharge-risk.md) |
| Reflector V3 | S-463 | Medium | [Price-query fees exceed capped results](./2025-10-reflector-v3/S-463-M-overcharging-occurs-because-records-exceed-capped-limit.md) |
| Reflector V3 | S-464 | Medium | [Cross-price fees exceed capped results](./2025-10-reflector-v3/S-464-M-x-prices-overcharges-requested-records-exceeding-cap.md) |
| Reflector V3 | S-462 | Low | [Fixed-point divisor scaling correctness](./2025-10-reflector-v3/S-462-L-divisor-scaling-causes-zero-panic-and-incorrect-floor.md) |
| Covenant | S-554 | Low | [aToken pricing during undercollateralization](./2025-10-covenant/L-01-undercollateralized-atoken-may-be-incorrectly-priced-as-positive.md) |

</details>

## Attribution and related submissions

For Reflector V3, S-458 and S-453 concern the same public H-01 issue. S-459, S-463, and S-464 are documented under the M-01 overcharging issue. S-457 maps to M-04. They remain separate submission records, not separate unique issues within those groups.

For Swafe, S-547 and S-546 describe the same historical-key lookup behavior and are retained as separate submission records without a uniqueness claim.

The Chainlink Medium entry is confidential. Its existing filename contains a local portfolio label, not a verified official public issue number.

Public report attribution, private submission status, remediation status, and independently reproduced test results are different types of evidence. See [portfolio evidence policy](../DISCLOSURE.md).
