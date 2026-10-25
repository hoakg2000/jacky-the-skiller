# Skill checklist

The standard every `SKILL.md` must meet. `create-skill` builds against it and `doc-reviewer` checks against it. Cite rules by ID.

## Frontmatter
- **F1** Valid YAML between `---` lines. Quote any value that contains `: `.
- **F2** `name` is kebab-case and equals the folder name (`skills/<name>/SKILL.md`).
- **F3** `description` is present. Other fields only if the target supports them.

## Description
- **D1** One sentence of what the skill does.
- **D2** "Use when …" with every trigger: task kinds, file types, phrases a user would type. The body loads only after the skill triggers, so triggers in the body do nothing.
- **D3** "Not for …" naming the close skill or agent.
- **D4** No trigger overlaps another skill's description without a "Not for" that separates them.

## Structure
- **T1** One folder: `SKILL.md` plus optional `references/`, `scripts/`, `templates/`, `assets/`. Every file the body names exists in the folder.
- **T2** Self-contained: copying the folder to `~/.claude/skills/` is enough. No path outside the folder except the user's project.
- **T3** Template files are not named `SKILL.md` (use `*.template.md`), so they are never loaded as skills.
- **T4** Body under ~200 lines. Long detail goes in `references/<topic>.md`, with a line saying when to read it.
- **T5** Deterministic or repeated work goes in `scripts/` with the exact run command.

## Workflow
- **W1** Phases in order (for example Analyze → Create → Verify), steps numbered, each one action.
- **W2** Each step names exact files, commands, or checks. No "as needed".
- **W3** A Verify phase that proves the result: commands that must pass, or a checklist or reviewer pass, with a round limit.
- **W4** Agents used in a step are optional helpers: the step says what to tell the agent, what it must not touch, and what to do if the agent is not installed.
- **W5** Decisions that belong to the user are shown and confirmed before the work that depends on them.
- **W6** The skill runs in the main session, so it may ask the user. Say where it must stop and ask.

## Safety
- **X1** No secrets; placeholders like `<API_TOKEN>`.
- **X2** No destructive commands, deploy, push or commit unless the user asks.
- **X3** No `<placeholder>` or template comment left.
