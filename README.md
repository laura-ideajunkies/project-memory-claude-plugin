# project-memory

Three skills that stop agent sessions starting cold.

Claude now has built-in memory in claude.ai chats, in cloud Cowork sessions and in Projects, saved as topics while you chat and held in your Claude account. That memory is not in the folder. It cannot be versioned with the work, read by a teammate or another tool, or audited line by line, and sessions that run only on your computer (legacy local-only Cowork tasks, and third-party or self-hosted desktop deployments) do not use it. A Cowork session on a local folder runs in the cloud and does use it. So decisions still get re-litigated, preferences re-stated, the same mistakes corrected twice. These three skills fix that with three plain markdown files per project folder and a routine that keeps them current - and the corrections compound: make the same mistake twice and it becomes a standing lesson the project never forgets.

## The three files

- **`CLAUDE.md`** - standard project instruction file created by Claude Cowork/ Code; standing context. What the project is, who's involved, how to work in this folder. Loads every session.
- **`memory.md`** - the running log. Dated entries, newest first: decisions, preferences, corrections, open threads. What was true lately.
- **`lessons.md`** - the distillation. When a correction in memory recurs, it graduates here as one dated line. What stays true.

Meeting notes versus house style guide: notes tell you what happened lately; the style guide exists so nobody repeats a mistake often enough to need notes about it.

## The three skills

**`project-init`** - run it in any new folder (Cowork cloud folder or local repo). It checks what already exists, interviews you briefly - one question at a time, skipping anything the conversation already answers - then scaffolds the three files, wired together. Idempotent: it fills gaps and never overwrites.

**`compound-learning`** - run it at the end of a working session, or schedule it daily. It reflects on the conversation, writes a dated entry to `memory.md` (decisions, preferences, context, corrections, open threads - never the play-by-play), and checks each new correction against the log. Second occurrence of the same underlying mistake: it writes the lesson to `lessons.md`, marks the promotion, and keeps the history.

The promotion threshold is deliberate. A one-off correction stays in memory; a lesson is something the project keeps teaching.

**`pm-reflect`** - run it occasionally (`/pm-reflect`) as the prune to compound-learning's grow. It reads all three files, flags entries that look stale - closed threads, superseded decisions, passed dates, duplicates, contradictions - and presents recommendations. Nothing is changed without your explicit approval, and unattended runs only report, never edit. Each prune leaves a dated trace in `memory.md`.

## How this fits with Claude's built-in memory

| | Built-in memory | project-memory files |
|---|---|---|
| Where it lives | Your Claude account (account memory, per-project memory) | `memory.md` and `lessons.md` in the project folder |
| Who writes it | Claude, as you chat, or when you say "remember this" | Claude, when you run compound-learning |
| Reviewable | Settings > Your Claude > Memory, as topics | Open the file; prune with pm-reflect |
| Travels with the work | No | Yes - commit it, share it, open it in any tool |
| Works in computer-only sessions (legacy local-only tasks, third-party or self-hosted desktop) | No | Yes |

Use both. Built-in memory catches what you say in passing. A session reads the files at the start, and they compound into lessons.

## Install

**Claude.ai, Claude Desktop and mobile** - the repo doubles as a plugin marketplace. Open **Customize > Plugins**, select **Add marketplace**, paste `https://github.com/laura-ideajunkies/project-memory-claude-plugin`, then install `project-memory`. A plugin installed this way is saved to your account and loads in chat, Cowork sessions, scheduled tasks and Claude Code cloud sessions on every device.

Or upload the `.skill` files in `dist/` one at a time under **Customize > Skills**.

**Claude Code CLI**:

```
/plugin marketplace add https://github.com/laura-ideajunkies/project-memory-claude-plugin
/plugin install project-memory@project-memory
```

Or copy the folders from `skills/` into `~/.claude/skills/`. Skills that exist only in `~/.claude/skills/` or in a repository's `.claude/skills/` do not load in Cowork, cloud sessions or cloud scheduled tasks - enable the plugin on your account for those.

## Use

In a new project folder: "set up this project". Answer the questions. From then on, end sessions with "run compound learning" - or schedule it daily and stop thinking about it.

Scheduled tasks run in the cloud by default. A cloud run reaches a local folder only through the open Claude Desktop app, so for a local folder leave the app open at the scheduled time; a Cowork project folder or an attached repository needs nothing open.

In a Claude Code Project (beta) with no repository, the skills use the project's own memory files and instructions instead of creating a parallel set. Checked against Anthropic's Cowork and memory documentation as of October 2026.

## Structure

```
project-memory-claude-plugin/
├── .claude-plugin/          # plugin + marketplace manifests
├── skills/
│   ├── project-init/SKILL.md
│   ├── compound-learning/SKILL.md
│   └── pm-reflect/SKILL.md
├── dist/                    # packaged .skill files for Claude.ai / Cowork
├── LICENSE
└── README.md
```

## Licence

MIT. Fork and adapt freely.
