# 2nd Brain — Setup Wizard

A personal knowledge system built on **Obsidian** + **Claude Code**. Captures, connects, and surfaces your work knowledge. Syncs automatically to GitHub.

This repo contains a setup wizard that installs and configures everything automatically — you only need to paste one command.

---

## What you get

- Structured Obsidian vault (wiki, tasks, ideas, daily notes)
- Claude as your personal knowledge agent — ingests sources, answers questions, manages tasks
- Automatic git backup at the end of every session
- Obsidian Clipper to save web pages in one click

## Prerequisites

- Mac
- [Claude Code](https://claude.ai/code) installed

That's it. The wizard handles everything else: Obsidian, Homebrew, git, GitHub CLI, repo creation.

---

## Install

Open Terminal and paste:

```bash
cd ~/Desktop && curl -L https://github.com/gustavolima-arch/2nd-brain-setup/archive/refs/heads/main.zip -o setup.zip && unzip setup.zip && mv 2nd-brain-setup-main 2nd-brain-setup && rm setup.zip && cd 2nd-brain-setup && claude
```

Claude opens, reads the setup instructions, and walks you through the rest — takes about 30 minutes.

---

## What the wizard does

| Step | What happens |
|---|---|
| 0 | Welcome + explains how the system works |
| 1 | Installs Obsidian (if needed) |
| 2 | Creates vault structure at `~/Documents/2nd-brain/` |
| 3 | Copies context templates |
| 4 | Configures the agent instruction file |
| 5 | Creates GitHub account (if needed) |
| 6 | Installs Homebrew + gh CLI, authenticates GitHub, creates private backup repo |
| 7 | Installs Obsidian Clipper |
| 8 | Opens first real session |
| 9 | Deletes itself — vault is clean |

The only things you do manually: Mac password once (Homebrew) and a browser interaction to authenticate GitHub.

---

## Daily workflow

| Command | What happens |
|---|---|
| `today` | Shows what's due, overdue, meetings, priorities |
| `new task: [X]` | Creates a task file |
| `ingest` | Processes files dropped in `raw/` into the wiki |
| `bye` | Logs session + syncs to GitHub |

---

## After install

Your vault lives at `~/Documents/2nd-brain/`. Open Claude Code from `~/Documents/` (where `CLAUDE.md` is) to start each session.
