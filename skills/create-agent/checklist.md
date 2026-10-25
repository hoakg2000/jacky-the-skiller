# Agent checklist

The standard every `agent.md` must meet. `create-agent` builds against it and its verify phase checks against it. Cite rules by ID.

## Frontmatter
- **F1** Valid YAML between `---` lines. Quote any value that contains `: `.
- **F2** `name` is kebab-case, unique, and equals the file name.
- **F3** Only real fields: `name`, `description`, `tools`, `disallowedTools`, `model`, `color`, `permissionMode`, `skills`, `mcpServers`, `hooks`, `memory`.
- **F4** `tools` lists only real tool names, and only the ones the job needs. Read-only agents: `tools: Read, Grep, Glob, Bash` + `disallowedTools: Edit, Write`.
- **F5** `model`: `opus` for review, audit, design, analysis, debugging; `sonnet` for building, tests, docs, ops; `haiku` for cheap lookups.
- **F6** `color` follows the library: green build, cyan frontend, yellow docs/tests/QA, red review/debug, purple audit, blue analysis/planning, orange data/release, pink design.

## Description
- **D1** One sentence of what it does, naming its area (stack, layer, files).
- **D2** "Use when …" with concrete triggers: task kinds and phrases a user would type. `Use PROACTIVELY` only if it should run unasked.
- **D3** "Not for … (<neighbor-agent>)" for every agent whose area is close.
- **D4** No trigger phrase overlaps another agent's description without a "Not for" that separates them.

## Standalone
- **S1** Finishes its job with only the request and the repo. No step requires another agent to run first or after.
- **S2** Outputs of other agents (PRD, plan, criteria, reports) are optional inputs: "read it if it exists".
- **S3** Never "ask the user" mid-run (a subagent cannot). Use "return the questions and stop" or "use <default> and say so in the report".
- **S4** Other agents are named only as suggested next steps in Report back, or in "Not for".
- **S5** No hard-coded project paths or versions unless the Scope is a named project. Learn them at run time (README, `CLAUDE.md`, manifest, sibling file).

## Body
- **B1** First line: role and who uses the output, and why quality matters.
- **B2** Sections: `Context`, `Workflow`, `Rules`, `Done when`, `Report back`. Reviewer variant: `Scope`, `Checklist`, `Verification`, `Report back`.
- **B3** Workflow steps are numbered, ordered, each one action.
- **B4** Every rule is checkable. No "be careful", "as needed", "appropriate".
- **B5** Safety: no secrets in output or logs; no destructive commands; no production or shared systems; no deploy, push or commit unless the user asks.
- **B6** Says what it must not edit and that needed changes go in the report.
- **B7** `Done when` lists commands or facts that prove the task is done.
- **B8** `Report back` gives the exact shape of the final message (verdict, findings format, files, commands, follow-ups).
- **B9** Under ~60 lines of body. Long reference material goes in a skill, loaded with `skills:`.
- **B10** No `<placeholder>` or template comment left.
