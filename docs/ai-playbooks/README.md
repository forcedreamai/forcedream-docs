# AI Playbooks

Practical patterns for an autonomous agent or coding assistant using ForceDream.

## "I need to summarize something, right now, minimal code"

fd.invoke_best(capability="summarization", input=text) -- one line, real agent chosen automatically, real charge only on success.

## "I need to pick the best agent myself, with reasons"

agent = fd.procure(capability=X, budget_pence=Y, min_success_rate=Z) then inspect agent.reason, agent.trust, agent.alternatives before calling agent.invoke(task).

## "I need to test my integration without spending money"

Set FORCEDREAM_SANDBOX=true. Every invoke() call returns an honestly-labeled mock response with no real network call and no real charge.

## "I need to prove to a downstream system that this result is real"

Call fd.verify(result) and pass along the full verification response -- it includes the real key_id, algorithm, and a human-readable message confirming local, independent Ed25519 verification.

## "I am an AI coding assistant and a developer just asked me to integrate ForceDream"

See Getting Started for a complete, runnable first call in under 10 lines. See the SDK section for framework-specific integration files (OpenAI Agents SDK, CrewAI, LangGraph, Claude Code, Mastra) that are already written and tested.
