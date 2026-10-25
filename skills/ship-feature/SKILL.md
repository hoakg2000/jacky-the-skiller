---
name: ship-feature
description: Runs the full product life cycle with subagents, from a product description or feature request to a reviewed, tested, documented release - PM, analysis, UX, architecture, task split, build, tests, QA, review, security, docs, release. Use when the user says "build this product", "ship this feature end to end", "run the full flow", or hands over a product description to deliver. Not for a single small fix (use debugger or a developer agent directly).
---

# Ship a feature end to end

The main session is the orchestrator: subagents cannot start other subagents, so only the main session runs each stage. Ask every planning and QA agent to write its output file in the plans folder, and pass the next agent those file paths, not a summary from memory. Reviewers and auditors write no files: keep their reports and pass them to `release-manager`.

Severity gate (one rule for every stage): any open Critical, High or Major finding, failing build or test, or QA **Not ready** blocks the next stage.

Input: a product description or feature request, and the target repo.

## Stages

| # | Stage | Agent | Output | Skip when |
|---|---|---|---|---|
| 1 | PRD | `product-manager` | `<name>-prd.md` | a single feature in an existing product |
| 2 | Analysis | `project-analyst` | `<name>-analysis.md` (stories + acceptance criteria) | never |
| 3 | UX | `ux-designer` | `<name>-ux.md` | no new screens |
| 4 | Plan | `solution-architect` | `<name>.md` plan | never |
| 5 | Tasks | `task-planner` | `<name>-tasks.md` | plan has 3 steps or fewer |
| 6 | Build | `db-architect` (Prisma), `nestjs-developer` / `spring-developer`, `nextjs-developer` | code per ticket | — |
| 7 | Tests | `test-engineer` (Vitest stacks); `spring-developer` writes its own JUnit tests and Flyway/Liquibase migrations | tests per ticket | — |
| 8 | Review | `nestjs-reviewer` / `spring-reviewer` / `nextjs-reviewer` | verdict per ticket | — |
| 9 | QA | `qa-engineer` | pass/fail per acceptance criterion | — |
| 10 | Security | `security-auditor` | findings | no auth, input, data-access or dependency change |
| 11 | Performance | `performance-auditor` | findings | no new queries, endpoints or pages |
| 12 | Docs | `doc-writer` | updated docs | — |
| 13 | Release | `release-manager` | go/no-go, changelog | the user is not releasing |

## Steps

1. **Check agents**: confirm the agents above exist in `.claude/agents/` or `~/.claude/agents/`. Name any that are missing and ask before continuing.
2. **Plan stages 1–5** in order. After each, show the user the open questions. Stop for answers before the next stage; do not decide owner questions yourself.
3. **Build each ticket** (stage 6–8) in the task file's order. If stage 5 was skipped, treat each plan step as a ticket. For each ticket:
   1. Run the owner agent with the ticket ID and the task file path.
   2. Run `test-engineer` for the ticket's logic if the owner did not add tests.
   3. Run the matching reviewer. On **Request changes**, send the findings back to the owner agent, then review again (max 2 rounds, then ask the user).
   4. Commit only if the user asked for commits, one commit per ticket.
   Run tickets marked parallel at the same time only when they touch different files, each with `isolation: "worktree"`, and never against one shared test database.
4. **Verify** (stages 9–11): run `qa-engineer` with the acceptance criteria file. Send each bug to `debugger` (asking for a fix) or the owner agent, run the matching reviewer on the fix, then QA again. Run the audits; blocking findings go back to the owner agent, then to the reviewer.
5. **Finish** (stages 12–13): run `doc-writer` with the list of changed files, then `release-manager` if releasing.
6. **Report** to the user: stages run or skipped (with reason), tickets done, test / QA / review / security results, open findings, and commands left for the user (deploy, push, tag).

## Rules
- Never skip a gate silently. A skipped stage is listed with its reason.
- The severity gate above applies to every stage.
- Never deploy, push or tag. The user runs those commands.
