# Covenant — Code4rena Audit

## Overview

| Field | Details |
|---|---|
| Platform | Code4rena |
| Project | Covenant |
| Contest Date | 22 Oct 2025 — 3 Nov 2025 |
| Report Date | 17 Nov 2025 |
| Ecosystem / Stack | Solidity, DeFi, leverage and yield markets |
| Code4rena Handle | CoMManDO |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-10-covenant) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-10-covenant) |
| Original Submission | [S-554](https://code4rena.com/audits/2025-10-covenant/submissions/S-554) |
| Scope Repository | [code-423n4/2025-10-covenant](https://github.com/code-423n4/2025-10-covenant) |

## My Findings

| ID | Severity | Title | Status | Link |
|---|---|---|---|---|
| L-01 | Low | Undercollateralized aToken may be incorrectly priced as positive | Valid — Primary — Sufficient — Fixed | [View Finding](./L-01-undercollateralized-atoken-may-be-incorrectly-priced-as-positive.md) |

## Summary

During this audit, I identified a valid Low severity issue related to aToken pricing during undercollateralized market conditions.

The issue was caused by checking `dexAmounts[LVRG] > 0` when deciding whether the aToken price should be zero. However, during undercollateralization, `dexAmounts[LVRG]` is explicitly set to zero earlier in the market-state calculation. As a result, the pricing logic could skip the intended zero-price branch and report a positive aToken price.

## Notes

- This finding was submitted under Code4rena submission `S-554`.
- The public submission page may require Code4rena login.
- The official public report is included as the main external reference.
