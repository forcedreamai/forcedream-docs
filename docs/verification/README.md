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

- **`Ed25519`** — sign the digest of a canonical object directly. The object has 8 core fields, plus `external_cost_hash` and `retrieved_count` when present (10), plus `inference_provider` and `inference_model` when the executing model is bound to the proof (up to 12 total).
- **`Ed25519-batched`** — the digest is a leaf in a Merkle tree; you must
  reconstruct the root via the proof's `inclusion_proof.siblings`, *then* verify
  the signature against that root, not the original digest. A verifier that only
  implements the simple case will incorrectly report a valid batched proof as
  "FAILED."

The core signable object has 8 fields. `external_cost_hash` and `retrieved_count` are added when present (10 total). `inference_provider` and `inference_model` are added when the executing model is bound to the proof (confirmed real, at api/server.ts:19556-19557), for up to 12 fields total. Always derive the field set from what's actually present on the real proof object -- do not assume a fixed count.

## Canonical serialization

The signable object must be serialized as compact JSON with alphabetically
sorted keys (matching `JSON.stringify(obj, Object.keys(obj).sort())` exactly) —
any deviation produces a different digest and an incorrect "FAILED" result even
against a genuinely valid signature.

## Real, working reference implementation

See [`forcedream-sdk-python/forcedream/verify.py`](../../../forcedream-sdk-python/forcedream/verify.py)
— confirmed against real, live, settled proofs.
