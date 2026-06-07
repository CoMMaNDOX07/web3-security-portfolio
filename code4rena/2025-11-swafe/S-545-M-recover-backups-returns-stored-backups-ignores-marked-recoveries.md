# S-545: recover_backups returns stored backups, ignores marked recoveries

## Metadata

| Field | Details |
|---|---|
| Project | Swafe |
| Platform | Code4rena |
| Severity | Medium |
| Status | Valid — Sufficient |
| Code4rena Handle | CoMManDO |
| Submission ID | S-545 |
| Public Report Mapping | M-02 |
| Contest Date | 18 Nov 2025 — 9 Dec 2025 |
| Report Date | 4 May 2026 |
| Ecosystem / Stack | Rust, Partisia, Cryptographic Account Recovery |
| Affected File | `lib/src/account/v0.rs` |
| Affected Functions | `recover_backups()`, `mark_recovery()` |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-11-swafe) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-11-swafe) |
| Original Submission | [S-545](https://code4rena.com/audits/2025-11-swafe/submissions/S-545) |
| Affected Code | [v0.rs#L230](https://github.com/code-423n4/2025-11-swafe/blob/main/lib/src/account/v0.rs#L230), [v0.rs#L516](https://github.com/code-423n4/2025-11-swafe/blob/main/lib/src/account/v0.rs#L516), [v0.rs#L246](https://github.com/code-423n4/2025-11-swafe/blob/main/lib/src/account/v0.rs#L246) |

## Summary

`recover_backups()` returned the general stored backup list instead of the list of backups explicitly marked for recovery.

This broke the expected semantics of “recovery backups” and caused recovery logic to operate on the wrong ciphertexts.

## Vulnerability Details

`AccountStateV0` stores backups in two separate vectors:

```rust
pub(crate) struct AccountStateV0 {
    cnt: u32,
    act: AccountCiphertext,
    pub(crate) rec: RecoveryStateV0,
    sig: sig::VerificationKey,
    pke: pke::EncryptionKey,
    backups: Vec<BackupCiphertext>,
    recover: Vec<BackupCiphertext>,
}
```

The two vectors have different meanings:

- `backups`: general stored backups
- `recover`: backups selected for recovery

When a backup is marked for recovery, it is correctly moved from `backups` into `recover`:

```rust
pub fn mark_recovery(&mut self, id: BackupId) -> Result<()> {
    if let Some(index) = self.backups.iter().position(|ct| ct.id() == id) {
        self.dirty = true;
        self.recover.push(self.backups.remove(index));
        Ok(())
    } else {
        Err(SwafeError::BackupNotFound)
    }
}
```

However, the getter for recovery backups ignored `recover` and returned `backups`:

```rust
pub fn recover_backups(&self) -> Vec<&BackupCiphertext> {
    self.backups.iter().collect()
}
```

This means that after a backup is marked for recovery, it is removed from `backups`, but `recover_backups()` still reads from `backups`. As a result, the backup selected for recovery is not returned.

## Impact

Any component relying on `recover_backups()` can operate on the wrong list of backups.

This can cause:

- Backups selected for recovery to be ignored
- Backups not selected for recovery to be shown or processed
- Recovery UI to display incorrect recovery data
- Guardian share upload or recovery flows to fail
- Account recovery liveness to break

At protocol level, this can prevent users from recovering the intended backup, creating an availability and correctness issue.

## Root Cause

The root cause is that `recover_backups()` returns:

```rust
self.backups.iter().collect()
```

instead of:

```rust
self.recover.iter().collect()
```

The function name and recovery flow expect selected recovery backups, but the implementation reads from the general backup pool.

## Recommended Mitigation

Return the `recover` vector instead of `backups`.

Suggested fix:

```rust
pub fn recover_backups(&self) -> Vec<&BackupCiphertext> {
    self.recover.iter().collect()
}
```

Alternatively, change the API to avoid allocation:

```rust
pub fn recover_backups(&self) -> &[BackupCiphertext] {
    &self.recover
}
```

If the intended behavior is to support multiple recovery sources, the function should explicitly search the correct recovery queues and document that behavior.

## References

- [Official Code4rena Report](https://code4rena.com/reports/2025-11-swafe)
- [Code4rena Contest Page](https://code4rena.com/audits/2025-11-swafe)
- [Original Submission S-545](https://code4rena.com/audits/2025-11-swafe/submissions/S-545)
- [Affected Code: AccountStateV0 fields](https://github.com/code-423n4/2025-11-swafe/blob/main/lib/src/account/v0.rs#L230)
- [Affected Code: mark_recovery](https://github.com/code-423n4/2025-11-swafe/blob/main/lib/src/account/v0.rs#L516)
- [Affected Code: recover_backups](https://github.com/code-423n4/2025-11-swafe/blob/main/lib/src/account/v0.rs#L246)
