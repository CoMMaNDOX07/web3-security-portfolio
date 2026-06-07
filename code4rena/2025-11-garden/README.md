# Garden — Code4rena Audit

## Overview

| Field | Details |
|---|---|
| Platform | Code4rena |
| Project | Garden |
| Contest Date | 24 Nov 2025 — 8 Dec 2025 |
| Report Date | 19 Feb 2026 |
| Ecosystem / Stack | Multichain, EVM, Solidity, Bitcoin Bridge |
| Code4rena Handle | CoMManDO |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-11-garden) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-11-garden) |
| Scope Repository | [code-423n4/2025-11-garden](https://github.com/code-423n4/2025-11-garden) |

## My Valid Submissions

| Submission | Severity | Title | Status | Public Report Mapping | Link |
|---|---|---|---|---|---|
| S-156 | Low | transfer to contracts can permanently lock ETH | Valid | Low / Individual Submission | [View](./S-156-L-transfer-to-contracts-can-permanently-lock-eth.md) |

## Summary

During this audit, I submitted a valid Low severity finding related to ETH recovery in Unique Deposit Address contracts.

The issue was caused by using Solidity’s `.transfer()` to send ETH to an arbitrary refund address. Since `.transfer()` forwards only 2300 gas, refunds to smart contract wallets or multisigs with non-trivial receive logic can fail permanently.

This creates a situation where accidentally sent ETH may become unrecoverable from the UDA contracts.

## Notes

- The official public Code4rena report is used as the main public reference.
- The individual Code4rena submission page may require login.
- This finding is documented as a valid Low severity submission.
