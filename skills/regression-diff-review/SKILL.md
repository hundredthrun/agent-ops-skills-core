---
name: regression-diff-review
description: Review a change to an agent's prompt, model, tools or retrieval data for regressions by comparing eval results case by case, before and after. Use on any pull request or config change that touches agent behavior, or when someone says the new prompt is "better".
---

# Prompt and regression diff review

"The new prompt is better" usually means it fixed the case someone was looking at. This skill checks what it broke.

## When to use

- A pull request or config change edits a system prompt, few-shot examples, tool descriptions, model name, temperature, or retrieval data.
- A model provider announces a new version and someone wants to switch.

## Inputs to ask for

1. The change itself (diff of the prompt, config or tool schema).
2. Eval results for the same frozen case set on the old and new versions, with the same number of runs per case. If either is missing, run them first (see the pre-launch eval runner skill). Do not compare runs on different case sets.

## Steps

1. **Read the diff and predict.** Before looking at results, write down which case types the change should affect and which it should not. This catches surprises.
2. **Label every case** on each side using worst-of-N: pass (every run passed), fail (no run passed) or flaky (mixed).
3. **Bucket the changes:**
   - Regressed: pass before, fail or flaky after. These block the change by default.
   - Fixed: fail or flaky before, pass after.
   - Still failing: not passing on either side.
   - Added or removed cases: removed cases are a red flag; ask why.
4. **Read every regressed case's transcript** and state the cause in one line. Look for these common causes: instruction moved or reworded so an earlier rule lost priority, a new example the model now copies too literally, a tool description change that shifts tool choice, longer prompt pushing important context further away.
5. **Check the noise.** If you know the flake rate of the unchanged agent, a single flip in either direction may be noise. Re-run regressed cases 5 more times before blaming the change.
6. **Report:**

```
Change: <one line>
Cases: <n>, runs per case: <N>
Regressed: <ids>  Fixed: <ids>  Still failing: <ids>  Removed: <ids>
Predicted vs actual: <matches or surprises>
Cause of each regression: <id: one line>
Recommendation: MERGE | MERGE AFTER FIX | DO NOT MERGE
```

## Guardrails

- A net improvement does not excuse a regression on a critical case.
- Never edit the case set in the same change as the prompt. Separate pull requests.
- If no eval set exists, say the change cannot be reviewed for regressions and recommend building one first. Do not approve on vibes.
