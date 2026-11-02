---
name: rollback-drill
description: Plan and run a timed rollback drill for an AI agent or agent feature, so you know the kill switch works and how long it takes before you need it. Use before launch, after changing deploy or config paths, or when someone says "we can always roll back."
---

# Rollback drill

"We can always roll back" is a guess until you have done it, timed it and checked that the old behavior actually came back.

## When to use

- Before the first production launch of an agent or a new agent capability.
- After changing how prompts, models, tools or feature flags are deployed.
- Quarterly for any agent that can write, send, pay or delete.

## Inputs to ask for

1. What "rolled back" means here: previous prompt version, previous model, feature flag off, agent fully disabled with a human fallback, or all of these.
2. Where each is controlled (flag service, config file, environment variable, deploy pipeline) and who has access.
3. The environment for the drill. Staging first. Production only with the owner's explicit approval and a quiet window.
4. The target time to safe state (for example 5 minutes from decision to old behavior verified).

## Steps

1. **Write the runbook first** as numbered commands or clicks. If a step says "ask someone", that is a finding.
2. **Pick a verification check** that proves the old behavior is back: a known test case whose output differs between versions, a version string in logs, or the flag state read back from the service.
3. **Start a timer and announce the drill** in the team channel (drills that surprise people cause real incidents).
4. **Execute the runbook exactly as written.** Record the time at each step. Do not fix the runbook mid-drill; note the problem and keep going if safe.
5. **Verify** with the check from step 2. Then verify in-flight work: what happened to tasks the agent had already started? Were queued actions cancelled, completed or duplicated?
6. **Roll forward again** and verify the new version is back.
7. **Report:**

```
Drill: <date>, <environment>, run by <role>
Rollback type: <flag off | previous prompt | previous model | full disable>
Time to safe state: <mm:ss> (target <mm:ss>)  PASS | FAIL
Verification used: <check>
In-flight work: <what happened>
Runbook problems found: <list, each with an owner>
Next drill due: <date>
```

## Guardrails

- Never run the first drill in production.
- If the drill shows rollback takes longer than the target, the agent is not ready to widen. Say so plainly.
- Keep the runbook where on-call can find it without the agent or the person who wrote it.
- Watch for state the rollback does not undo: emails already sent, records already written, payments already made. List these as "not reversible by rollback" in the report.
