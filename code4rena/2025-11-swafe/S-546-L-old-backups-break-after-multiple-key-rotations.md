# S-546: Old backups break after multiple key rotations

## Metadata

| Field | Details |
|---|---|
| Project | Swafe |
| Platform | Code4rena |
| Severity | Low |
| Status | Valid — Sufficient |
| Code4rena Handle | CoMManDO |
| Submission ID | S-546 |
| Public Report Mapping | Low / Individual Submission |
| Contest Date | 18 Nov 2025 — 9 Dec 2025 |
| Report Date | 4 May 2026 |
| Ecosystem / Stack | Rust, Partisia, Cryptographic Account Recovery |
| Affected File | `lib/src/account/v0.rs` |
| Affected Functions | `decrypt_share()`, `new_pke()` |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-11-swafe) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-11-swafe) |
| Original Submission | [S-546](https://code4rena.com/audits/2025-11-swafe/submissions/S-546) |
| Affected Code | [v0.rs#L572](https://github.com/code-423n4/2025-11-swafe/blob/main/lib/src/account/v0.rs#L572), [v0.rs#L492](https://github.com/code-423n4/2025-11-swafe/blob/main/lib/src/account/v0.rs#L492) |

## Summary

`decrypt_share()` only attempted decryption with the current key and the last rotated key.

After two or more key rotations, backups encrypted with older keys could no longer be decrypted, even though those keys were still stored in `old_pke`.

## Vulnerability Details

The decryption logic was:

```rust
fn decrypt_share<A: Tagged>(&self, aad: &A, backup: &BackupCiphertext) -> Option<SecretShare> {
    fn decrypt_v0<A: Tagged>(
        v0: &BackupCiphertextV0,
        aad: &A,
        pke: &crate::crypto::pke::DecryptionKey,
    ) -> Option<SecretShare> {
        let (data, index) = pke
            .decrypt_batch::<BackupShareV0, _>(
                &v0.encap,
                &EncryptionContext {
                    aad: (A::SEPARATOR, aad),
                    data: &v0.data,
                    comms: &v0.comms,
                },
            )
            .ok()?;

        Some(SecretShare::V0(DecryptedShareV0 {
            idx: index as u32,
            share: data,
        }))
    }

    match backup {
        BackupCiphertext::V0(v0) => {
            if let Some(share) = decrypt_v0(v0, aad, &self.pke) {
                return Some(share);
            }
            self.old_pke.last().and_then(|old| decrypt_v0(v0, aad, old))
        }
    }
}
```

The key rotation logic stored old keys:

```rust
pub fn new_pke<R: Rng + CryptoRng>(&mut self, rng: &mut R) {
    self.dirty = true;
    self.old_pke.push(self.pke.clone());
    self.pke = pke::DecryptionKey::gen(rng);
}
```

After two rotations:

```text
old_pke = [pke0, pke1]
self.pke = pke2
```

But `decrypt_share()` only tries:

```text
pke2
pke1
```

It never tries:

```text
pke0
```

So a backup encrypted with `pke0` becomes unrecoverable.

## Impact

Backups encrypted under older keys can become permanently unrecoverable after multiple key rotations.

This affects:

- Backup recovery
- Social recovery
- Long-lived accounts
- Guardians or users who rotate encryption keys more than once

The issue is especially dangerous because the system still stores the old keys, giving the impression that historical backups remain supported.

## Root Cause

The root cause is only using:

```rust
self.old_pke.last()
```

instead of iterating through the full `old_pke` key history.

## Recommended Mitigation

Try all historical keys.

Suggested fix:

```rust
match backup {
    BackupCiphertext::V0(v0) => {
        if let Some(share) = decrypt_v0(v0, aad, &self.pke) {
            return Some(share);
        }

        for old in self.old_pke.iter().rev() {
            if let Some(share) = decrypt_v0(v0, aad, old) {
                return Some(share);
            }
        }

        None
    }
}
```

This preserves compatibility with backups encrypted under any stored historical decryption key.

## References

- [Official Code4rena Report](https://code4rena.com/reports/2025-11-swafe)
- [Code4rena Contest Page](https://code4rena.com/audits/2025-11-swafe)
- [Original Submission S-546](https://code4rena.com/audits/2025-11-swafe/submissions/S-546)
- [Affected Code: decrypt_share](https://github.com/code-423n4/2025-11-swafe/blob/main/lib/src/account/v0.rs#L572)
- [Affected Code: new_pke](https://github.com/code-423n4/2025-11-swafe/blob/main/lib/src/account/v0.rs#L492)
