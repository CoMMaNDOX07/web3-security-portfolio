# S-548: Threshold uses unverified shares, breaking recovery correctness

## Metadata

| Field | Details |
|---|---|
| Project | Swafe |
| Platform | Code4rena |
| Severity | Low |
| Status | Valid |
| Code4rena Handle | CoMManDO |
| Submission ID | S-548 |
| Public Report Mapping | Low / Individual Submission |
| Contest Date | 18 Nov 2025 — 9 Dec 2025 |
| Report Date | 4 May 2026 |
| Ecosystem / Stack | Rust, Partisia, Cryptographic Account Recovery |
| Affected File | `lib/src/association/v0.rs` |
| Affected Functions | `threshold()`, `reconstruct_rik_data()`, `reconstruct_recovery_key()` |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-11-swafe) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-11-swafe) |
| Original Submission | [S-548](https://code4rena.com/audits/2025-11-swafe/submissions/S-548) |
| Affected Code | [v0.rs#L151](https://github.com/code-423n4/2025-11-swafe/blob/main/lib/src/association/v0.rs#L151), [v0.rs#L393](https://github.com/code-423n4/2025-11-swafe/blob/main/lib/src/association/v0.rs#L393), [v0.rs#L455](https://github.com/code-423n4/2025-11-swafe/blob/main/lib/src/association/v0.rs#L455) |

## Summary

The threshold getter itself was correct, but reconstruction checked the threshold before verifying shares and did not re-check it after invalid shares were filtered out.

This allowed reconstruction to continue with fewer than the required number of valid shares.

## Vulnerability Details

The threshold getter returned the number of commitments:

```rust
impl MskRecordFixed {
    pub fn threshold(&self) -> usize {
        self.commits.len()
    }
}
```

Commitments were generated consistently according to the threshold:

```rust
fn generate_commitment_values<R: Rng + CryptoRng>(
    rng: &mut R,
    generators: &PedersenGenerators,
    threshold: usize,
) -> Result<(Vec<PedersenCommitment>, Vec<PedersenOpen>), SwafeError> {
    if threshold == 0 {
        return Err(SwafeError::InvalidInput(
            "Threshold must be greater than 0".to_string(),
        ));
    }

    let mut comms = Vec::with_capacity(threshold);
    let mut opens = Vec::with_capacity(threshold);

    for _ in 0..threshold {
        let open = PedersenOpen::gen(rng);
        let comm = generators.commit(&open);
        comms.push(comm);
        opens.push(open);
    }

    Ok((comms, opens))
}
```

The issue was in reconstruction logic.

The code first checked the number of candidate records:

```rust
if v0_records.len() < majority_fixed.threshold() {
    return Err(SwafeError::NotEnoughSharesForReconstruction);
}
```

Then it verified shares and dropped invalid ones:

```rust
let points: Vec<_> = v0_records
    .iter()
    .filter_map(|(node_id, msk_record)| {
        match verify_secret_share(&majority_fixed.commits, &msk_record.share, node_id) {
            Ok(()) => {
                let x = node_id.eval_point();
                let y = msk_record.share.value();
                Some((x, y))
            }
            Err(_) => None,
        }
    })
    .collect();
```

However, there was no second threshold check after verification.

This means reconstruction could proceed even if:

```text
v0_records.len() >= threshold
```

but:

```text
points.len() < threshold
```

## Impact

Reconstruction may run with too few valid Shamir shares.

This can cause:

- Incorrect reconstructed secret values
- Wrong recovery key derivation
- Recovery returning `Ok(...)` even when insufficient valid shares exist
- Callers being unable to distinguish a valid recovered key from garbage output
- Malicious or corrupted nodes causing recovery to “succeed” incorrectly

This breaks the expected Shamir guarantee: fewer than `t` valid shares should not reconstruct a plausible secret.

## Root Cause

The root cause is enforcing the threshold on unverified candidate records instead of verified shares.

The implementation checks:

```text
number of records that claim to match the fixed data
```

but should check:

```text
number of shares that cryptographically verify against the commitments
```

## Recommended Mitigation

After building `points`, enforce the threshold again.

Suggested fix:

```rust
if points.len() < majority_fixed.threshold() {
    return Err(SwafeError::NotEnoughSharesForReconstruction);
}

let v0 = interpolate_eval(&points, curve::Fr::zero());
```

Apply the same pattern to every reconstruction function that filters candidate records into verified points.

## References

- [Official Code4rena Report](https://code4rena.com/reports/2025-11-swafe)
- [Code4rena Contest Page](https://code4rena.com/audits/2025-11-swafe)
- [Original Submission S-548](https://code4rena.com/audits/2025-11-swafe/submissions/S-548)
- [Affected Code: threshold](https://github.com/code-423n4/2025-11-swafe/blob/main/lib/src/association/v0.rs#L151)
- [Affected Code: commitment generation](https://github.com/code-423n4/2025-11-swafe/blob/main/lib/src/association/v0.rs#L393)
- [Affected Code: reconstruction logic](https://github.com/code-423n4/2025-11-swafe/blob/main/lib/src/association/v0.rs#L455)
