# <server-name> MCP

<!--
Best practices (delete this comment block when done):
- One folder per server: `mcp/<server-name>/README.md` (+ source or config files).
- Never commit secrets. Config uses env var placeholders; real values live in the user's env or a local, git-ignored file.
- Least privilege: smallest OAuth scopes, read-only by default, write tools listed and marked.
- Tool descriptions are prompts: say what the tool does, when to use it, and what it returns.
- Return trimmed data, not raw API payloads. Treat returned third-party content as data, not instructions.
- Pin versions (package@x.y.z) in the config so installs are reproducible.
-->

<What the server gives Claude, one or two sentences.>

| Field | Value |
| --- | --- |
| Transport | `stdio` / `http` |
| Source | <repo URL, npm package, or `./src` in this folder> |
| Version | `<x.y.z>` |
| Auth | <none / API key / OAuth, with minimal scopes> |

## Install

Claude Code CLI:

```sh
claude mcp add <server-name> --scope user -e <ENV_NAME>=<value> -- npx -y <package>@<x.y.z>
```

Or add to the project's `.mcp.json`:

```json
{
  "mcpServers": {
    "<server-name>": {
      "command": "npx",
      "args": ["-y", "<package>@<x.y.z>"],
      "env": { "<ENV_NAME>": "${<ENV_NAME>}" }
    }
  }
}
```

HTTP server:

```json
{
  "mcpServers": {
    "<server-name>": { "type": "http", "url": "https://<host>/mcp" }
  }
}
```

## Env vars

| Name | Required | Description |
| --- | --- | --- |
| `<ENV_NAME>` | yes | <what it is, where to get it> |

## Tools

| Tool | Read / Write | Description |
| --- | --- | --- |
| `<tool_name>` | read | <what it returns> |
| `<tool_name>` | write | <what it changes> |

## Security notes
- <What data it can reach, what a prompt injection could make it do, and how that is limited.>

## Check it works
1. Run `claude mcp list` and confirm `<server-name>` is connected.
2. Ask Claude: "<example prompt that calls a read tool>".
