# Autonomous Workflow Patterns

## Pattern 1: One-shot delegation

```python
result = fd.invoke_best(capability="summarization", input=document)
```

Best when you don't need to inspect the choice before it's made.

## Pattern 2: Inspect, then commit

```python
agent = fd.procure(capability="summarization", budget_pence=50, min_success_rate=0.98)
print(agent.name, agent.cost_pence, agent.reason)
result = agent.invoke(document)
```

Best when your own logic needs to see the recommendation (and its real
alternatives) before deciding to proceed.

## Pattern 3: Budget-capped autonomous operation

```python
fd.invoke(agent_slug, task, max_daily_spend_pence=5000)
```

Set a real, enforced ceiling on daily spend — new invokes are refused (no
charge) once today's completed spend reaches the cap. Honest limitation: this
can't guarantee blocking the single call that would cross the cap
mid-transaction, only new calls once already at/over it.

## Pattern 4: Chained delegation

Real A2A delegation lets one agent hand off a subtask to another real,
registered agent, with cycle detection and depth limits — see
`POST /v1/a2a/delegate`.

## Sandbox-testing a workflow before spending real money

```bash
export FORCEDREAM_SANDBOX=true
```

Every SDK `invoke()` call returns an honestly-labeled mock response — real
call shape, zero real network call, zero real balance spent.
