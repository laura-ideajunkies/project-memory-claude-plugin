---
name: project-init
description: Set up a new project folder properly - CLAUDE.md, memory.md and lessons.md, wired together and compatible with the compound-learning skill. Use whenever the user says "set up this project", "init this project", "project init", "new project folder", "scaffold this folder", or starts working in a new or empty folder (Cowork cloud folder or local repo) that has no CLAUDE.md. Also use when the user asks to add memory or lessons files to an existing project. In a classic claude.ai Project with no working folder, creates the files as Project docs and gives the user a project-instructions block to paste in; in a Claude Code Project with no repository, uses the project's own memory and instructions instead.
---

# Project init

Sets up a project folder so every future session starts warm: standing context in CLAUDE.md, session-to-session memory in memory.md, durable lessons in lessons.md. Works in Cowork folders, local repos, repositories attached to cloud sessions, and Claude Projects with no working folder.

## Step 0 - detect where the files will live

Three cases:

- **Working folder** (a Cowork folder or project folder, a local repo, or a repository attached to a cloud session or a Claude Code Project thread): the files are ordinary files in that folder. Follow the steps as written. In Cowork, the folder's `CLAUDE.md` is also what the app shows as the folder's instructions, and Claude may edit it during a session; a folder with no `CLAUDE.md` falls back to `AGENTS.md` where one exists.
- **Claude Code Project (beta) with no repository attached**: the project already has memory files Claude writes itself (a `MEMORY.md` index plus topic files, under Project settings > Memory) and project instructions Claude can change on request. Do not create a parallel set of files. Put the interview answers into the project instructions and finish with **Claude Code Projects** at the end.
- **Classic claude.ai Project (chat) with no working folder**: the workspace is ephemeral. Create the files as Project knowledge docs `claude/CLAUDE.md`, `claude/memory.md`, `claude/lessons.md` if the session has a tool that writes Project docs; otherwise hand the user the three files to add to Project knowledge themselves. Follow the steps below with those paths, and finish with **Classic claude.ai Projects** at the end.

## Step 1 - check what exists

Look in the current folder (or the Project's doc list, in cloud-project mode) for CLAUDE.md, memory.md and lessons.md. If any exist, say which, and only create or fill what is missing - never overwrite. If all three exist, say the folder is already set up and ask whether anything needs updating.

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

## Classic claude.ai Projects - extra step

Only when the files were created as Project knowledge docs. Project knowledge does not load as binding instructions the way a folder's CLAUDE.md does, and in a classic Project the instructions field is edited by the user, not by Claude. So after creating the docs, give the user this block verbatim in a code block and tell them to paste it into the Project's instructions (project page on claude.ai or the desktop app - "Set project instructions"):

```
## Project memory

This project uses three Claude-managed docs. Follow this in every session:

1. At session start, read claude/CLAUDE.md (standing context and working
   instructions) and claude/memory.md (decisions, open threads and
   corrections from previous sessions, newest first). Treat both as
   binding context before doing any work.
2. Before substantial work, read claude/lessons.md and apply its lessons.
3. At the end of a working session, or after any material decision or
   correction, run the project-memory:compound-learning skill to update
   claude/memory.md. If the skill is unavailable, append the entry to
   claude/memory.md directly (newest first, dated).
4. If a correction in claude/memory.md recurs, promote it to
   claude/lessons.md as a single dated line and note the promotion.
5. If any of these docs are missing, say so rather than proceeding as if
   they exist, and offer to run project-memory:project-init.
```

Tell the user the block is deliberately generic - identical for every project set up this way, since everything project-specific lives in CLAUDE.md - so it can be pasted as-is into any project that has these docs. Setup is not complete until the user has been given this block.

Claude's own project memory runs alongside these docs: it saves topics as you chat and anything the user asks Claude to remember reaches the next conversation. Say so, and say that `claude/memory.md` remains the reviewable, portable record the compound-learning and pm-reflect skills work on.

## Claude Code Projects (beta) - extra step

Only for a Claude Code Project with no repository attached. Every cloud thread reads the project's `MEMORY.md` index at start and opens the other memory files when it needs them, and the user can read, edit and delete them under Project settings > Memory. So:

1. Put the interview answers into the project instructions - ask Claude to update them, or edit them under Project settings > Memory > Project instructions - using the same headings as the CLAUDE.md template.
2. Ask Claude to remember two standing rules in project memory: at the end of a working session, run compound-learning and record decisions, preferences, corrections and open threads as a dated entry in project memory; when a correction recurs, record it as a single dated lesson.
3. If a repository is attached later, run project-init again inside that repository so `memory.md` and `lessons.md` live with the code. Cloud threads read each attached repository's `CLAUDE.md` at start.

## Rules

- Never overwrite an existing file; fill gaps only.
- Interview answers go into CLAUDE.md, never into memory.md - memory.md stays clean for compound-learning to manage.
- Keep CLAUDE.md short. It loads every session, so every line costs context; standing context earns its place, narrative does not.
