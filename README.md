# handoff-skill

Two agent skills that let you **compact a long session, clear it, and resume clean**.

A long conversation accumulates dead ends, corrected assumptions, and exploration
noise. `/handoff` distills only the parts that matter into one small file; you clear
the context; `/handoff-pickup` loads that file into the fresh session and deletes it.

```
/handoff  →  /clear  →  /handoff-pickup
```

## Skills

| Skill | What it does |
| --- | --- |
| **handoff** | Writes a compacted handoff to `.claude/.handoff.md` — goal, current state, work that landed, hard-won facts, next steps — stripping wrong turns and noise. Then tells you to clear. |
| **handoff-pickup** | Reads `.claude/.handoff.md` into the new session, confirms the state in a short summary, and deletes the file. |

The handoff lives at `.claude/.handoff.md` in your project root — a fixed path so the
pickup always finds it after a clear. It's transient: pickup removes it. Add it to
your project's `.gitignore` if you don't want it committed.

## Install

With [skills.sh](https://skills.sh):

```bash
# both skills
npx skills add OWNER/handoff-skill

# or just one
npx skills add OWNER/handoff-skill --skill handoff
```

Replace `OWNER` with the GitHub account/org you publish this repo under.

## Use

1. Mid-task, when the context is getting long or muddled: run `/handoff`.
2. Clear the conversation (`/clear`).
3. In the fresh session: run `/handoff-pickup` and keep going.

## Layout

Flat [skills.sh](https://skills.sh) layout — each skill is a folder with a `SKILL.md`:

```
skills/
  handoff/SKILL.md
  handoff-pickup/SKILL.md
```

## License

MIT
