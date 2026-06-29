---
name: handoff
description: >
  Compact the current conversation into a clean handoff file (`.claude/.handoff.md`)
  that keeps only the work that matters and strips dead ends, wrong assumptions, and
  abandoned directions — so the context can be cleared and resumed fresh. Trigger on
  `/handoff`, or "compact the context", "hand this off", "prepare for a clear".
  Companion: `/handoff-pickup` reads the file back into a new session and deletes it.
---

# /handoff Skill

Distill everything useful in THIS conversation into one compact markdown file, then
tell the user they can clear the context and resume with `/handoff-pickup`.

The point is signal, not a transcript. A fresh session should be able to pick up the
work from this file alone — without re-learning the wrong turns you already took.

---

## Step 1 — Write the handoff file

Write to `.claude/.handoff.md` at the project root (resolve it with
`git rev-parse --show-toplevel`, falling back to the current working directory if
that's not a git repo). Do NOT hardcode an absolute path. Overwrite any existing
file — only one handoff is active at a time. If one already exists, mention you're
replacing it.

Use this exact structure. Fill every section from the real conversation; drop a
section entirely if it genuinely has nothing (don't pad it):

```markdown
# Handoff — <short task title>
_Written <date> · resume with `/handoff-pickup`_

## Goal
<The actual objective in 1–3 lines — what we're really trying to achieve, not how.>

## State
<What is DONE and verified vs. what is in progress right now. Be honest: if tests
were not run, say so. If something is half-applied, say where it stopped.>

## Work done
- <Concrete change that STUCK: `file:line` + what changed + why. One bullet each.>
- <Decisions that were made and held — the conclusion, not the debate.>

## Key facts & constraints
- <Non-obvious things learned that the next session needs: a gotcha, an env quirk,
  an entry point, a value, a name. The stuff that was expensive to discover.>

## Next steps
1. <Ordered, concrete, actionable. What to do first in the fresh session.>

## Do NOT
- <Only traps worth naming so they aren't re-explored: "X doesn't work because Y."
  Omit this section unless a dead end is genuinely useful to flag.>
```

### What to KEEP

- The goal, current state, and what remains.
- Changes that landed and decisions that held — with file references.
- Hard-won facts: entry points, gotchas, constraints, exact names/values/ids.
- The single best path forward.

### What to STRIP (this is the whole job)

- Wrong assumptions you later corrected — keep only the corrected conclusion.
- Abandoned approaches and dead ends — unless one is worth a `Do NOT` line.
- Exploration noise: files you read that led nowhere, false starts, back-and-forth.
- Reasoning chains — keep the conclusion, not how you got there.
- Anything the codebase already records (it'll still be there after the clear).
- Pleasantries, restated user prompts, tool-by-tool narration.

Rule of thumb: if deleting a line wouldn't slow down a fresh session, delete it.

---

## Step 2 — Hand back

After writing, tell the user, in 2–3 lines:

1. The handoff is written to `.claude/.handoff.md`.
2. A one-line summary of what it captured.
3. They can now clear the context (`/clear`), then run `/handoff-pickup` in the new
   session to resume.

Do not start any new work. The handoff IS the deliverable.
