---
name: handoff-pickup
description: >
  Resume work from a handoff file written by `/handoff`. Reads `.claude/.handoff.md`,
  loads it as the working context for this session, confirms the picked-up state in a
  short summary, then deletes the file. Trigger on `/handoff-pickup`, or "pick up the
  handoff", "resume the handoff", "load the handoff".
---

# /handoff-pickup Skill

Inject the context saved by `/handoff` into this fresh session, then clean up.

---

## Step 1 — Read the handoff

Read `.claude/.handoff.md` at the project root (resolve with
`git rev-parse --show-toplevel`, falling back to the current working directory).

If the file does not exist, STOP and tell the user there's no handoff to pick up
(they may not have run `/handoff`, or it was already picked up). Do not guess.

## Step 2 — Load it as your working context

Treat the file as the source of truth for what's going on:

- Internalize the goal, state, work done, facts/constraints, and next steps.
- Verify before trusting: the handoff may name a `file:line`, function, env var, or
  value. Confirm anything load-bearing still exists in the codebase before acting on
  it — the file is a memo, not ground truth.

## Step 3 — Confirm

Give the user a short summary (≤5 lines): the goal, where things stand, and the next
step you're about to take. This proves the context loaded cleanly.

## Step 4 — Delete the handoff

Delete `.claude/.handoff.md` (`rm` it). It has served its purpose; leaving it around
risks a stale pickup later. Confirm it's gone in one line.

Then continue the work from the "Next steps" — unless the user's `/handoff-pickup`
message included new instructions, which take priority.
