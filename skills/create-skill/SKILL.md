---
name: create-skill
description: Creates a new Claude Code skill folder (SKILL.md plus optional references, scripts, templates) in three phases - analyze (overlap with existing skills and agents, skill vs agent, triggers), create (from the bundled template), verify (structure checks plus a doc-reviewer pass against the bundled checklist). Use when the user asks to "create a skill", "add a skill", "turn this workflow into a skill", or to rework an existing SKILL.md. Not for subagents (create-agent).
---

# Create a skill

Files in this skill folder:
- `templates/SKILL.template.md` — the starting file.
- `checklist.md` — the standard (rule IDs F, D, T, W, X). Read it before phase 1.

Input: what the skill should do, and where it goes: this library's `skills/`, a project's `.claude/skills/`, or `~/.claude/skills/`. Ask if the target is not clear.

## 1. Analyze
1. Read `checklist.md`.
2. List existing skills (`<target>/*/SKILL.md`, `.claude/skills/`, `~/.claude/skills/`) and agents. Read each `description`.
3. Overlap check: if an existing skill already covers most of the job, propose extending it instead and stop for the user's choice.
4. Skill or agent? A procedure the current session follows step by step, or knowledge loaded on demand, is a skill. A job that needs its own context, tools or model and returns a report is an agent: switch to `create-agent`.
5. Collect the real procedure: from the user, from an existing workflow in a project (read its docs and scripts; use the `Explore` agent for a large project), or from official docs. An existing domain agent may help, told "read-only: report the steps and conventions, do not edit files".
6. Draft the design: `name`, `description` with every trigger (D1–D4), phases and steps, files the folder needs (`references/`, `scripts/`, `templates/`), agents used as optional helpers, and where the skill stops to ask the user.
7. Show the design to the user in a few lines and wait for an OK or changes.

## 2. Create
1. Create `<target>/<name>/` and copy `templates/SKILL.template.md` to `<target>/<name>/SKILL.md`.
2. Fill every placeholder from the design. Delete unused lines and every template comment.
3. Move long reference detail into `references/<topic>.md`; put repeated commands in `scripts/` (T4, T5). Name any template file `*.template.md` (T3).
4. If the target is this library, add a row to the Skills table in `README.md`.

## 3. Verify
1. Mechanical checks (fix before step 2):
   - frontmatter parses and `name` equals the folder name (F1, F2);
   - every file the body names exists in the folder (T1);
   - no template placeholder or comment left: run `Select-String -Path <folder>\*.md -Pattern '<[a-z-]+>|<!--'` and check each match; name patterns the skill really uses (like `<group>/<action>`) may stay (X3).
2. Run the `doc-reviewer` agent in review + verify mode: "Review `<folder>/SKILL.md` against the standard `<this skill folder>/checklist.md`, citing rule IDs. Verify that every agent, skill, path, script and command it names exists." If `doc-reviewer` is not installed, apply the checklist yourself, rule by rule.
3. Fix every Major and Minor finding. Re-run step 2 once. If findings remain after 2 rounds, report them to the user.
4. Optional smoke test: copy the folder into a project's `.claude/skills/`, start a new session, type one of its trigger phrases, and check that the skill loads and its first phase runs.

## Report
Folder path, the design (name, triggers, phases, helper agents), checklist result (rules passed / failed), doc-reviewer verdict and remaining findings, and whether the smoke test ran.
