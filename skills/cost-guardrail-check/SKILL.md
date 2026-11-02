---
name: cost-guardrail-check
description: Check that an AI agent has cost guardrails before it runs unattended (a per-run budget, a step limit, alerts and a kill switch) and estimate monthly cost from real usage. Use before scheduling an agent, raising its limits, or when a bill surprised someone.
---

# Cost guardrail check

Agents that loop, retry or call tools in a cycle can spend money quickly and quietly. This check makes sure something stops them before the invoice does.

## When to use

- Before an agent runs on a schedule, on a queue, or without a person watching.
- Before raising traffic, adding a bigger model or adding tools that call other models.
- After an unexpected bill.

## Steps

1. **Find the usage data.** You need, per run: model, input tokens, output tokens and number of model calls. Most providers and agent frameworks log this. If nothing logs it, that is finding number one.
2. **Get current prices from the provider's pricing page** and write the date next to them. Do not use remembered prices; they change.
3. **Compute per run:** cost = input tokens x input price + output tokens x output price (per million tokens, divided accordingly). Note the average, the most expensive run and the run with the most steps.
4. **Check each guardrail exists and is tested:**
   - Per-run budget: a hard stop when a single run passes a dollar or token limit.
   - Step limit: a maximum number of model calls or tool calls per run (loop protection).
   - Retry policy: limited retries with backoff, not infinite.
   - Daily or monthly spend alert at the provider and in your own monitoring.
   - Kill switch: a flag or setting that stops all runs, and someone who knows where it is.
   - Provider-side limits: spending caps or project budgets if the provider offers them.
   Mark each as present and tested, present but untested, or missing.
5. **Project monthly cost:** average cost per run x expected runs per month. Show the arithmetic. Add a line for the worst case: most expensive run x runs per month.
6. **Report:**

```
Prices used (checked YYYY-MM-DD): <model: in / out per million tokens>
Runs analysed: <n> | average $<x> | max $<y> | max steps <z>
Projection: $<avg> x <runs> = $<month>   worst case: $<max> x <runs> = $<month>
Guardrails: budget <status>, step limit <status>, retries <status>, alerts <status>, kill switch <status>
Recommendation: <what to add before it runs unattended>
```

## Guardrails

- Never assume a model is free because it is missing from the price list. Ask.
- Do not set limits so tight that normal runs fail. Set the step limit from observed runs (for example, comfortably above the largest normal run) and review it monthly.
- Test the per-run budget by forcing a run over it in staging.
