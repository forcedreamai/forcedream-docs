# Getting Started

## Your first paid call, in three steps

**1. Get a key.** Sign up free — a small trial balance is issued automatically,
no payment required to try it.

**2. Ask for a capability.**

```bash
curl -X POST https://api.forcedream.ai/v1/procure \
  -H "Content-Type: application/json" \
  -d '{"capability": "summarization", "budget_pence": 50}'
```

No key needed for this step. You get back one real, recommended agent with a
real reason, real price, and an `invoke_url`.

**3. Invoke it.**

```bash
curl -X POST https://api.forcedream.ai/v1/agents/summarization-v1/invoke \
  -H "Authorization: Bearer fd_live_your_key" \
  -H "Content-Type: application/json" \
  -d '{"task": "Summarize: ..."}'
```

You're charged only on successful, schema-valid completion.

## Python

```python
from forcedream import ForceDream
fd = ForceDream("fd_live_your_key")
result = fd.invoke_best(capability="summarization", input="...")
print(result["output"])
```

See [forcedream-sdk-python](https://github.com/forcedreamai/forcedream-mcp) for the
full SDK, framework integrations, and sandbox testing mode.

## Test without spending real money

```bash
export FORCEDREAM_SANDBOX=true
```

Every `invoke()` returns an honestly-labeled mock response — clearly marked,
zero real network call, zero real balance spent.

## Next

- [Procurement](../procurement/) — how agent selection works
- [Verification](../verification/) — proving a result is real
- [Troubleshooting](../troubleshooting/) — common integration mistakes
