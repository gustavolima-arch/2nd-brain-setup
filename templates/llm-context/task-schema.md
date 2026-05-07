# Task Management Schema

Loaded when triggering task/idea operations. All tasks and ideas live as individual .md files.

---

## Task frontmatter

```yaml
---
type: task
title: Human-readable task title
created: YYYY-MM-DD
due_date: YYYY-MM-DD
status: active | done | blocked | overdue
priority: high | medium | low
tags: [tag1, tag2]
context: "[[wiki/sources/slug]]"   # optional — what sparked this task
---

Task description and inline notes go here.
Add context as you work. Append, never delete.
```

**Rules when creating a task:**
- Always infer tags from content (see tag taxonomy below)
- Always ask for `due_date` if not provided — never create a task without one
- Always infer `priority` from context (explicit request → high; routine → medium; someday → low)
- Link `context` to a wiki source or concept if the task came from a discussion or ingest
- Filename: `tasks/YYYY-MM-DD-<slug>.md` (date prefix makes tasks sortable and dynamic)

---

## Idea frontmatter

```yaml
---
type: idea
title: Human-readable idea title
created: YYYY-MM-DD
status: backlog | in-progress | done | dropped
tags: [tag1, tag2]
sparked_by: "[[wiki/sources/slug]]"   # optional — what conversation or source generated this
---

Idea description and evolving notes.
```

**Rules when creating an idea:**
- No due_date required (ideas are timeless; tasks have deadlines)
- `sparked_by` links to the wiki source, concept, or describes the conversation that generated it
- Status defaults to `backlog`; change to `in-progress` when you start acting on it
- Filename: `ideas/YYYY-MM-DD-<slug>.md`

---

## Tag taxonomy

Customize these tags to match your role. Examples:

| Tag | Use for |
|---|---|
| `product` | Product management work, PRDs, roadmap, discovery |
| `strategy` | Strategic thinking, business decisions, positioning |
| `learning` | Study, courses, tools, frameworks to act on |
| `people` | 1:1s, hiring, stakeholder management |
| `research` | Papers, market research, user research |
| `writing` | Docs, proposals, content creation |
| `admin` | Administrative, operational, logistical |

Add or remove tags to match your domain. Update `work-context.md` with the final taxonomy.

---

## Daily files

All daily files live in `daily/YYYY-MM-DD.md`. Never write to vault root.

### File structure

```markdown
# YYYY-MM-DD

> [Day summary — 2–3 sentences: what to expect, top priority, what might complicate it. Generated at session start.]

---

## 🔴 NOW (before HH:MM)

| | |
|---|---|
| **[Urgent item]** | [[tasks/slug]] — context, exact deadline, what needs to be ready |

*(omit this section if nothing is urgent at session start)*

---

## Today's Meetings

| Time | Meeting | Action needed |
|---|---|---|
| HH:MM–HH:MM | Meeting name | Action or "Recurring — no action" |

*(pull from calendar sync if available; otherwise omit)*

---

## Tasks Due Today

- [ ] [[tasks/slug]] — title *(priority)*

## Overdue

- [ ] [[tasks/slug]] — title *(due: YYYY-MM-DD)*

## Coming Up (next 7 days)

| Task | Due | Priority |
|---|---|---|
| [[tasks/slug\|title]] | YYYY-MM-DD | high/medium/low |

## High Priority Backlog

| Task | Notes |
|---|---|
| [[tasks/slug\|title]] | short note |

---

## Log — Session 1
*(appended at session end — detailed: discussions, decisions, reasoning, context)*

## Takeaway — Session 1
*(appended at session end — 2–3 sentence summary of this session)*

## Log — Session 2
*(if multiple sessions in a day — increment N)*

## Takeaway — Session 2

## Takeaway of the Day
*(only on last session of the day — synthesis across all sessions)*
```

### Morning generation (session start)

Write `daily/YYYY-MM-DD.md` with the top half only (summary through High Priority Backlog). Do NOT write Log or Takeaway yet.

**Day summary** — 2–3 sentences opening the file: top priority, what might complicate things, general tone. Generated after reading tasks + calendar.

**🔴 NOW** — any item with a deadline within ~2 hours of session start (imminent meeting, pending material, urgent action).

**1. Due Today** — `tasks/` where `due_date = today` AND `status = active`.
**2. Overdue** — `tasks/` where `due_date < today` AND `status = active`. Update those files to `status: overdue`.
**3. Coming Up** — `tasks/` where `due_date` within next 7 days AND `status = active`. Exclude due-today items.
**4. High Priority Backlog** — `tasks/` where `priority = high` AND `status = active` AND `due_date > today`. Top 5 max.

### Missing day backfill

If `daily/` has no file for one or more days since the last session:
1. Create `daily/YYYY-MM-DD.md` for each missing day
2. Mark it: `> *No session recorded.*`
3. List any tasks that were due on that day
4. Carry those tasks forward as overdue (update their frontmatter)

### End-of-day completion (session end)

Append to the existing daily file:
- `## Log — Session N` — detailed session log (discussions, decisions, reasoning)
- `## Takeaway — Session N` — 2–3 sentences on this session
- `## Takeaway of the Day` — only if you signal this is the last session of the day

Increment N by checking how many `## Log — Session` blocks already exist.

---

## new task: [X]

1. Parse: title, due date (if given), context clues for tags and priority
2. If no due date given: ask "When is this due?" before creating
3. Create `tasks/YYYY-MM-DD-<slug>.md` with full frontmatter
4. If the task was sparked by current wiki session or ingest: link `context` to the relevant source/concept
5. If due_date = today: add to today's file (if it exists) under "Due Today"
6. Confirm: "Task created: [title] — due [date]"

---

## new idea: [X]

1. Parse: title, context clues for tags
2. Capture `sparked_by` from current conversation — what were we discussing?
3. Create `ideas/YYYY-MM-DD-<slug>.md` with full frontmatter
4. Confirm: "Idea saved: [title]"

---

## done: [X]

1. Find the task by title or slug (natural language search ok)
2. Update `status: done` and add `completed: YYYY-MM-DD` to frontmatter
3. If task is in today's file: check it off
4. Confirm: "Done: [title]"

---

## what's my [area] pipeline?

1. Search `tasks/` for `tags` matching [area] AND `status = active | overdue`
2. Sort by due_date ascending
3. Report as a list with title, due date, status, and priority

---

## help me find [X]

Natural language search across `tasks/` and `ideas/`:
1. Search by title keywords, body content, tags
2. If no exact match: try variations
3. Return top 3 matches with file paths
4. Ask: "Is this what you're looking for?"

---

## Dynamic linking rules

- If a task was created during or after an **ingest**: set `context` to the ingested source
- If a task is about a **concept** tracked in the wiki: mention the concept page in the task body
- If an **idea** emerged from a synthesis page: link `sparked_by` to that synthesis
- When running `today`: surface the `context` link next to relevant tasks
