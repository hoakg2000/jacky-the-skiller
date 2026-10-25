# jacky-the-skiller

My personal library of AI building blocks for Claude Code and other agents: subagents, skills, MCP servers and tools. Everything here is written by me, kept in one place, and copied into projects when needed.

## Layout

```
jacky-the-skiller/
├── agents/       # Claude Code subagents, one <name>.md each
├── skills/       # skills, one folder each: <name>/SKILL.md (+ references/, scripts/)
├── mcp/          # MCP servers, one folder each: <name>/README.md (+ config or source)
├── tools/        # tool definitions for agents, one folder each: <group>-<action>/
└── templates/    # starting points for MCP servers and tools
    ├── mcp/README.md
    └── tool/README.md
```

Agent and skill templates live inside the `create-agent` and `create-skill` skills (`templates/` + `checklist.md` in each), so the skills work on their own when copied.

## Catalog

### Agents

| Agent | Role | Model | Read-only | Scope |
| --- | --- | --- | --- | --- |
| [product-manager](agents/product-manager.md) | Turns a product description into a PRD (users, MVP scope, non-goals, metrics) | opus | writes PRD only | general |
| [project-analyst](agents/project-analyst.md) | Turns a request into user stories, acceptance criteria, impact and risks | opus | writes analysis only | general |
| [ux-designer](agents/ux-designer.md) | Designs user flows and screen specs with every state and message | opus | writes spec only | general |
| [solution-architect](agents/solution-architect.md) | Turns an analysis into a step-by-step implementation plan | opus | writes plan only | general |
| [task-planner](agents/task-planner.md) | Splits a plan into ordered tickets with owner agent and "done when" | opus | writes tasks only | general |
| [db-architect](agents/db-architect.md) | Designs and checks Prisma / PostgreSQL schema and migrations | opus | no | general |
| [nestjs-developer](agents/nestjs-developer.md) | Builds NestJS server features | sonnet | no | general |
| [nestjs-reviewer](agents/nestjs-reviewer.md) | Reviews NestJS server code | opus | yes | general |
| [spring-developer](agents/spring-developer.md) | Builds Java Spring Boot server features | sonnet | no | general |
| [spring-reviewer](agents/spring-reviewer.md) | Reviews Java Spring Boot code | opus | yes | general |
| [nextjs-developer](agents/nextjs-developer.md) | Builds the Next.js web client | sonnet | no | general |
| [nextjs-reviewer](agents/nextjs-reviewer.md) | Reviews Next.js / React code | opus | yes | general |
| [test-engineer](agents/test-engineer.md) | Writes and fixes Vitest unit, integration and e2e tests | sonnet | no | general |
| [qa-engineer](agents/qa-engineer.md) | Runs the app and checks each acceptance criterion pass/fail | sonnet | writes QA report only | general |
| [debugger](agents/debugger.md) | Reproduces a bug, proves the root cause, fixes it with a regression test | opus | no | general |
| [security-auditor](agents/security-auditor.md) | Security audit (auth, tokens, injection, dependencies) | opus | yes | general |
| [performance-auditor](agents/performance-auditor.md) | Performance audit (N+1, indexes, blocking work, bundles) | opus | yes | general |
| [devops-engineer](agents/devops-engineer.md) | CI/CD pipelines, Docker, env config | sonnet | no | general |
| [release-manager](agents/release-manager.md) | Release gates, SemVer bump, changelog, go/no-go | sonnet | edits changelog only | general |
| [doc-writer](agents/doc-writer.md) | Keeps docs in sync with the code | sonnet | no | general |
| [doc-reviewer](agents/doc-reviewer.md) | Reviews the docs doc-writer wrote or changed: accuracy against the code, completeness, writing | opus | yes | general |
| [doc-auditor](agents/doc-auditor.md) | Whole-repo gap between the built product and its docs, both ways, with a coverage table | opus | yes | general |
| [ts-developer](agents/ts-developer.md) | Builds the jktools package (core, CLI, CI) | sonnet | no | jktools |
| [ts-reviewer](agents/ts-reviewer.md) | Reviews jktools TypeScript code | opus | yes | jktools |
| [tool-author](agents/tool-author.md) | Writes jktools provider tool templates | sonnet | no | jktools |

Full life cycle (run by the `ship-feature` skill): `product-manager` → `project-analyst` → `ux-designer` → `solution-architect` → `task-planner` → developers → `test-engineer` → reviewers → `qa-engineer` → `security-auditor` / `performance-auditor` → `doc-writer` → `release-manager`.

### Skills

| Skill | What it does | Scope |
| --- | --- | --- |
| [ship-feature](skills/ship-feature/SKILL.md) | Orchestrates the full life cycle with the agents above, with gates between stages | general |
| [create-agent](skills/create-agent/SKILL.md) | Analyze → create → verify a new subagent, with bundled template and checklist | general |
| [create-skill](skills/create-skill/SKILL.md) | Analyze → create → verify a new skill folder, with bundled template and checklist | general |
| [add-tool](skills/add-tool/SKILL.md) | Steps to add one provider tool to jktools (template, fixture, test, registry) | jktools |

### MCP servers

None yet. Start from [templates/mcp/README.md](templates/mcp/README.md).

### Tools

None yet. Start from [templates/tool/README.md](templates/tool/README.md).

## Use in a project

Claude Code loads subagents from `~/.claude/agents/` (all projects) and `<project>/.claude/agents/` (one project). Skills load from `~/.claude/skills/<name>/` and `<project>/.claude/skills/<name>/`.

PowerShell, from this repo's root:

```powershell
# One project
New-Item -ItemType Directory -Force <project>\.claude\agents, <project>\.claude\skills | Out-Null
Copy-Item agents\nestjs-developer.md, agents\nestjs-reviewer.md <project>\.claude\agents\
Copy-Item skills\add-tool <project>\.claude\skills\ -Recurse

# All projects
New-Item -ItemType Directory -Force $HOME\.claude\agents, $HOME\.claude\skills | Out-Null
Copy-Item agents\*.md $HOME\.claude\agents\
```

Then run `/agents` in Claude Code to check they are listed. A project agent with the same name wins over the user-level one.

MCP servers: follow the Install section in each `mcp/<name>/README.md`.

## Add a new item

- **Agent**: ask Claude to "create an agent for …" (`create-agent` skill).
- **Skill**: ask Claude to "create a skill for …" (`create-skill` skill).
- **MCP server or tool**: copy the matching file from `templates/` (`mcp/<name>/README.md`, `tools/<group>-<action>/README.md`), fill every `<placeholder>`, delete the comment block, and add a row to the catalog.

Try every new item in a real project before relying on it.

## Rules

- No secrets, tokens or personal data in this repo. Use placeholders like `<API_TOKEN>` and env vars.
- Agents and skills learn project details at run time (README, `CLAUDE.md`, `package.json`) instead of hard-coding them. Project-specific items say so in the Scope column.
