# Agent Ops Skills (open core)

Five agent skills for the boring part of running AI agents in production: testing before launch, rolling back, triaging incidents, reviewing prompt changes and keeping cost in check.

Each skill is a folder with a `SKILL.md` file (YAML frontmatter with `name` and `description`, then plain steps). Your coding or ops agent loads a skill when the task matches its description, then follows the steps.

By Hundredth Run. MIT licensed. Free.

## The skills

| Skill | Use it when |
|---|---|
| [`pre-launch-eval-runner`](skills/pre-launch-eval-runner/SKILL.md) | Before launching or widening an agent. Frozen cases, N runs each, worst-of-N, a pass rule written before the run |
| [`rollback-drill`](skills/rollback-drill/SKILL.md) | Before you need the kill switch. A timed drill with a verification check and a report |
| [`incident-triage`](skills/incident-triage/SKILL.md) | An agent is doing something wrong for real users. Contain first, severity, evidence, timeline |
| [`regression-diff-review`](skills/regression-diff-review/SKILL.md) | A prompt, model, tool or data change. Case-by-case before and after, regressions block |
| [`cost-guardrail-check`](skills/cost-guardrail-check/SKILL.md) | Before an agent runs unattended. Budget, step limit, retries, alerts, kill switch, monthly projection |

## Install

Copy the folders inside `skills/` into the skills folder your tool reads:

| Tool | Project (shared with the repo) | Personal (all your projects) |
|---|---|---|
| Cursor | `.cursor/skills/` or `.agents/skills/` | `~/.cursor/skills/` |
| Claude Code | `.claude/skills/` | `~/.claude/skills/` |

Cursor also reads `.claude/skills/` and `.codex/skills/`, so one copy can serve several tools. Other tools that support the Agent Skills format have their own folder; check their docs. Paths checked against the Cursor and Claude Code docs on 2026-11-01.

Example (Cursor, project level):

```
mkdir -p .cursor/skills
cp -r skills/* .cursor/skills/
```

Then ask your agent something like "we're about to launch the triage agent, is it ready?" and it should pick up `pre-launch-eval-runner`. You can also invoke a skill by name, for example `/rollback-drill`.

## How to adapt them

- Change the defaults (runs per case, pass threshold, severity scale) to your team's numbers. They are starting points, not standards.
- Keep the guardrail sections. They are what stops an agent from "fixing" an eval by deleting the failing case.
- Keep each `name` equal to its folder name. Cursor and the Agent Skills spec require it.

## Want more?

The paid **Agent Ops Skill Pack** has 12 deeper skills, each with a worked example, plus tested helper scripts (worst-of-N eval gate, before and after eval diff, cost and loop guard, transcript redaction, reproducible review sampling, skill linter): https://hundredthrun.gumroad.com/l/agent-ops-skill-pack

More kits for people who spec, launch and run AI agents: https://hundredthrun.gumroad.com

## License

MIT. See [LICENSE](LICENSE). Copyright (c) 2026 Hundredth Run.
