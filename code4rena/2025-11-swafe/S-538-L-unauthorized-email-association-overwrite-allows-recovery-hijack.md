# S-538: Unauthorized email association overwrite allows recovery hijack

## Metadata

| Field | Details |
|---|---|
| Project | Swafe |
| Platform | Code4rena |
| Severity | Low |
| Status | Valid — Sufficient |
| Code4rena Handle | CoMManDO |
| Submission ID | S-538 |
| Public Report Mapping | Low / Individual Submission |
| Contest Date | 18 Nov 2025 — 9 Dec 2025 |
| Report Date | 4 May 2026 |
| Ecosystem / Stack | Rust, Partisia, Cryptographic Account Recovery |
| Affected File | `contracts/src/http/endpoints/association/upload_msk.rs` |
| Affected Function | `upload_msk` handler |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-11-swafe) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-11-swafe) |
| Original Submission | [S-538](https://code4rena.com/audits/2025-11-swafe/submissions/S-538) |
| Affected Code | [upload_msk.rs#L60](https://github.com/code-423n4/2025-11-swafe/blob/main/contracts/src/http/endpoints/association/upload_msk.rs#L60) |

## Summary

The `upload_msk` handler overwrote existing MSK associations for the same email tag without checking whether the existing association belonged to the same user or whether a valid rotation flow was authorized.

This could allow an attacker who obtains a valid email certificate and threshold VDRF evaluation for the same email to overwrite the original user’s recovery association.

## Vulnerability Details

The handler derives an `EmailKey` from the email and VDRF evaluation:

```rust
let email: EmailInput = email.parse()?;
let email_tag: EmailKey = EmailKey::new(&vdrf_pk, &email, request.vdrf_eval.0)?;
```

It then stores the MSK association under that deterministic `email_tag`:

```rust
MskRecordCollection::store(
    &mut ctx,
    email_tag,
    request.association.0.verify(user_pk, &node_id)?,
);
```

The issue is that the store operation acts as an upsert without checking whether an association already exists for the same `email_tag`.

There is no validation such as:

```text
Does this email_tag already exist?
If it exists, does it belong to the same user_pk?
If it belongs to another user, is there a valid rotation authorization?
```

As a result, a new upload can overwrite an existing association for the same email tag.

## Attack Scenario

An attacker must be able to obtain:

- A valid `EmailCert` for the target email
- The corresponding threshold VDRF evaluation for that email

This could happen through temporary mailbox compromise, reused email addresses, provider compromise, or another failure in the email proof process.

Given those inputs, the attacker can call `upload_msk` and overwrite the association for the same `EmailKey`.

## Impact

The previous user’s recovery association can be overwritten.

This can cause:

- Recovery denial of service for the original user
- Recovery linkage hijacking
- Replacement of the user’s association with attacker-controlled data
- Broken assumption that an email remains bound to the original owner unless explicitly rotated

This impacts account recovery correctness and user safety.

## Root Cause

The root cause is blindly overwriting records keyed by `EmailKey`.

The implementation does not enforce:

- First-write-wins behavior
- Existing owner check
- Secure rotation authorization
- Conflict response on unauthorized overwrite

## Recommended Mitigation

Use one or more of the following patterns.

### Option 1: First-write-wins

Reject the store operation if an entry already exists for the same `email_tag`.

Return an error such as:

```text
409 Conflict
```

### Option 2: Ownership check

If an entry already exists, require the existing owner to match:

```text
existing.fixed.user_pk == user_pk
```

Reject replacement if the current owner does not match the uploading user.

### Option 3: Secure rotation flow

If an entry already exists, require a rotation signature from the current owner before replacing it.

The rotation proof should authorize the new association data and prevent unauthorized replacement.

## References

- [Official Code4rena Report](https://code4rena.com/reports/2025-11-swafe)
- [Code4rena Contest Page](https://code4rena.com/audits/2025-11-swafe)
- [Original Submission S-538](https://code4rena.com/audits/2025-11-swafe/submissions/S-538)
- [Affected Code: upload_msk.rs#L60](https://github.com/code-423n4/2025-11-swafe/blob/main/contracts/src/http/endpoints/association/upload_msk.rs#L60)
