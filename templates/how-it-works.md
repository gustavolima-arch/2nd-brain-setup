---
title: How Your 2nd Brain Works
type: concept
domain: work
tags: [knowledge-management]
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
---

# How Your 2nd Brain Works

Your 2nd Brain is a personal knowledge system that lives on your Mac at `~/Documents/2nd-brain/`. You feed it information — articles, PDFs, meeting notes, ideas — and Claude organizes, connects, and surfaces it for you.

---

## The four folders you use directly

| Folder | What it's for |
|---|---|
| `raw/` | Your inbox. Drop any file here: PDFs, articles, notes, clippings. Claude processes them. |
| `tasks/` | One file per task. Claude creates and manages these — you never edit directly. |
| `ideas/` | Same as tasks, but for ideas with no deadline. |
| `wiki/` | Your knowledge base. Claude owns this entirely. Don't edit files here manually. |

---

## Your daily routine — 3 commands

| Command | What happens |
|---|---|
| `today` | Claude shows what's due, overdue, today's meetings and priorities |
| `new task: [description]` | Claude creates a task file and asks for the due date |
| `bye` | Claude logs the session, updates the wiki, and syncs everything to GitHub |

---

## All commands

### Tasks & ideas
| Command | What happens |
|---|---|
| `new task: [description]` | Creates a task file, asks for due date if not given |
| `new idea: [description]` | Saves an idea linked to the current conversation |
| `done: [task name]` | Marks a task complete |
| `what's my [topic] pipeline?` | Lists all active tasks for that topic |
| `help me find [X]` | Natural language search across tasks and ideas |
| `today` | Generates the daily view: due, overdue, coming up, high-priority backlog |

### Wiki & knowledge
| Command | What happens |
|---|---|
| `ingest` | Processes everything in `raw/` into your wiki |
| `ingest [file or URL]` | Processes one specific source — Claude asks you questions before filing |
| `save` / `capture` | Saves the current conversation as a wiki page |
| `lint` / `health check` | Audits the wiki for broken links, orphan pages, stale content |

### General
| Command | What happens |
|---|---|
| `bye` / `done` / `valeu` | Ends the session: logs, updates wiki, syncs to GitHub |
| Any question | Claude queries the wiki and answers with citations |

---

## How the wiki works

Claude maintains five types of pages:

| Type | What it contains |
|---|---|
| `sources/` | One page per ingested file or article — summary, key claims, connections |
| `entities/` | People, tools, organizations you work with |
| `concepts/` | Ideas, frameworks, mental models |
| `synthesis/` | Answers to questions, strategic analysis, cross-source reasoning |
| `overview.md` | Master synthesis — the big picture as it evolves |

You never create these manually. Say `ingest` or `capture` and Claude does the filing.

---

## The git backup

Every session that ends with `bye` automatically:
1. Stages all changes (`git add -A`)
2. Commits with a timestamp
3. Pushes to your private GitHub repo

If your Mac is lost or broken, your entire vault is recoverable with one command:
```bash
git clone https://github.com/[your-username]/2nd-brain
```

---

## How to open Claude Code

Always open Claude Code from `~/Documents/` — the folder that contains `CLAUDE.md`. That file is what tells Claude how to behave as your 2nd Brain agent.

**Not** from inside `~/Documents/2nd-brain/` — that's the vault itself, not the agent entry point.

Quick way on Mac: open Terminal → `cd ~/Documents && claude`

---

## Sources

- Installed during setup on YYYY-MM-DD
