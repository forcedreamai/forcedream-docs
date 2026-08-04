# Verifying a Result

Every ForceDream task produces a real, cryptographically signed proof.
Verification runs entirely locally — ForceDream is never asked whether its own
work is valid.

## The real endpoints

```bash
curl https://api.forcedream.ai/v1/workforce/proof/{task_id}/public
curl https://api.forcedream.ai/v1/workforce/proof/public-key
```

**Important:** `/v1/audit/merkle/verify` is a different, unrelated, paid tool
(platform audit-log verification) — it does not verify individual task proofs.
Using it for that purpose will not work.

## Two real proof shapes

Proofs use one of two algorithms, and a correct verifier must handle both:

- **`Ed25519`** — sign the digest of an 8-field canonical object directly.
- **`Ed25519-batched`** — the digest is a leaf in a Merkle tree; you must
  reconstruct the root via the proof's `inclusion_proof.siblings`, *then* verify
  the signature against that root, not the original digest. A verifier that only
  implements the simple case will incorrectly report a valid batched proof as
  "FAILED."

If `external_cost_hash` is present on the proof, the signable object has 10
fields instead of 8 (adds `external_cost_hash` and `retrieved_count`).

## Canonical serialization

The signable object must be serialized as compact JSON with alphabetically
sorted keys (matching `JSON.stringify(obj, Object.keys(obj).sort())` exactly) —
any deviation produces a different digest and an incorrect "FAILED" result even
against a genuinely valid signature.

## Real, working reference implementation

See [`forcedream-sdk-python/forcedream/verify.py`](../../../forcedream-sdk-python/forcedream/verify.py)
— confirmed against real, live, settled proofs.
