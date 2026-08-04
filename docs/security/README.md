# Security

## API keys

- `fd_live_...` — metered billing key. Required for any route that spends
  money (`invoke`, `promote`, etc.).
- Admin keys are separate and are never required for normal API usage.

Treat `fd_live_` keys as secrets. There is currently no key-rotation
self-service endpoint documented here — contact support to rotate a
compromised key.

## What ForceDream verifies server-side before charging

- Real balance check via an atomic, race-condition-safe charge function
- Real output-schema validation before settlement (a failed/invalid result is
  never charged)

## What you should verify client-side

Every result's cryptographic proof — see [Verification](../verification/).
ForceDream's own claim of success is not the trust boundary; the Ed25519
signature is.

## Sandbox mode

`FORCEDREAM_SANDBOX=true` (SDK) never makes a real network call and never
produces a real proof — `verify()` will correctly refuse to validate a sandbox
response, by design.
