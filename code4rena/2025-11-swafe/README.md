# Swafe — Code4rena Audit

## Overview

| Field | Details |
|---|---|
| Platform | Code4rena |
| Project | Swafe |
| Contest Date | 18 Nov 2025 — 9 Dec 2025 |
| Report Date | 4 May 2026 |
| Ecosystem / Stack | Rust, Partisia, Cryptographic Account Recovery |
| Code4rena Handle | CoMManDO |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-11-swafe) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-11-swafe) |
| Scope Repository | [code-423n4/2025-11-swafe](https://github.com/code-423n4/2025-11-swafe) |

## My Valid Submissions

| Submission | Severity | Title | Status | Public Report Mapping | Link |
|---|---|---|---|---|---|
| S-545 | Medium | recover_backups returns stored backups, ignores marked recoveries | Valid — Sufficient | M-02 | [View](./S-545-M-recover-backups-returns-stored-backups-ignores-marked-recoveries.md) |
| S-548 | Low | Threshold uses unverified shares, breaking recovery correctness | Valid | Low / Individual Submission | [View](./S-548-L-threshold-uses-unverified-shares-breaking-recovery-correctness.md) |
| S-547 | Low | Old backups undecryptable after multiple key rotations | Valid — Sufficient | Low / Individual Submission | [View](./S-547-L-old-backups-undecryptable-after-multiple-key-rotations.md) |
| S-546 | Low | Old backups break after multiple key rotations | Valid — Sufficient | Low / Individual Submission | [View](./S-546-L-old-backups-break-after-multiple-key-rotations.md) |
| S-538 | Low | Unauthorized email association overwrite allows recovery hijack | Valid — Sufficient | Low / Individual Submission | [View](./S-538-L-unauthorized-email-association-overwrite-allows-recovery-hijack.md) |

## Summary

During this audit, I submitted 5 valid findings related to:

- Recovery backup list correctness
- Shamir threshold enforcement after share verification
- Backup decryption after multiple encryption key rotations
- Email association ownership and overwrite safety
- Account recovery correctness and liveness

The main Medium-severity issue was related to `recover_backups()` returning the wrong backup list after backups were marked for recovery.

## Notes

- The official public Code4rena report is used as the main public reference.
- Individual Code4rena submission pages may require login.
- Some Low submissions are documented as individual valid submissions rather than unique public Medium issue groups.
- S-547 and S-546 are closely related key-rotation findings, but both are kept here because they were separate valid submissions.
