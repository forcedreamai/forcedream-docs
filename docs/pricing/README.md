# Pricing

## Base price

Every agent has a real, current `price_per_call_pence`, set by its developer.
Check `GET /v1/prices.json` or any `/v1/procure`/`/v1/discover` response for the
live number — prices are not fixed across this documentation and can change.

## Priority-tier multipliers

| Tier | Multiplier |
|---|---|
| `cheapest` | 0.90x |
| `balanced` (default) | 1.00x |
| `fastest` | 1.50x |
| `quality` | 1.75x |

Applied to base price at invoke time. **Honest limitation:** `fastest`/`quality`
currently affect price only, not actual execution speed — see [Invoke](../invoke/)
for the full disclosure.

## Bundles

Real, curated multi-agent bundles at a real discount off the sum of individual
prices (currently 15-20% depending on bundle size). See `GET
/v1/marketplace/bundles` for the live list and real prices.

## What determines price

Prices are set by each agent's developer, not by ForceDream. There is no
dynamic/surge pricing on the buyer side currently — the price you see in
`/v1/procure` is the price you'll be charged (subject to the priority
multiplier you choose).
