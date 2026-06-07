# S-547: Old backups undecryptable after multiple key rotations

## Metadata

| Field | Details |
|---|---|
| Project | Swafe |
| Platform | Code4rena |
| Severity | Low |
| Status | Valid — Sufficient |
| Code4rena Handle | CoMManDO |
| Submission ID | S-547 |
| Public Report Mapping | Low / Individual Submission |
| Contest Date | 18 Nov 2025 — 9 Dec 2025 |
| Report Date | 4 May 2026 |
| Ecosystem / Stack | Rust, Partisia, Cryptographic Account Recovery |
| Affected File | `lib/src/account/v0.rs` |
| Affected Functions | `new_pke()`, `decrypt_share()` |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-11-swafe) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-11-swafe) |
| Original Submission | [S-547](https://code4rena.com/audits/2025-11-swafe/submissions/S-547) |
| Affected Code | [v0.rs#L491](https://github.com/code-423n4/2025-11-swafe/blob/main/lib/src/account/v0.rs#L491), [v0.rs#L572](https://github.com/code-423n4/2025-11-swafe/blob/main/lib/src/account/v0.rs#L572) |

## Summary

`new_pke()` stored a full history of previous decryption keys in `old_pke`, but `decrypt_share()` only tried the current key and the most recent old key.

After two or more key rotations, backups encrypted under older keys could become permanently undecryptable, even though the needed keys were still stored.

## Vulnerability Details

Key rotation stored the current key before generating a new one:

```rust
pub fn new_pke<R: Rng + CryptoRng>(&mut self, rng: &mut R) {
    self.dirty = true;
    self.old_pke.push(self.pke.clone());
    self.pke = pke::DecryptionKey::gen(rng);
}
```

After several rotations, the key state can look like this:

```text
After first rotation:
self.pke = K1
old_pke = [K0]

After second rotation:
self.pke = K2
old_pke = [K0, K1]

After third rotation:
self.pke = K3
old_pke = [K0, K1, K2]
```

This suggests the system intends to keep historical decryption keys.

However, `decrypt_share()` only tries the current key and `old_pke.last()`:

```rust
match backup {
    BackupCiphertext::V0(v0) => {
        if let Some(share) = decrypt_v0(v0, aad, &self.pke) {
            return Some(share);
        }
        self.old_pke.last().and_then(|old| decrypt_v0(v0, aad, old))
    }
}
```

This means backups encrypted with keys older than the most recent previous key are never attempted.

Example:

```text
Backup encrypted with K0
Rotations: K0 -> K1 -> K2

Current state:
self.pke = K2
old_pke = [K0, K1]

decrypt_share tries:
K2 -> fails
K1 -> fails
K0 -> never tried
```

The backup becomes undecryptable even though `K0` still exists in `old_pke`.

## Impact

This can cause data loss in backup and recovery flows.

Affected scenarios include:

- Backups encrypted before multiple key rotations
- Guardians rotating their encryption keys more than once
- Social recovery depending on older encrypted shares
- Users expecting historical backups to remain decryptable

The system appears to retain all old keys, but the decryption logic only supports current plus one previous key.

## Root Cause

The root cause is using:

```rust
self.old_pke.last()
```

instead of iterating through all historical keys.

The storage model keeps a vector of old keys, but the decryption logic only checks the latest old key.

## Recommended Mitigation

Try all stored keys, preferably from newest to oldest.

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

This aligns behavior with the `old_pke: Vec<_>` data model and preserves decryptability for backups whose keys are still stored.

## References

- [Official Code4rena Report](https://code4rena.com/reports/2025-11-swafe)
- [Code4rena Contest Page](https://code4rena.com/audits/2025-11-swafe)
- [Original Submission S-547](https://code4rena.com/audits/2025-11-swafe/submissions/S-547)
- [Affected Code: new_pke](https://github.com/code-423n4/2025-11-swafe/blob/main/lib/src/account/v0.rs#L491)
- [Affected Code: decrypt_share](https://github.com/code-423n4/2025-11-swafe/blob/main/lib/src/account/v0.rs#L572)
