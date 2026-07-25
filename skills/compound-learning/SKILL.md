---
name: compound-learning
description: End-of-session memory updater. Reflects on the current Cowork conversation, extracts anything worth carrying forward, updates the project's memory.md file, and promotes recurring corrections into lessons.md. Use manually at the end of a working session, or set up as a daily scheduled task so memory stays current without you having to think about it.
allowed-tools:
  - Read
  - Write
  - Edit
  - Grep
---

# Compound Learning Skill

**Purpose:** Cowork doesn't auto-update memory the way Chat does. Every session starts cold unless something tells Claude what mattered last time. This skill is that something.

Run it at the end of a session — or schedule it to run at the end of every day — and it captures the parts of the conversation worth keeping into the project's `memory.md` file. Next time you open the project, Claude reads `memory.md` first and picks up where you left off.

## When to use

- Manually at the end of a working session (`/compound-learning` or "run compound learning")
- As a scheduled task — typically end of day, scoped to one or more project folders
- After a session where something material changed: a decision, a preference, a new piece of context, a recurring gotcha

## What it captures

Five things, scanned in this order:

1. **Decisions made** — anything the user committed to, even informally ("we'll go with option A", "skip that for now")
2. **Preferences expressed** — voice, tone, format, working style, tools, anything that should hold for future sessions
3. **Context the user added** — projects, deadlines, people, constraints, the why behind the work
4. **Gotchas and corrections** — moments where Claude went wrong and was corrected; the underlying reason matters more than the surface fix
5. **Open threads** — work-in-progress that should be picked up next session, with enough detail to restart cleanly

What it does NOT capture: the play-by-play of the conversation, code snippets that are already in files, summaries of what was done (the work itself is the record).

## Process

### Step 1: Read existing memory.md

Read the project's `memory.md` if it exists. If it doesn't, create one with this header:

```markdown
# Project Memory

Auto-updated by the compound-learning skill. Most recent entries at the top.
```

Scan existing entries so you don't duplicate them. If an entry covers the same ground as something new, update the existing entry rather than adding a duplicate.

### Step 2: Reflect on the session

Look back over the conversation. For each of the five capture categories above, ask:

- Is there something here a future session would benefit from knowing?
- Would not having this make Claude re-ask a question, repeat a mistake, or fail to apply something the user has already explained?
- Is this durable (a preference, a constraint, a decision) or ephemeral (a one-off task)?

Only keep durable items.

### Step 3: Write the entry

Add a new dated entry to the top of `memory.md`, under the header. Use this structure:

```markdown
## YYYY-MM-DD

**Decisions:**
- [decision 1]
- [decision 2]

**Preferences:**
- [preference 1]

**Context:**
- [new context 1]

**Corrections:**
- [thing I got wrong and why]

**Open threads:**
- [WIP item with enough detail to resume]
```

Drop any category that has no entries — don't write empty headers.

Keep each bullet short. One sentence. If something needs more explanation, link to the file or doc where the detail lives rather than repeating it.

### Step 3.5: Check for promotion to lessons.md

While writing a **Corrections** bullet, compare it against corrections already in `memory.md`. Match on the underlying cause, never the surface wording - "used em dashes again" and "wrong punctuation in the report" can be the same lesson.

If the same underlying correction has appeared before:

1. Write it to `lessons.md` as one dated line stating the lesson positively ("Always X" / "Never Y - it causes Z"). Create `lessons.md` if missing, headed:

   ```markdown
   # Lessons

   Durable lessons for this project - things that hold across sessions, promoted from memory.md when a correction or gotcha recurs. One line each, dated.
   ```

2. In the new memory entry, record the correction with the note "(promoted to lessons.md)" rather than logging the same mistake as though it were new.
3. Leave earlier occurrences in `memory.md` untouched - the history stays.

Promote only on recurrence. A one-off correction stays in memory; a lesson is something the project keeps teaching.

### Step 4: Confirm

Tell the user what was added. Show the new entry, and call out any promotion to lessons.md separately. Ask if anything should be edited or removed before you finish. Wait for response.

If the user says "looks good" or equivalent, finish. If they want changes, apply them and confirm again.

## Scheduling

To run this automatically:

1. In Cowork, set up a scheduled task: "Run /compound-learning in [project folder] every day at [time]"
2. Recommended time: end of your working day (e.g. 18:00)
3. The scheduled run will skip the confirmation step (no user present) — it commits the entry directly, dated and timestamped, and you can review next time you open the project

## Guardrails

**Do:**
- Keep entries durable and short
- Update existing entries rather than duplicating
- Link out to files for detail rather than copying detail in
- Date every entry
- Promote a correction to lessons.md the second time it appears, never the first

**Do not:**
- Capture conversation history as narrative
- Write entries longer than three lines per bullet
- Capture sensitive personal information (financial details, health, government IDs, addresses, passwords) unless the user explicitly asks
- Edit or remove existing entries without flagging the change

## Example output

```markdown
## 2026-06-01

**Decisions:**
- Landing page copy goes with version B; version A kept as a backup

**Preferences:**
- Draft files live in /drafts, named with the date first
- No pricing in any client-facing doc until sign-off

**Open threads:**
- Finish the pricing page copy
- Review the welcome email sequence before Friday
```
