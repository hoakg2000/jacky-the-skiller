---
name: create-agent
description: Creates a new Claude Code subagent (agent.md) in three phases - analyze (overlap with existing agents, agent vs skill, tools, model), create (from the bundled template), verify (frontmatter checks plus a doc-reviewer pass against the bundled checklist). Use when the user asks to "create an agent", "add a subagent", "make an agent for X", or to rework an existing agent.md. Not for skills (create-skill).
---

# Create an agent

Files in this skill folder:
- `templates/agent.template.md` — the starting file. Has a reviewer variant at the bottom.
- `checklist.md` — the standard (rule IDs F, D, S, B). Read it before phase 1.

Input: what the agent should do, and where it goes: this library's `agents/`, a project's `.claude/agents/`, or `~/.claude/agents/`. Ask if the target is not clear.

## 1. Analyze
1. Read `checklist.md`.
2. List existing agents in the target folder, `.claude/agents/` and `~/.claude/agents/`. Read the `description` of each.
3. Overlap check: if an existing agent already covers most of the job, propose extending it instead and stop for the user's choice.
4. Agent or skill? An agent fits a job that needs its own context, tools or model and returns a report. A procedure the current session should follow step by step is a skill: switch to `create-skill`.
5. If the agent is for a specific stack or project, read that project's README, `CLAUDE.md` and manifest for real conventions. For a large project, use the `Explore` agent; for domain conventions, an existing domain agent may help (for example `spring-reviewer` for a Spring agent), told "read-only: report conventions, do not edit files".
6. Draft the design: `name`, `description` (D1–D4), `tools` (least privilege), `disallowedTools`, `model`, `color`, read-only or not, inputs, outputs, neighbors for "Not for".
7. Show the design to the user in a few lines and wait for an OK or changes.

## 2. Create
1. Copy `templates/agent.template.md` to `<target>/<name>.md`. Use the reviewer variant for read-only reviewers and auditors.
2. Fill every placeholder from the design. Keep the section headings. Delete unused lines and every template comment.
3. Write workflow steps and rules specific to this role. Generic lines from the template stay only if they apply.
4. If the target is this library, add a row to the Agents table in `README.md`.

## 3. Verify
1. Mechanical checks (fix before step 2):
   - frontmatter parses and `name` equals the file name (F1, F2);
   - only fields from F3 and real tool names (F4);
   - no template placeholder or comment left: run `Select-String -Path <file> -Pattern '<[a-z-]+>|<!--'` and check each match; name patterns the agent really uses (like `<kebab-name>.md`) may stay (B10).
2. Run the `doc-reviewer` agent in review + verify mode: "Review `<file>` against the standard `<this skill folder>/checklist.md`, citing rule IDs. Verify that every agent, skill, path and command it names exists." If `doc-reviewer` is not installed, apply the checklist yourself, rule by rule.
3. Fix every Major and Minor finding. Re-run step 2 once. If findings remain after 2 rounds, report them to the user.
4. Optional smoke test: copy the file into a project's `.claude/agents/`, start a new session, check `/agents` lists it, and give it one small real task.

## Report
File path, the design (name, model, tools, read-only), checklist result (rules passed / failed), doc-reviewer verdict and remaining findings, and whether the smoke test ran.
