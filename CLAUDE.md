# CLAUDE.md

This repo is a library of AI building blocks (agents, skills, MCP servers, tools). It is not an app: no build, no tests.

- New agents and skills go through the `create-agent` / `create-skill` skills; their `checklist.md` is the standard. MCP servers and tools start from `templates/`. Keep the template's sections; fill every `<placeholder>`; delete the comment block.
- Names are kebab-case and match the file or folder name (`agents/<name>.md`, `skills/<name>/SKILL.md`).
- Every add, rename or removal updates the Catalog tables in `README.md`.
- Every agent is standalone: it works when called directly with only the request and the repo. Other agents' outputs are optional inputs; other agents are named only as suggested next steps. Orchestration lives outside the agents (the `ship-feature` skill, or a future orchestrator).
- Agents and skills stay project-agnostic: they read README, `CLAUDE.md` and manifests at run time. If an item only fits one project, mark it in the catalog Scope column.
- Never commit secrets, real tokens or personal data. Use placeholders and env vars.
- When improving an agent, keep its frontmatter fields (`name`, `description`, `tools`, `disallowedTools`, `model`, `color`) and its `Done when` / `Report back` sections.
