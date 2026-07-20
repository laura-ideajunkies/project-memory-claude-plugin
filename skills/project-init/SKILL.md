---
name: project-init
description: Set up a new project folder properly - CLAUDE.md, memory.md and lessons.md, wired together and compatible with the compound-learning skill. Use whenever the user says "set up this project", "init this project", "project init", "new project folder", "scaffold this folder", or starts working in a new or empty folder (Cowork cloud folder or local repo) that has no CLAUDE.md. Also use when the user asks to add memory or lessons files to an existing project.
---

# Project init

Sets up a project folder so every future session starts warm: standing context in CLAUDE.md, session-to-session memory in memory.md, durable lessons in lessons.md. Works the same in Cowork cloud folders and local repos.

## Step 1 - check what exists

Look in the current folder for CLAUDE.md, memory.md and lessons.md. If any exist, say which, and only create or fill what is missing - never overwrite. If all three exist, say the folder is already set up and ask whether anything needs updating.

## Step 2 - interview

Ask one question at a time. Before asking anything, extract what the conversation and any files in the folder already answer, and confirm rather than re-ask. Skip any question already covered.

1. **What is this project?** One or two sentences - what it is and what done looks like.
2. **Standing context.** People, clients, deadlines, related projects, constraints - anything Claude should know at the start of every session.
3. **Custom instructions.** How to work in this folder - tone, tools, things to always or never do. If the user has none, leave the section out rather than padding it.

Stop interviewing as soon as there is enough to write the files. Three questions is the ceiling, not a form to complete.

## Step 3 - create the files

**CLAUDE.md** - from this template, dropping any empty section:

```markdown
# <Project name>

<One or two sentence purpose, from the interview.>

## Context

<Standing context, as short bullets.>

## Working instructions

<Custom instructions, as short bullets.>

## Session files

- Read `memory.md` at the start of every session - it holds decisions, preferences, open threads and corrections from previous sessions, newest first.
- Read `lessons.md` before substantial work - it holds durable lessons for this project.
- At the end of a working session, or after any material decision or correction, run the compound-learning skill to update `memory.md`.
- When a correction in `memory.md` recurs, promote it to `lessons.md` as a single stated lesson, and note the promotion.
```

**memory.md** - exactly the header compound-learning expects, nothing else:

```markdown
# Project Memory

Auto-updated by the compound-learning skill. Most recent entries at the top.
```

**lessons.md**:

```markdown
# Lessons

Durable lessons for this project - things that hold across sessions, promoted from memory.md when a correction or gotcha recurs. One line each, dated.
```

## Step 4 - confirm

One short confirmation listing the files created and, in one line each, what went into CLAUDE.md. Offer to adjust. Do not read the files back in full.

## Rules

- Never overwrite an existing file; fill gaps only.
- Interview answers go into CLAUDE.md, never into memory.md - memory.md stays clean for compound-learning to manage.
- Keep CLAUDE.md short. It loads every session, so every line costs context; standing context earns its place, narrative does not.
