---
name: <kebab-name>
description: <What the skill does, one sentence>. Use when <task kind, file types, or phrases that should load it>. Not for <close but different task> (<other skill or agent>).
---

<!-- Fill from ../checklist.md. Delete this comment and every unused line. -->

# <Skill title>

Files in this skill folder (delete the list if there are none):
- `references/<topic>.md` — <when to read it>.
- `scripts/<script>` — <what it does>. Run: `<command>`.

Input: <what the caller provides>. If <required input> is missing, ask for it.

## 1. <Analyze / Prepare>
1. Read <rules, docs, files>.
2. Check <source of truth: official docs, existing code>. Note the URLs or paths.
3. <Decision the user must confirm, if any>: show it and wait for an OK.

## 2. <Do the work>
1. <Step with the exact file path and naming>.
2. <Step>. Optional helper: the `<agent-name>` agent, told "<what to do and what not to touch>". If it is not installed, do this step yourself.

## 3. Verify
1. Run <lint, typecheck, test, or a script>. All must pass.
2. <Check that proves the result, e.g. a reviewer agent or a checklist>.
3. Fix and re-check. Stop after 2 rounds and report what is left.

## Report
<What to tell the user: files, results, open items.>
