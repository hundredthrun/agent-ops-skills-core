---
name: pre-launch-eval-runner
description: Run a frozen set of test cases against an AI agent several times each and make a go or no-go call against pass rules written before the run. Use before launching, widening or re-enabling an agent, or when someone asks "is this agent ready?"
---

# Pre-launch eval runner

A demo proves the agent can work once. This skill checks whether it works every time on the cases that matter, using rules you wrote down before you saw the results.

## When to use

- Before an agent goes to real users, a new customer, a new workflow or a bigger share of traffic.
- After any change to the prompt, model, tools, retrieval data or temperature.
- When asked "is it ready?" and the only evidence is a few good demo runs.

## Inputs to ask for

1. The agent's job in one sentence, and the actions it can take (read only, write, send, pay).
2. A test case set (20 to 50 cases is a practical start). If none exists, stop and help write one first: common cases, edge cases, cases where the agent must refuse or hand off.
3. The pass rule, agreed before running. Default if nobody has one: every critical case passes every run, and at least 90% of all cases pass every run.
4. How many runs per case. Default 3. Use 5 for agents that take irreversible actions.

## Steps

1. **Freeze the cases.** Copy the case set to a dated file (for example `evals/cases-2026-11-01.jsonl`). Do not edit cases after you see results. If a case is wrong, fix it in the next version and say so.
2. **Write the pass rule at the top of the results file** before the first run, with the date. Name the critical cases.
3. **Measure noise first** if you have not before: run the unchanged agent on 5 cases, 3 times each. If results flip between runs with no change, note the flake rate. You cannot read a small improvement smaller than this noise.
4. **Run every case N times.** Record one line per run: case id, run number, pass or fail, and a one-line reason for every fail. Grade against the expected outcome in the case, not against your impression.
5. **Score worst-of-N.** A case passes only if all N runs passed. A case with mixed results is flaky and counts as a fail.
6. **Apply the rule exactly as written.** Any critical case failing any run is a NO-GO, whatever the overall rate.
7. **Report** in this shape:

```
Decision: GO | NO-GO
Rule (written YYYY-MM-DD): <rule>
Cases: <n> | passed every run: <k> (<k/n>%) | runs per case: <N>
Critical failures: <ids or none>
Flaky cases: <ids or none>
Top 3 failure reasons, with case ids:
What would change the decision:
```

## Guardrails

- Never change the threshold after seeing results. If the rule was wrong, say so and re-run with a new rule next time.
- Never drop a failing case to make the numbers pass.
- Do not average across runs ("2 of 3 is 67%"). Worst-of-N is the point.
- If the agent can take irreversible actions, run against a sandbox or mocks. Never against production accounts.
- A GO on 30 cases does not prove rare failures are absent. With zero failures in n cases the true failure rate is probably under 3/n (rule of three). Say this in the report.
