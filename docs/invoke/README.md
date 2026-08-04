# Invoking an Agent

## The charge-only-on-success guarantee

You are billed **only** when your task completes successfully with schema-valid
output. A failed, empty, or invalid result costs £0 — every paid route in
ForceDream is built on one canonical billing function that enforces this
structurally, not just by convention.

## Real request/response shape

```bash
curl -X POST https://api.forcedream.ai/v1/agents/summarization-v1/invoke \
  -H "Authorization: Bearer fd_live_..." \
  -H "Content-Type: application/json" \
  -d '{"task": "...", "priority": "balanced"}'
```

Immediate response is one of:

- **`status: "pending"`** — real async work in progress. Poll `GET
  /v1/agents/:slug/result/:task_id`. Real, current terminal statuses:
  `completed`, `failed`, `dead_letter`, `frozen`.
- **`status: "succeeded"`** (rare, synchronous agents only) — result and charge
  already final.

There's also a generic `POST /v1/invoke` that takes `slug` (or `agent`) in the
body instead of the URL — same real logic underneath, for callers that prefer a
single fixed endpoint.

## Priority tiers — real pricing, honest limitation

| Tier | Price multiplier |
|---|---|
| `cheapest` | 0.90x |
| `balanced` (default) | 1.00x |
| `fastest` | 1.50x |
| `quality` | 1.75x |

**Honest disclosure:** `fastest` and `quality` currently affect **price only**.
They do not yet change actual execution priority or queue position — that would
require dispatcher-level changes not yet built. This is stated explicitly in the
API's own response (`priority_note` field) rather than implying a speed benefit
that doesn't exist.

## Optional: daily spend cap

Pass `max_daily_spend_pence` to have ForceDream refuse new invokes once your
declared daily spend is reached — no charge is made when the cap blocks a call.
Note: this cannot guarantee blocking the exact call that would cross the cap
mid-transaction, only new calls once today's already-completed spend meets or
exceeds it.
