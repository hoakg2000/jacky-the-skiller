---
name: group-discussion
description: Runs a research-backed group discussion between agents, moderated by the main session - researchers gather evidence (at most 2 in parallel), then role-based discussants (optionally carrying an existing agent's expertise) debate in a set number of rounds through a shared transcript, and a synthesizer writes the outcome. Use when the user asks to "research and discuss", "brainstorm with agents", "run a group chat", "debate this", "get several views on", or to decide between options with evidence. Not for one-off research (researcher agent) or building a feature (ship-feature).
---

# Group discussion

The main session is the orchestrator and moderator. Subagents cannot message each other, so the group chat is a transcript file: the orchestrator calls each participant with it and appends each reply. The orchestrator is the only one who writes files.

Files in this skill folder:
- `references/roles.md` — role presets and which existing agent to use as `persona`. Read it in phase 1.
- `templates/brief.template.md`, `templates/transcript.template.md`, `templates/summary.template.md` — the run files.

Agents used: `researcher`, `discussant`. If either is not installed, stop and tell the user which file to copy into `.claude/agents/`.

Input: topic, goal (`decide`, `brainstorm` or `explore`), research questions or sources (optional), participants (2–5, each a role plus an optional persona), rounds (default 3, max 6), word limit per message (default 200), model (default sonnet).

**Parallel rule**: never more than 2 subagents running at once (foreground or background), and only for calls that do not depend on each other. Never start a call that needs a result still running.

Input: rounds N must be at least 1; `brainstorm` needs N ≥ 2.

## 1. Setup
1. Read `references/roles.md`. Propose participants for the goal if the user gave none.
2. Show the user: topic, goal, research questions with scope, participants, rounds, word limit, model. Wait for an OK or changes.
3. Create `docs/discussions/<kebab-topic>/` and `research/` inside it. If the folder already exists, use `<kebab-topic>-2` (then `-3`…); never overwrite a run. Write `brief.md` from `templates/brief.template.md`.

## 2. Research
Skip if there are no research questions.
1. Split the work into independent questions, one scope each (for example one website per question).
2. Run `researcher`, 2 at a time, each with: question, scope, `brief.md` path, length limit.
3. Save each result as `research/R01.md`, `R02.md`… in the order asked.
4. Add a digest to `brief.md`: one line per note (`R01: <finding> (confidence)`).

## 3. Discussion
For round k = 1..N:
1. Pick the mode. `explore`/`decide`: `critique` until the last round, then `converge` (N = 1: `converge`). `brainstorm`: round 1 `diverge`, middle rounds `critique`, last round `converge`.
2. Run every participant on the transcript as it stood after round k-1 (turns in one round are independent), 2 at a time. Each call passes: role, persona path, topic, goal, mode, paths of `brief.md`, `research/` and `transcript.md`, `round k of N`, word limit, and `model` if set.
3. Hold the replies until every participant in round k has replied, so no one in round k sees another round-k message. Then append them to `transcript.md` in participant order as `M<k>.<n>`, using `templates/transcript.template.md`. Do not edit replies.
4. Write a moderator note under the round: open disagreements (IDs), questions between roles, research requests.
5. Research requests before the last round: run `researcher` for each (2 at a time, same scope rules), save as the next `R` notes, and add them to the digest. In the last round, do not run them; list them for the synthesizer under Risks and unknowns.
6. Early stop (only after a `critique` or `converge` round): every participant said `Converge? yes`, the moderator note lists no open disagreement, and no research request is open. Then stop after this round.

## 4. Synthesis
1. Run `discussant` with `role: synthesizer`, `mode: synthesize`, topic, goal, the paths of `brief.md`, `research/` and `transcript.md`, the open research requests from the last round, a word limit of 400, and `model` if set.
2. Write `summary.md` from `templates/summary.template.md` with its reply.

## 5. Verify
1. Every `R`/`M` ID cited in `summary.md` exists: list them with the Grep tool (pattern `\b[RM]\d+(\.\d+)?\b`, output only matches) on `summary.md`, then check each ID in `research/` and `transcript.md`.
2. Every disagreement left open in the last moderator note appears under Open disagreements in `summary.md`.
3. Fix `summary.md` (or re-run the synthesizer once) if a check fails. Stop after 2 rounds and report what is left.

## Report
Folder path, rounds run and why it stopped (rounds reached or converged), participants, the Outcome section of `summary.md`, open disagreements, and research notes that had Low confidence.

## Rules
- Never more than 2 subagents running at once.
- Research stays inside each question's scope. No logins, form submissions or uploads.
- No secrets or personal data in any run file.
- Do not commit the run folder unless the user asks.
