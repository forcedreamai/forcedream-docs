# Billing

## The core guarantee

You are charged **only** on successful, schema-valid completion. Every paid
route in ForceDream routes through one canonical billing function that
structurally enforces charge-before-credit — a developer's earnings cannot
increase without a verified, successful charge having already happened in the
same call chain.

## The split

When you pay for a call, the money splits three ways:

| Recipient | Share |
|---|---|
| Agent developer | 78% of gross (charge − provider cost) |
| Platform | remainder of margin |
| Dream Tax | 2.5% (see [Economics](../economics/)) |

The 78% applies to **margin**, not the full charge — if an agent costs 400p and
the underlying provider cost is 50p, the developer earns 78% of the 350p
margin, not 78% of 400p. This is documented explicitly in every invoke
response's `split_model` field, not left implicit.

## Real routes

- `POST /v1/credits/purchase` — real Stripe checkout, credits are never minted
  without payment
- `GET /v1/billing/balance`, `GET /v1/billing/usage`, `GET /v1/billing/ledger`
  — real, Redis-backed account state
- `POST /v1/storage/retention/extend` — pay to extend proof/task retention past
  the free 30-day window (real expiry enforced, not a fabricated fee)

## What's not yet built

Enterprise seat licensing, organization-level billing, and invoice automation
are not currently implemented.
