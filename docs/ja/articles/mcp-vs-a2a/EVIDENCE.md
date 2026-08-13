# Article 1 — Evidence Record

Every factual claim in `ja.article1.md` traced to its source. Captured 2026-08-12 from production `api.forcedream.ai`.

Required because Zenn's community guideline explicitly prohibits posting without verifying accuracy, and because this article is the first Japanese content published under the ForceDream name.

## Captured evidence

| Claim in article | Source | Verified |
|---|---|---|
| Signup at `/api/signup` returns `live_key` + trial balance | `POST /api/signup` → 201, `trial_balance_pence: 500` | ✅ |
| `/v1/signup` does not exist | `POST /v1/signup` → 404 | ✅ |
| AgentCard at `/.well-known/agent-card.json` | GET → 200 | ✅ |
| AgentCard `protocolVersion: 0.3.0` | Response body | ✅ |
| AgentCard `url` is the execute endpoint | `url: https://api.forcedream.ai/v1/a2a/execute` | ✅ |
| No `a2a_endpoint` field exists in A2A 0.3.0 | AgentCard key list contains no such field | ✅ |
| `skills` includes `data:extraction`, `summarization` | 17 skills listed in response | ✅ |
| A2A uses JSON-RPC 2.0 | Non-JSON-RPC body rejected with `-32600` | ✅ |
| A2A returns 202 with task ID, not a result | `HTTP 202`, `state: submitted`, `wtask_6085a4056d00fa1a15bd` | ✅ |
| Router resolves `data:extraction` → `data-extract-v1` | Poll URL in response `note` field | ✅ |
| Task completed with correct extraction | `state: completed`, rows match input | ✅ |
| `missing_fields: []`, `entity_verification: []` | Artifact text, verbatim | ✅ |
| Proof `algorithm: Ed25519-batched` | `GET /v1/workforce/proof/{id}/public` | ✅ |
| Proof `key_id: 0a0a7fa69af0` | Same response | ✅ |
| `cost_pence: 2` | Same response | ✅ |
| `input_hash`, `output_hash`, `merkle_root` | Same response, quoted verbatim | ✅ |
| Public key `key_id` matches proof `key_id` | `GET /v1/workforce/proof/public-key` → `0a0a7fa69af0` | ✅ |
| Public key PEM | Same response, quoted verbatim | ✅ |
| MCP config block | `forcedream-mcp/README.md`, unchanged | ✅ |

## Deliberately not claimed

- **Twelve-field signable variant.** The contract gained `inference_provider` and `inference_model` binding (SDK commits `a38f034` Kotlin, `9c518b3` Go, `baa8102` Java). The captured proof predates verification against that variant, so the article describes field structure only as "multiple variants, fetch the contract from the server" and points to the SDK implementations.
- **Merkle inclusion path.** The captured proof had `batch_size: 1` and zero siblings, so it does not demonstrate the multi-leaf case. The article does not describe an inclusion path it did not observe.
- **Latency figures.** The 202 took 1.58s from London against a `lhr1`-only deployment. No latency claim appears in the article, and no Japan-region measurement was possible.
- **Any comparison of MCP or A2A adoption, popularity, or performance.** No verifiable source.
- **Balance endpoint behaviour.** `/v1/balance` with `sk_fd_` returns earnings fields (`total_earned_pence`, `withdrawal_eligible`), not spending balance. Confusing enough that the article omits it rather than explaining a surface that needs clarification first.

## Pre-publication checklist

- [ ] Native Japanese technical review — required by Zenn's accuracy rule and by the AI エージェントナビ rejection reason
- [ ] Terminology checked against `docs/ja/glossary/`
- [ ] Test key `fd_live_87a0c177…` rotated or account discarded (exposed during capture)
- [ ] Confirm `/api/signup` still returns a trial balance at time of publication
- [ ] Confirm the task ID and proof are still retrievable at time of publication
- [ ] Decide platform: Qiita permits technical explanation of one's own product; Zenn prohibits promotion-primary articles. This article is protocol-first with ForceDream as example, which fits both — but Zenn is the stricter test.
