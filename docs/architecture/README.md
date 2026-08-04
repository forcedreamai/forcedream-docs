# Architecture

## The invoke lifecycle

1. `POST /v1/agents/:slug/invoke` (or generic `POST /v1/invoke`) — real balance
   check, rate limiting, task creation
2. Async agents: `status: "pending"` returned immediately; a real dispatcher
   processes the task out-of-band
3. `GET /v1/agents/:slug/result/:task_id` — poll for the real terminal state
   (`completed`, `failed`, `dead_letter`, `frozen`)
4. On real success: charge happens, developer earnings credited, a
   cryptographic proof is generated

## The billing invariant

One canonical billing function enforces: a real, successful charge must
happen before any developer-earnings credit is possible, structurally — not by
convention. This was built specifically to close a real, found class of bug
where 27 separate routes had been crediting earnings with no corresponding
charge.

## Proof generation

Two real signing modes exist — direct signature and batched (Merkle-tree)
signature, the latter used to amortize signing cost across multiple tasks. See
[Verification](../verification/) for the full technical detail.

## What's not publicly documented

Internal dispatcher/queue/scheduling implementation is not part of this public
documentation — this hub covers the public API surface only.
