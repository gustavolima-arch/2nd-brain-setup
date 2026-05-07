# Wiki Agent Schema — 2nd Brain

Full operational spec for wiki operations. Loaded when triggering INGEST, QUERY, CAPTURE, or LINT.

---

## Naming conventions

- All wiki filenames: lowercase, hyphens for spaces. Example: `product-strategy.md`, `q2-review.md`
- Source pages: match the source filename. Example: `raw/archive/prd-template.md` → `wiki/sources/prd-template.md`
- Log entries: `## [YYYY-MM-DD] <operation> | <title>`
- No spaces in any filenames.

---

## Page frontmatter

Every wiki page must start with YAML frontmatter:

```yaml
---
title: Human-readable page title
type: source | entity | concept | synthesis | overview
domain: work
tags: [tag1, tag2]
authors: ["[[wiki/entities/slug|Name]]"]   # source pages only; always aliased wikilinks
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: [source-slug-1, source-slug-2]    # non-source pages only
---
```

---

## Markdown conventions

- Internal links: `[[wiki/path/slug]]` (Obsidian wikilinks, relative to vault root)
- Aliased links: `[[wiki/entities/slug|Human Name]]`
- Callout blocks: `> [!NOTE]`, `> [!WARNING]`, `> [!CONTRADICTION]`
- Contradictions: add `> [!CONTRADICTION]` to **both** conflicting pages, naming the other
- Every page ends with a **Sources** section listing `[[wiki/sources/slug]]` links

---

## INGEST — single source

Two modes:
- **Manual** (`ingest [file or URL]`): follow all steps including Step 2.
- **Bulk** (`ingest` with no argument): batch Step 2 questions, then process sequentially.

**Step 1 — Read.** Read the full source file.

**Step 2 — Ask questions. (Manual mode only)**
Pick the 3–5 most relevant:
- "Why did you save this — what do you want to get out of it?"
- "Any specific angle or section to emphasize? Anything to skip?"
- "Does this connect to a project or question you're currently working on?"
- "Should this link to anything specific already in the wiki?"

Wait for answers before proceeding.

**Step 2b — Archive.** Move source from `raw/` to `raw/archive/` using `mv`.

**Step 3 — Write source page.** Create `wiki/sources/<slug>.md`:
- Frontmatter with `authors` as aliased wikilinks. Create entity pages first if they don't exist.
- **Summary** (3–5 paragraphs): what the source is, main argument, key evidence, notable details
- **Key claims** (bullets): most citable facts and assertions
- **Connections** (bullets): links to existing wiki pages
- **Gaps / open questions**: what the source leaves unresolved
- **Raw source**: `[[raw/archive/<filename>]]`

**Step 4 — Update entity and concept pages.**
- Exists → read, integrate new info, add source to sources list
- Doesn't exist → create it (see templates below)
- Target: 5–15 page touches per ingest.

**Step 5 — Update `wiki/overview.md`** if the source meaningfully shifts the overall picture.

**Step 6 — Update `wiki/index.md`.** Add source page, update changed entity/concept entries.

**Step 7 — Append to `wiki/log.md`.** Entry: `## [YYYY-MM-DD] ingest | <Title>` + 2-sentence summary.

**Step 8 — Report.** What was created, what was updated, one observation connecting this source to existing knowledge.

---

## INGEST — bulk (no argument)

1. List all files in `raw/` (excluding `raw/archive/`).
2. If none: report "raw/ is empty — nothing to ingest."
3. If files exist: confirm the list ("Found X files: [list]. Ingest all?")
4. For each file: run single INGEST, batch the Step 2 questions.
5. After all: report summary (total pages created/updated, key connections made).

---

## QUERY — answering questions

1. Read `wiki/index.md` to find relevant pages.
2. Read the relevant pages.
3. Synthesize an answer. Cite with `[[wiki/sources/slug]]` links.
4. Offer to file the answer as a `wiki/synthesis/` page for non-trivial synthesis.
5. Append to `wiki/log.md`: `## [YYYY-MM-DD] query | <Question summary>`.

---

## CAPTURE — saving from conversation

Trigger: say `save`, `save this`, `capture`, or `save as [type]`.

1. **Identify.** Core knowledge? Page type?
2. **Confirm.** Proposed title, type, 2–3 sentence summary. Ask: "Anything to add before I file this?"
3. **Write.** Use appropriate template. For conversation-sourced pages, use `## Origin: Captured from conversation on YYYY-MM-DD.`
4. **Update `wiki/index.md` and `wiki/log.md`.**

---

## LINT — health check

Check and report:
1. **Contradictions**: conflicting claims between pages
2. **Stale claims**: `updated` dates older than 60 days that may be superseded
3. **Orphan pages**: no inbound links from other wiki pages
4. **Missing pages**: concepts/entities mentioned in multiple pages but lacking their own page
5. **Missing cross-references**: pages that should link to each other but don't
6. **Data gaps**: questions the wiki can't answer yet — suggest sources to find

After reporting, ask which issues to fix.

---

## Page templates

### Entity page
```markdown
---
title: Entity Name
type: entity
domain: work
tags: [person|org|tool|project]
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
---

# Entity Name

One-sentence description.

## Overview
2–4 paragraphs.

## Key facts
- Fact 1

## Connections
- [[wiki/concepts/related]] — why connected

## Open questions
- Question 1

## Sources
- [[wiki/sources/slug]]
```

### Concept page
```markdown
---
title: Concept Name
type: concept
domain: work
tags: [tag1, tag2]
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
---

# Concept Name

One-sentence definition.

## Explanation
2–4 paragraphs.

## Evidence and examples
- Example — [[wiki/sources/slug]]

## Connections
- [[wiki/entities/related]]

## Sources
- [[wiki/sources/slug]]
```

### Synthesis page
```markdown
---
title: Synthesis Title
type: synthesis
domain: work
tags: [tag1, tag2]
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
---

# Synthesis Title

**Question:** ...
**Answer:** ...

## Analysis
...

## Sources drawn on
- [[wiki/sources/slug]]
```

---

## Domain and focus

**All content in this vault is `domain: work`.**

**Source types to prioritize:**
- PRDs and specs — extract patterns, logic gaps, dependencies
- Meeting notes and decisions — capture outcomes, owners, open questions
- AI tools and frameworks — evaluate fit for your domain
- Research papers and articles — extract the "so what" and connect to ongoing work
- Competitor or market research — link to strategic context

**Emphasis during ingest:**
- PRDs/specs → surface missing edge cases, unstated assumptions, data dependencies. Flag gaps proactively.
- Meeting notes → who decided what, what's still open, what needs follow-up.
- Tools → what problem they solve, fit for your context, integration complexity.
- Research → core contribution, practical implication, connection to current work.
