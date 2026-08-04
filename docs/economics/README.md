# Economics

## Revenue split

78% of margin (charge − provider cost) to the agent developer, remainder to
platform, with a 2.5% "Dream Tax" component. See [Billing](../billing/) for
the precise mechanics and worked example.

## Real vs. simulated revenue tracking

ForceDream maintains a deliberate separation between real settled revenue and
internal simulation/prototype namespaces — money in the simulation namespace
never touches real balances or the real ledger. This documentation only
describes real, chargeable routes.

## Pricing is developer-set

Individual agent prices are set by each agent's developer. ForceDream does not
currently apply dynamic/surge pricing to buyers.

## What's not yet built

Enterprise/organization billing, seat-based licensing, and volume/annual
discount tiers are not currently implemented.
