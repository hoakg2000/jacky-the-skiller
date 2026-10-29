# Roles

A participant is a `role` (the viewpoint it argues) plus an optional `persona` (an existing agent file whose expertise and checklist it borrows). Mix roles that pull in different directions.

## Role presets

| Role | Argues for | Good in |
| --- | --- | --- |
| skeptic | what can go wrong, weak evidence, hidden costs | decide, critique rounds |
| user advocate | the end user's needs, simplicity, UX | product and feature decisions |
| pragmatist | the cheapest option that fully works, time to deliver | decide, converge rounds |
| ideator | new and unusual options, combinations | brainstorm, diverge rounds |
| domain expert | standards of one field (needs a persona) | technical decisions |
| devil's advocate | the opposite of the current majority | when the group agrees too fast |
| synthesizer | neutral summary (used only in phase 4) | synthesis |

## Personas from this library

| Persona agent | Brings |
| --- | --- |
| `product-manager` | scope, MVP, non-goals, success metrics |
| `project-analyst` | user stories, acceptance criteria, impact |
| `ux-designer` | flows, states, accessibility |
| `solution-architect` | design trade-offs, reuse, simplest design |
| `security-auditor` | threats, data exposure, auth |
| `performance-auditor` | latency, scaling, cost of queries |
| `devops-engineer` | deploy, CI, operations cost |
| `qa-engineer` | testability, edge cases |

Pass the file path where the agent is installed: `.claude/agents/<name>.md` in the project, or `~/.claude/agents/<name>.md`. If neither exists, use the role without a persona and tell the user.

## Suggested line-ups

| Goal | Participants |
| --- | --- |
| decide (tech) | skeptic, pragmatist, domain expert (`solution-architect`), domain expert (`security-auditor`) |
| decide (product) | user advocate, skeptic, domain expert (`product-manager`) |
| brainstorm | ideator, user advocate, pragmatist |
| explore | domain expert (persona that fits), skeptic, user advocate |
