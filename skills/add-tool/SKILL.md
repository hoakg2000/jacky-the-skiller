---
name: add-tool
description: Steps to add one provider tool to jktools (template, fixture, test, registry entry). Use when adding or changing any tool under templates/<group>/, e.g. gmail/search or calendar/create.
---

# Add a jktools tool

Input: the tool name `<group>/<action>` (e.g. `gmail/search`) from `docs/04-tool-catalog.md`.

1. **Read the rules**: `docs/05-tool-authoring.md`, the tool's row in `docs/04-tool-catalog.md`, `templates/core.ts`, and an existing tool in the same group with its `_shared.ts`.
2. **Check the official API docs** (provider's own docs site): endpoint, method, query and body params, response fields, minimal OAuth scope, pagination, rate-limit status code. Note the URLs.
3. **Write the template** `templates/<group>/<action>.ts`:
   - `export const <camelName> = defineTool({ name: '<group>_<action>', provider, description, input, write, execute })`.
   - Plain `fetch`; credential from `await ctx.getCredential('<provider>')`; pass `ctx.signal`.
   - Shared request/error/format helpers go in `templates/<group>/_shared.ts` only when two tools need them.
   - Return a trimmed object, never a bare array or raw payload.
4. **Add a fixture** `test/fixtures/<group>/<action>.json`: realistic shape, fake data only (`example.com`, fake ids).
5. **Write the test** `test/tools/<group>/<action>.test.ts` with mocked `fetch`:
   - request URL, method, params (and body for write tools),
   - trimmed output shape from the fixture,
   - 401 → `auth`, 404 → `not_found`, 429 → `rate_limited`.
6. **Register it** in `src/registry.json`: `provider`, `auth`, `scopes`, `write`, `files` (template plus `_shared.ts` if used), `dependencies: ["zod"]`; add to its group and any preset from 04.
7. **Run** lint, typecheck and test from `package.json`. All must pass.
