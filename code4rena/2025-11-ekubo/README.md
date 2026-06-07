# Ekubo — Code4rena Audit

## Overview

| Field | Details |
|---|---|
| Platform | Code4rena |
| Project | Ekubo |
| Contest Date | 19 Nov 2025 — 10 Dec 2025 |
| Report Date | 12 Jan 2026 |
| Ecosystem / Stack | EVM, Solidity, AMM, Oracle |
| Code4rena Handle | CoMManDO |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-11-ekubo) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-11-ekubo) |
| Scope Repository | [code-423n4/2025-11-ekubo](https://github.com/code-423n4/2025-11-ekubo) |

## My Valid Submissions

| Submission | Severity | Title | Status | Public Report Mapping | Link |
|---|---|---|---|---|---|
| S-1065 | Medium | Unrestricted expandCapacity overwrites oracle storage, permanently bricking | Valid — Sufficient — Fixed | M-01 | [View](./S-1065-M-unrestricted-expandcapacity-overwrites-oracle-storage-permanently-bricking.md) |

## Public Report Mapping

| Public Report ID | Title | Related My Submission |
|---|---|---|
| M-01 | Oracle Data Corruption via Storage Key Collision | S-1065 |

## Summary

During this audit, I submitted a valid Medium severity finding related to the oracle extension’s manual storage layout.

The issue was caused by unsafe raw storage slot derivation in the oracle, where `Counts` metadata and snapshot storage could collide. In my submission, I focused on how unrestricted `expandCapacity()` could write to storage slots derived from user-controlled input and corrupt oracle state for affected tokens.

This could lead to corrupted oracle data and denial-of-service behavior for affected pools or oracle consumers.

## Notes

- The official public Code4rena report is used as the main public reference.
- The individual Code4rena submission page may require login.
- This finding maps to public report issue `M-01`.
