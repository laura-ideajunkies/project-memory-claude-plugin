---
name: project-init
description: Set up a new project properly - standing context, memory, lessons and pending lessons, wired together for compound-learning and pm-reflect. Use whenever the user says "set up this project", "init this project", "project init", "new project folder", "scaffold this folder", or starts working in a new or empty folder (Cowork cloud folder or local repo) that has no CLAUDE.md. Also use when the user asks to add memory or lessons to an existing project, including a Claude Project, where the files go into the Project's native memory.
---

# Project init

Sets up a project so every future session starts warm: standing context, session memory, binding lessons, pending lessons and the lesson log. Works in Cowork cloud folders, local repos and Claude Projects.

## Step 0 - detect the storage mode

- **Folder mode** - persistent working folder: ordinary files `CLAUDE.md`, `memory.md`, `lessons.md`, `pending-lessons.md`, `lesson-log.md` in that folder.
- **Project memory mode** - session bound to a Claude Project with the memory tools available: native memory files under the Project's memory subtree named in the session context - `<root>/project-memory/context.md` (the CLAUDE.md content), `memory.md`, `lessons.md`, `pending-lessons.md`, `lesson-log.md`. Write them directly with the memory tools; never hand the user files to save. If legacy `claude/` Project docs exist, run pm-reflect's migration instead of starting fresh.
- **Neither** - say this session cannot store project memory and stop.

## Step 1 - check what exists

Only create or fill what is missing; never overwrite. If everything exists, say so and ask whether anything needs updating.

## Step 2 - interview

One question at a time, and only for what the conversation and existing files don't already answer:

1. **What is this project?** One or two sentences - what it is and what done looks like.
2. **Standing context.** People, clients, deadlines, related projects, constraints.
3. **Working instructions.** Tone, tools, always and never. Leave the section out if there are none.

Stop as soon as there is enough to write the files. Ask too whether the default lesson threshold of 3 distinct sessions suits this project; record a different number only if the user gives one.

## Step 3 - create the files

**CLAUDE.md** (folder mode) or **context.md** (Project memory mode), dropping empty sections:

```markdown
# <Project name>

<Purpose.>

## Context

<Standing context, short bullets.>

## Working instructions

<Custom instructions, short bullets.>

Lesson threshold: 3

## Session files

- Read memory at the start of every session - decisions, preferences, open threads and corrections, newest first.
- Read lessons before substantial work and treat them as binding. Pending lessons are observations, not rules.
- At the end of a session, run close-out (or compound-learning then pm-reflect).
```

**memory.md** - exactly the header compound-learning expects.
**lessons.md**, **pending-lessons.md**, **lesson-log.md** - the headers in pm-reflect's File formats section, nothing else.

In Project memory mode, give each file the native header (name matching the file stem, a one-line description, sources: [chat], aliases) and tag content lines `[stated]`. Use these descriptions so future sessions know when to open them:
- context: "Standing context and working instructions - read at the start of every session"
- memory: "Session memory - decisions, open threads, corrections; read at session start"
- lessons: "Binding lessons for this project - read before any substantial work"
- pending-lessons: "Observed patterns not yet proven - not binding; read by pm-reflect"
- lesson-log: "Evidence trail for lessons - read by pm-reflect"

## Step 4 - confirm

One short confirmation listing what was created and, in one line each, what went into the context file. Do not read files back in full.

## Project memory mode - optional instructions line

Claude cannot edit a Project's custom instructions. Native memory is listed to Claude in every session, so the setup works without this, but one line in the instructions makes the start-of-session read reliable. Offer it once, in a code block:

```
At session start, read project-memory/context.md, memory.md and lessons.md from this Project's memory and treat lessons as binding. Close sessions with /close-out.
```

## Rules

- Never overwrite an existing file; fill gaps only.
- Interview answers go into CLAUDE.md or context.md, never into memory.md.
- Keep the context file short - it is read every session.
