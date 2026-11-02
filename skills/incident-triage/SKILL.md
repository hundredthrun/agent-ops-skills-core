---
name: incident-triage
description: Triage a live problem with an AI agent in production. Contain first, set severity, capture evidence, and keep a timeline. Use when an agent is doing something wrong for real users, such as wrong answers, wrong actions, loops, cost spikes or data exposure.
---

# Incident triage for AI agents

Agents fail differently from ordinary code. The same input can work nine times and fail the tenth, and the damage is often an action, not an error page. Contain first. Diagnose second.

## When to use

- Users or staff report the agent did something wrong, unsafe or expensive.
- Monitoring shows a spike in errors, cost, handoffs, refunds or complaints tied to the agent.
- Someone found personal data where it should not be.

## Steps

1. **Start a timeline now.** One line per event with the time and source: "10:42 support ticket says agent cancelled the wrong order." Keep adding to it the whole time.
2. **Ask four containment questions, in order:**
   - Is it still happening? (check the last 15 minutes of runs)
   - Can it take irreversible actions (send, pay, delete, publish) right now?
   - Is personal or customer data exposed?
   - Is cost running away?
   If any answer is yes, recommend containment immediately: turn the agent or the risky tool off behind its flag, switch to read-only or human approval, or roll back to the last known good version. The person on call decides; you give them the exact command or setting.
3. **Set severity** with this default scale (adjust to the team's own if they have one):
   - SEV1: harm is ongoing to customers, money or data, or legal exposure. Contain now, wake people up.
   - SEV2: wrong actions or answers affecting real users, contained or limited. Fix today.
   - SEV3: degraded quality, no harm yet. Fix this week.
4. **Capture evidence before it rots:** run ids or trace ids of 3 to 5 bad runs and 2 good ones, the exact prompt, model and tool versions in use, recent changes (deploys, config, data updates, provider incidents). Do not paste personal data into chat; reference ids.
5. **Form a first hypothesis** by comparing bad and good runs: what input, tool result or version differs? Prefer "what changed" over "why would the model do that".
6. **Write the status update** every 30 minutes for SEV1 and SEV2:

```
Status: investigating | contained | mitigated | resolved
Impact: who and what, with counts if known
Containment in place: <what>
Current hypothesis: <one line>
Next update: <time>
```

## Guardrails

- Do not "fix" by editing the prompt in production during the incident unless that is the documented rollback. Untested prompt edits cause second incidents.
- Do not delete logs or runs, even embarrassing ones.
- Do not message customers yourself. Draft the message for a human to send.
- After resolution, hand the timeline to a postmortem. An incident without one will repeat.
