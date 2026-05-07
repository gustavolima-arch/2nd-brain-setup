# [YOUR NAME]'s 2nd-brain — Agent Instructions

Routing document. Detailed schemas live in `llm-context/`. Load only what's relevant to the task.

> **Setup:** Replace every `[PLACEHOLDER]` in this file before your first session.

---

## Vault layout

```
[YOUR VAULT NAME]/
├── raw/                    ← Drop sources here. Read-only. Move to raw/archive/ after processing.
│   ├── assets/             ← Media and file attachments. Also scanned on bulk ingest.
│   └── archive/            ← Processed sources. Never modify.
├── tasks/                  ← One .md file per task. Dynamic — date-aware, context-linked.
├── ideas/                  ← One .md file per idea. Linked to sparking source or discussion.
├── daily/                  ← One .md file per day. Auto-generated at session start.
├── llm-context/            ← Context library. Load files as needed per task type.
│   ├── index.md            ← Map of all context files
│   ├── wiki-schema.md      ← Full wiki operations (INGEST, QUERY, CAPTURE, LINT, templates)
│   ├── task-schema.md      ← Task management system (today, new task, new idea, due dates)
│   └── work-context.md     ← Your work context (role, team, priorities, stakeholder framing)
└── wiki/                   ← Wiki pages (owned and maintained by Claude)
    ├── index.md            ← Content catalog. Update on every wiki operation.
    ├── log.md              ← Append-only event log. Update on every wiki operation.
    ├── overview.md         ← Master synthesis. Update when the picture shifts.
    ├── hot.md              ← Current state snapshot. Always loaded at session start.
    ├── sources/
    ├── entities/
    ├── concepts/
    └── synthesis/
```

---

## Context routing

| Task type | Load |
|---|---|
| Wiki operations (ingest, query, capture, lint) | `llm-context/wiki-schema.md` |
| Task / idea management | `llm-context/task-schema.md` |
| Work strategy, proposals, stakeholder comms | `llm-context/work-context.md` |

---

## Triggers

### Wiki operations
- `ingest [file or URL]` → single source INGEST (wiki-schema.md)
- `ingest` *(no argument)* → bulk INGEST — scan raw/ and raw/assets/ for all files, ingest each sequentially, report summary
- `save` / `capture` / `save as [type]` → CAPTURE (wiki-schema.md)
- Question about wiki content → QUERY (wiki-schema.md)
- `lint` / `health check` → LINT (wiki-schema.md)

### Task management
- `today` → generate/show daily file (task-schema.md)
- `new task: [X]` → create task with due date prompt if not given (task-schema.md)
- `new idea: [X]` → create idea linked to current context (task-schema.md)
- `what's my [area] pipeline?` → query tasks by tag (task-schema.md)
- `help me find [X]` → natural language search across tasks/ and ideas/ (task-schema.md)
- `done: [X]` → mark task as complete (task-schema.md)

---

## Session start

On the first message of a new session:
0. **Check for setup folder** — if `[YOUR VAULT NAME]/2nd-brain - Quick Setup/` exists, stop and run the setup wizard in that folder's `CLAUDE.md` before doing anything else. The vault is not ready until setup is complete.
1. Read `wiki/hot.md` — current state snapshot (fast, always loaded).
2. Read `wiki/log.md` (last 10 entries) — recent activity.
3. **Scan `daily/` for gaps** — check the last file date. If days are missing since the last session, generate a file for each missing day: mark "No session recorded", carry forward any tasks that were due on those days as overdue in `tasks/` (update their `status: overdue`).
4. **Check yesterday's daily file** — if it exists but has no `## Log` or `## Takeaway`, reconstruct from `wiki/log.md` entries for that date and fill it in.
5. **Generate today's daily file** at `daily/YYYY-MM-DD.md` if it doesn't exist yet — morning view only (tasks due today, overdue, coming up, high priority backlog). Do not append Log/Takeaway yet.
6. Read `wiki/index.md` — full content map (only if needed for the task).
7. Proceed with the request.

---

## Session end

Triggered automatically when the conversation signals it's closing — any of: "bye", "done", "cya", "that's it", or similar closing signal in any language.

Run this sequence without being asked:
1. **Append `## Log — Session N` to `daily/YYYY-MM-DD.md`** — detailed log of what happened this session: what was discussed, decisions made, reasoning, context that doesn't fit in `wiki/log.md`. Be specific. Check if `## Log — Session 1` already exists — if so, increment N.
2. **Append `## Takeaway — Session N`** — 2–3 sentences: how this session was spent, what moved, what's still open.
3. **If this is the last session of the day** (signal it explicitly, e.g. "last one today"): append a `## Takeaway do Dia` — synthesis across all sessions, overall what moved today.
4. **Update `wiki/hot.md`** — only ADD or UPDATE entries; never remove items that a concurrent session may have added. Keep under ~500 words.
5. **Update `llm-context/` files** if anything changed that affects future sessions.
6. **Create or update wiki pages** if new knowledge emerged that warrants a page.
7. **Append to `wiki/log.md`** — one line per operation, overview level only (not session detail).
8. **Git sync** — stage all changes and commit with a timestamped message, then push to origin:
   ```bash
   cd "[YOUR VAULT FULL PATH]"
   git add -A
   git commit -m "sync: $(date '+%Y-%m-%d %H:%M') — session end"
   git push origin main
   ```
   If there are no changes (`git status` is clean), skip the commit. Never fail silently — if push fails, surface the error.

**Two-tier logging:**
- `wiki/log.md` = overview, one line per operation, grep-friendly
- `daily/## Log — Session N` = full detail: discussions, decisions, reasoning, context

If the session ends without a close signal, the next session start reconstructs and fills in the gap.

---

## Entity creation policy

Create an entity page when the subject is:
- A primary contributor or subject of a source
- Appearing across multiple sources
- Directly informing a concept or synthesis
- A person, tool, or system you are actively working with

When in doubt: create the page. A stub is cheaper than a lost connection.

---

## Always rules
- **Never summarize what you just did** at the end of a response. The diff speaks for itself.
- **Never assume time-sensitive state** — run `date` (Bash) whenever the current time matters. If state still can't be determined, ask. Never guess.
- **Never modify** files in `raw/`. Read-only.
- **Always `mv`, never `rm`**: use a single `mv` command for all file moves. Never delete.
- **Archive after ingest**: `mv` raw files to `raw/archive/` immediately after processing.
- **No broken links**: if you reference a page, it must exist first.
- **Always use `[[wiki links]]`** for internal references. Never use bare URLs for internal pages.
- **One file owns each fact** — never duplicate data across wiki files. If hot.md mentions a deadline or status, it links to the task/page that owns it, not restates it.
- **No orphan pages**: every wiki page needs at least one inbound link.
- **Bidirectional links**: if a task links to a source, the source must link back — always two-way.
- **Update `wiki/index.md` and `wiki/log.md`** at the end of every wiki operation.
- **Update `wiki/hot.md` and `wiki/log.md` immediately** — in the same response as any task or wiki change, not deferred to session end.
- **Proactively surface implied updates** — if new information suggests a deadline should shift, a task status is stale, or hot.md no longer reflects reality: flag it and ask, or update directly if the change is unambiguous.
- **Prefer updating** existing pages over creating new ones.
- **Ask rather than guess** when domain, placement, or scope is ambiguous.
- **Daily files** always go in `daily/YYYY-MM-DD.md`. Never write to vault root.
- **File answers back** — good analyses belong in the wiki, not just in chat history.
- **Resolve contradictions explicitly** — when new source conflicts with existing wiki content, update the affected page and note the conflict in the source summary.
- **Ask before bulk-deleting** anything from the wiki. Deletions require owner confirmation.

---

## Domain classification

**`domain: work`** — all content in this vault is work domain by default.
Customize sub-tags to match your role (examples: `product`, `engineering`, `research`, `strategy`, `admin`).

---

## Language

Respond in whatever language you use. Internal docs follow your team's convention.
