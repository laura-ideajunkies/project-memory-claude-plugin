# project-memory

Two skills that stop agent sessions starting cold.

Claude Code and Cowork sessions begin with no memory of the last one. Decisions get re-litigated, preferences get re-stated, the same mistakes get corrected twice. This pair fixes that with three plain markdown files per project and a routine that keeps them current - and the corrections compound: make the same mistake twice and it becomes a standing lesson the project never forgets.

## The three files

- **`CLAUDE.md`** - standing context. What the project is, who's involved, how to work in this folder. Loads every session.
- **`memory.md`** - the running log. Dated entries, newest first: decisions, preferences, corrections, open threads. What was true lately.
- **`lessons.md`** - the distillation. When a correction in memory recurs, it graduates here as one dated line. What stays true.

Meeting notes versus house style guide: notes tell you what happened lately; the style guide exists so nobody repeats a mistake often enough to need notes about it.

## The two skills

**`project-init`** - run it in any new folder (Cowork cloud folder or local repo). It checks what already exists, interviews you briefly - one question at a time, skipping anything the conversation already answers - then scaffolds the three files, wired together. Idempotent: it fills gaps and never overwrites.

**`compound-learning`** - run it at the end of a working session, or schedule it daily. It reflects on the conversation, writes a dated entry to `memory.md` (decisions, preferences, context, corrections, open threads - never the play-by-play), and checks each new correction against the log. Second occurrence of the same underlying mistake: it writes the lesson to `lessons.md`, marks the promotion, and keeps the history.

The promotion threshold is deliberate. A one-off correction stays in memory; a lesson is something the project keeps teaching.

## Install

**Claude Code** - the repo doubles as a plugin marketplace:

```
/plugin marketplace add laura-ideajunkies/project-memory-claude-plugin
/plugin install project-memory@project-memory
```

Or copy the two folders from `skills/` into `~/.claude/skills/`.

**Claude.ai / Cowork** - open the `.skill` files in `dist/` in a chat and save them, or add the `skills/` folders to your workspace skills.

## Use

In a new project folder: "set up this project". Answer the questions. From then on, end sessions with "run compound learning" - or schedule it daily and stop thinking about it.

## Structure

```
project-memory-claude-plugin/
├── .claude-plugin/          # plugin + marketplace manifests
├── skills/
│   ├── project-init/SKILL.md
│   └── compound-learning/SKILL.md
├── dist/                    # packaged .skill files for Claude.ai / Cowork
├── LICENSE
└── README.md
```

## Licence

MIT. Fork and adapt freely.
