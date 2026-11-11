---
name: compound-learning
description: End-of-session memory updater. Reflects on the current conversation, extracts decisions, preferences, context, corrections and open threads worth carrying forward, and writes them to the project's memory - memory.md in a working folder, or the Project's native memory in a Claude Project. Lesson promotion is handled by pm-reflect. Use at the end of a working session, as part of close-out, or as a daily scheduled task.
allowed-tools:
  - Read
  - Write
  - Edit
  - Grep
---

# Compound Learning Skill

**Purpose:** every session starts cold unless something tells Claude what mattered last time. This skill captures the parts of the conversation worth keeping so the next session picks up where this one left off. It records; pm-reflect decides what becomes a lesson.

## When to use

- Manually at the end of a working session (`/compound-learning` or "run compound learning")
- As step 2 of close-out
- As a scheduled task, typically end of day
- After a session where something material changed

## Storage mode

Use the same detection as pm-reflect:

- **Folder mode** - persistent working folder (Cowork cloud folder or local repo): write to `memory.md` in that folder.
- **Project memory mode** - session bound to a Claude Project with the memory tools available: write to `<root>/project-memory/memory.md` in the Project's native memory, under the subtree named in the session context. Write it directly; never hand the user a file to save. If the file does not exist but a legacy `claude/memory.md` Project doc does, run pm-reflect's migration first.
- **Neither** - say this session cannot write project memory and stop.

## What it captures

Scanned in this order:

1. **Decisions made** - anything the user committed to, even informally
2. **Preferences expressed** - voice, tone, format, working style, tools
3. **Context the user added** - projects, deadlines, people, constraints, the why behind the work
4. **Corrections** - moments where Claude went wrong and was corrected, with the underlying reason
5. **Open threads** - work in progress, with enough detail to restart cleanly

Not captured: play-by-play, code already in files, summaries of what was done.

## Process

### Step 1 - read existing memory

Read memory.md (or its native equivalent). If missing, create it with this header:

```markdown
# Project Memory

Auto-updated by the compound-learning skill. Most recent entries at the top.
```

In Project memory mode, add the native header above it (name: memory, a one-line description, sources: [chat]) and tag every content line `[stated]`.

Scan existing entries so you don't duplicate them. Update an existing entry rather than adding a duplicate.

### Step 2 - reflect on the session

For each capture category ask whether a future session would re-ask a question, repeat a mistake or miss something the user explained without it. Keep durable items only.

### Step 3 - write the entry

Add a new dated entry at the top:

```markdown
## YYYY-MM-DD

**Decisions:**
- [decision]

**Preferences:**
- [preference]

**Context:**
- [context]

**Corrections:**
- [what went wrong, the user's words if short, and the underlying cause]

**Open threads:**
- [WIP item with enough detail to resume]
```

Drop empty categories. One sentence per bullet; link to files for detail.

Record corrections faithfully, with the user's own words where short. pm-reflect reads them as evidence for pending lessons, so the quote matters more than a tidy paraphrase. Do not write to lessons.md or pending-lessons.md - that is pm-reflect's job.

### Step 4 - confirm

Show the new entry and ask if anything should change. When run inside close-out, skip this and return the entry to close-out, which confirms everything once. In a scheduled run, commit directly.

## Guardrails

**Do:** keep entries durable and short, update rather than duplicate, link out for detail, date every entry.

**Do not:** narrate the conversation, write bullets longer than three lines, capture sensitive personal information unless the user explicitly asks (in Project memory mode the native privacy rules also apply), or edit or remove existing entries without flagging it.
