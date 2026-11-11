---
name: pm-reflect
description: Lesson engine and memory hygiene for project-memory projects. Scans the session for feedback, matches it against lessons and pending lessons, adds evidence, elevates pending lessons that reach the threshold, expires stale ones, diagnoses lessons the user had to repeat, logs the result, and recommends pruning of stale memory. Runs automatically as the last step of close-out. Also use when the user invokes /pm-reflect, or says "run reflect", "check the lessons", "prune the memory", "clean up the memory files", "is anything in memory stale", or "review the project memory".
---

# PM Reflect

**Purpose:** compound-learning records what happened. This skill decides what the project has learned. It keeps three tiers - lessons (binding), pending lessons (observed, not yet proven) and a lesson log (the evidence trail) - and checks every session whether the lessons actually worked. The test it applies throughout: the user should never have to give the same feedback twice.

## Step 1 - detect the storage mode

- **Folder mode** - the session has a persistent working folder (Cowork cloud folder or local repo). The store is ordinary files in that folder: `memory.md`, `lessons.md`, `pending-lessons.md`, `lesson-log.md`, plus `CLAUDE.md`.
- **Project memory mode** - the session is bound to a Claude Project and the memory tools (memory_read, memory_write, memory_str_replace, memory_append) are available. The store is the Project's native memory, under the Project's memory subtree named in the session context: `<root>/project-memory/memory.md`, `lessons.md`, `pending-lessons.md`, `lesson-log.md`. Never hand the user a markdown file to save instead - write to memory directly. Follow the native format (see **Project memory format** below).
- **Neither** - say plainly that this session cannot write project memory and stop.

**Migration (Project memory mode, first run only):** if the native files do not exist but legacy Project docs do (`claude/memory.md`, `claude/lessons.md`, read-only copies under `/mnt/project/`), read them and create the native files from their content, then tell the user the legacy docs can be deleted from the Project. Legacy lessons import as lessons with evidence "imported from lessons.md". Do not invent pending lessons during migration.

Create any missing store file with its header (see **File formats**) before continuing. Never overwrite an existing file.

## Step 2 - read everything together

Read all store files and CLAUDE.md. Staleness and repetition are often visible only across files.

## Step 3 - extract feedback events from this session

Scan the conversation for every moment the user shaped how Claude works: corrections, explicit instructions ("always", "never", "from now on"), rejected outputs, redone work, approvals of a non-obvious choice, and repeated feedback. For each event record:

- the user's words, quoted (short)
- the underlying cause in one line - match on cause, never surface wording ("used em dashes again" and "wrong punctuation in the report" can be the same cause)
- whether it was **explicit** (a standing instruction) or **inferred** (a pattern Claude spotted)
- the correction turns it took before the user got what they wanted (user turns spent on that issue)

Skip one-off task feedback with no bearing on future sessions.

## Step 4 - match each event against the store

For each event, look for a lesson or pending lesson with the same underlying cause.

**Matches a lesson** - the user had to repeat feedback a lesson already covers. Diagnose which failure it was and record it in the log:
- *Not applied* - the lesson is right but did not shape behaviour (not read, buried, or worded too vaguely to act on). Fix: propose sharper wording or a more prominent position.
- *Wrong* - the lesson misdescribes what the user wants, or is too broad or too narrow. Fix: propose a rewrite.

Increment that lesson's repeat count. Both fixes are approval-gated (Step 8).

**Matches a pending lesson** - append the dated quote to its evidence and reset its expiry count. If it now meets the threshold, elevate it (Step 5).

**No match:**
- explicit standing instruction -> write straight to lessons. Explicit instructions never wait in pending.
- inferred pattern -> add to pending with the quote as evidence. A pending lesson must always carry at least one quote of the user's own words; never add one that rests only on Claude's guess about how the user works.

## Step 5 - elevate, expire, check effect

- **Elevate** a pending lesson when its evidence spans at least **3 distinct sessions**, or the number set by a line `Lesson threshold: N` in CLAUDE.md. Several instances in one session count once. Write it as one positive line ("Always X" / "Never Y - it causes Z"), move its evidence to the log, remove it from pending.
- **Expire** a pending lesson after **5 reflect runs** with no new evidence. Remove it and record the discard in the log with its last evidence, so it can be revived if the pattern returns.
- **Check effect** - for each lesson relevant to this session's work, note in the log whether it held (applied, no repeat) or failed (with the Step 4 diagnosis). Note any lesson not relevant for 60 days as a hygiene candidate.
- **Spotter misses** - if the user repeated feedback this session that appears in an earlier session but was never captured as a pending lesson, log it as a miss. Misses are how capture gets sharper: when misses share a pattern, propose a line for CLAUDE.md telling future runs to watch for it.

## Step 6 - scope

Mark each new lesson **project** (holds only here) or **global** (about the user across all work, such as punctuation or banned words). Global lessons still go into this store so they bind now, but list them in the report as candidates for the user to add to their account-level preferences or global CLAUDE.md. In Project memory mode Claude cannot write outside the Project's subtree, so never attempt it.

## Step 7 - memory hygiene

Review memory.md, lessons.md and CLAUDE.md for stale entries. Flag:

- **Closed threads** - open threads that later entries or this session show as finished or abandoned
- **Superseded decisions** - the newest stands, the old one is the candidate
- **Passed dates** - deadlines or "current" framing whose date has gone
- **Duplicates and overlaps** - candidate is a merge, not two deletions
- **Contradictions** - CLAUDE.md context that newer memory contradicts; candidate is a CLAUDE.md update
- **Dead lessons** - lessons about tools, formats or constraints no longer in use, or flagged in Step 5 as long unused
- **Bloat** - play-by-play detail that belongs in a linked file

Do not flag anything from the last 7 days, recurring corrections, or anything merely uncertain. When in doubt, leave it.

## Step 8 - apply

**Applied automatically** (additive or evidence-driven, and recoverable from the log): new lessons from explicit instructions, new pending lessons, evidence added, elevations that meet the threshold, expiries, repeat counts, log entries.

**Approval-gated** (destructive or judgement calls): rewriting or sharpening an existing lesson, deleting any lesson, every Step 7 hygiene item, any CLAUDE.md change.

Present the approval-gated items as one numbered list - file, entry quoted, reason, proposed action - and ask which to apply (AskUserQuestion multi-select where available, otherwise item numbers in plain text). Apply exactly the approved set. Approval from a past session never carries over.

**Unattended run** (no user present): apply the automatic class, report the approval-gated items, change nothing else.

## Step 9 - log and report

Append one dated entry to the lesson log (format below). Then report in at most six lines: lessons added or elevated, pending added and expired, repeats with their diagnosis, misses, global candidates, and approval items applied. When called by close-out, return the report and the approval list to close-out rather than delivering them yourself.

## File formats

**lessons.md** - loaded every session, so it stays compact. One line per lesson; evidence lives in the log.
```
# Lessons

Binding lessons for this project. Read before substantial work. Managed by pm-reflect.

- L-012 · 2026-11-08 · project · Never present a draft as done before the v2 voice rules pass - drafts kept going out with rule breaches. [repeats: 0]
```

**pending-lessons.md**
```
# Pending lessons

Observed patterns not yet proven. Do not treat as binding. Managed by pm-reflect.

## P-007 · Over-explains in plan summaries
- Proposed: Keep plan summaries to the decision and the next step.
- Scope: project
- Evidence: 2026-11-01 "just tell me what you're doing"; 2026-11-08 "shorter"
- Sessions: 2 of 3 · Runs since evidence: 0
```

**lesson-log.md** - when it gets long, roll entries older than 30 days into one summary line per month, in batches.
```
# Lesson log

## 2026-11-08
- Held: L-003, L-009
- Repeated: L-012 (not applied - wording too vague; sharpening proposed) · 2 correction turns
- Elevated: P-004 -> L-015
- Pending added: P-009 · Expired: P-002 (last evidence 2026-09-20)
- Misses: feedback on table formatting given 2026-10-30 and today, never captured
```

IDs are never reused. Elevated lessons keep a note of their pending ID in the log.

## Project memory format

In Project memory mode each store file needs the native header, and every content line is tagged `[stated]` and grounded in the user's own words:
```
---
name: lessons
description: Binding lessons for this project - read before any substantial work
sources: [chat]
aliases: [lessons.md, project lessons]
---
- [stated] L-012 · 2026-11-08 · project · Never present a draft as done before the v2 voice rules pass. [repeats: 0]
```
Use memory_str_replace or memory_append for single changes, passing the version token from the last read. Native memory never stores certain categories (health inferences, ID numbers and similar); if a feedback event touches one, leave it out rather than paraphrasing it. Files are size-capped: when a write reports a file near its cap, roll up the lesson log and condense pending evidence.

## Rules

- Match on underlying cause, never wording.
- Every pending lesson carries the user's own words as evidence.
- Explicit instructions go straight to lessons; only inferred patterns wait.
- Never delete or rewrite a lesson without approval given in this conversation.
- Never remove entries under 7 days old in hygiene.
- Lesson deletions need a sentence of justification - lessons earned their place by recurring.
- The measure is repeated feedback, not total feedback. Never trade away the user's stated preferences to reduce corrections.
