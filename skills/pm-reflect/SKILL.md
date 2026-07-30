---
name: pm-reflect
description: Memory hygiene review for project-memory projects. Reads memory.md, lessons.md and CLAUDE.md, identifies entries that look stale, superseded, duplicated or no longer true, and presents pruning recommendations. Makes no change without the user's explicit approval. Use when the user invokes /pm-reflect, or says "prune the memory", "clean up the memory files", "is anything in memory stale", or "review the project memory".
---

# PM Reflect

**Purpose:** compound-learning only ever adds. Over time memory.md fills with closed threads, reversed decisions and passed deadlines, and every stale line costs context and can mislead a future session. This skill is the prune to compound-learning's grow: it reviews all three files, recommends what to remove or tighten, and applies only what the user approves.

## Step 1 - locate and read the files

Same file location logic as project-init: in a session with a working folder (Cowork cloud folder or local repo), read `memory.md`, `lessons.md` and `CLAUDE.md` from the current folder. In a session attached to a Claude Project with no working folder, read the Project docs `claude/memory.md`, `claude/lessons.md` and `claude/CLAUDE.md` instead.

If any file is missing, say which and review the ones that exist. If none exist, say the project has no memory files and offer project-init instead.

## Step 2 - identify candidates

Read all three files together - staleness is often visible only across files (a memory entry contradicted by a later one, a CLAUDE.md claim overtaken by memory). Flag:

- **Closed threads** - open threads in memory.md that later entries, or this conversation, show as finished or abandoned
- **Superseded decisions** - a decision reversed or replaced by a later entry; the newest stands, the old one is the candidate
- **Passed dates** - deadlines, events or "current sprint" framing whose date has gone by, in any of the three files
- **Duplicates and overlaps** - two entries covering the same ground; candidate is a merge, not two deletions
- **Contradictions** - CLAUDE.md context that newer memory contradicts; candidate is an update to CLAUDE.md
- **Dead lessons** - lessons.md lines about tools, formats or constraints the project no longer uses
- **Bloat** - memory entries that captured play-by-play or detail that belongs in a linked file

Do not flag: anything from the last 7 days, recurring corrections (they are the early warning lessons.md depends on), or anything you are merely unsure about. When in doubt, leave it alone - this skill removes only what is clearly dead.

## Step 3 - present recommendations

Present a numbered list before touching anything. For each item: the file, the entry quoted or tightly paraphrased, why it looks stale, and the proposed action - delete, merge into another entry, condense, or update wording. Group by file, memory.md first.

If nothing qualifies, say the files are clean and stop.

## Step 4 - get explicit approval

Ask the user which recommendations to apply. Use AskUserQuestion with multi-select where available (one option per item, or sensible groups if more than four); otherwise ask in plain text for the item numbers to apply. Accept partial approval - apply exactly the approved set, nothing more.

If no user is present (scheduled or unattended run), stop after Step 3: output the recommendations as the result and make no changes. This skill never modifies files in an unattended run.

## Step 5 - apply and record

Apply only the approved items. Then add one dated line at the top of memory.md's newest entry (or as a new dated entry) recording the prune - e.g. "pm-reflect: removed 3 closed threads, merged 2 duplicate context entries, updated CLAUDE.md sprint framing" - so the deletion itself leaves a trace. Confirm to the user in one short summary what changed in each file; do not read the files back in full.

## Rules

- No change of any kind without explicit approval given in this conversation - approval in a past session does not carry over.
- Recommend deletions conservatively; history is cheap, a wrongly deleted lesson is not.
- Never rewrite the user's wording while condensing beyond what the approved action requires.
- Never flag or remove entries less than 7 days old.
- lessons.md deletions deserve an extra sentence of justification - lessons were promoted precisely because they kept recurring.
