# Sequence: Transaction Rails — Code4rena Audit

## Overview

| Field | Details |
|---|---|
| Platform | Code4rena |
| Project | Sequence: Transaction Rails |
| Contest Date | 11 Nov 2025 — 17 Nov 2025 |
| Report Date | 17 Dec 2025 |
| Ecosystem / Stack | EVM, Solidity, Multichain Transaction Rails |
| Code4rena Handle | CoMManDO |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-11-sequence-transaction-rails) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-11-sequence-transaction-rails) |
| Scope Repository | [code-423n4/2025-11-sequence](https://github.com/code-423n4/2025-11-sequence) |

## My Valid Submissions

| Submission | Severity | Title | Status | Link |
|---|---|---|---|---|
| S-112 | Low | Events misreport deposits for rebasing tokens | Valid | [View](./S-112-L-events-misreport-deposits-for-rebasing-tokens.md) |
| S-108 | Low | injectSweepAndCall miscalculates received tokens, causing reverts | Valid | [View](./S-108-L-injectsweepandcall-miscalculates-received-tokens-causing-reverts.md) |
| S-104 | Low | Public injectAndCall allows full router asset drain | Valid | [View](./S-104-L-public-injectandcall-allows-full-router-asset-drain.md) |
| S-102 | Low | Unrestricted ERC20 sweep allows router balance drain | Valid | [View](./S-102-L-unrestricted-erc20-sweep-allows-router-balance-drain.md) |

## Summary

During this audit, I submitted 4 valid Low severity findings related to:

- Incorrect event accounting for non-standard ERC20 tokens
- Fee-on-transfer / rebasing token balance mismatches
- Public router functions spending router-held balances
- Leftover ERC20 balances being drainable through unrestricted router flows

The findings mainly focused on asset accounting, router balance safety, and unsafe assumptions around ERC20 transfer behavior.

## Notes

- The official public Code4rena report is used as the main public reference.
- Individual Code4rena submission pages may require login.
- These findings are documented as valid Low severity submissions.
