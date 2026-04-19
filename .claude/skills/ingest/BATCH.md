---
name: ingest-batch
description: Batch mode supplement for the ingest skill. Load this file when a _batch-plan.md exists, there are 10+ unprocessed sources, or the user says "create a batch plan" / "prepare for ingestion" / "process the next batch". Covers session state recovery, batch workflow, log format, progress tracking, and plan generation.
---

# Batch Mode Ingest

Supplement to the ingest skill (`SKILL.md`). Each batch should run in a **fresh session** — the wiki is the persistent memory, not conversation history.

---

## Session Start: State Recovery

Read these files in order — this is your full state:

1. `TOPIC.md` — domain rules, layout, page conventions, ingest notes
2. `wiki/index.md` — what wiki pages currently exist
3. `wiki/log.md` (last ~10 entries) — what happened recently
4. `wiki/_batch-plan.md` — find the next unchecked batch (first `- [ ]` group)

If existing wiki pages are relevant to the current batch (e.g., a principle page you'll be updating), read those too. Don't load all wiki pages — only the ones the batch will touch. Use `index.md` to identify them.

---

## Batch Workflow

Process all sources in the batch together, not sequentially:

1. **Read all sources** in the batch completely (Phase 2 for each)
2. **One combined discussion** (Phase 3) — cover the batch as a group: what themes emerge, what connects them, what's new vs. what reinforces existing wiki content. **Skipped by default** (auto-mode) — see Phase 3 in SKILL.md. Only engage if the user says "discuss mode" or if genuine ambiguity requires input (use `vscode_askQuestions` inline).
3. **Create all source summaries** (Phase 4 for each source)
4. **Integrate into wiki pages** (Phase 5) — holistic update, not per-source:
   - Group related claims from across the batch before writing to any page
   - A concept/principle page gets one coherent update from the batch, not N separate edits
   - Cross-reference within the batch: if source A introduces an idea and source B extends it, the wiki page should reflect both
5. **Update standalone pages** — if the topic has standalone pages (e.g., `quotes.md`, `timeline.md`), add new entries from this batch
6. **Bookkeep** (Phase 6) — one combined log entry for the batch; update index with all new pages

---

## Batch Log Entry Format

```markdown
## [YYYY-MM-DD] ingest-batch | Batch N — <brief description>

- Sources: raw/articles/source-1.md, source-2.md, source-3.md
- Created: sources/source-1, sources/source-2, sources/source-3, folder/new-page
- Updated: folder/existing-page, folder/existing-page, quotes, timeline
- Notes: <contradictions found, judgment calls made, questions filed>
```

---

## Marking Progress

After the batch is fully processed and bookkeeping is done, check off completed sources in `wiki/_batch-plan.md`:

```markdown
- [x] source-file-1.md
- [x] source-file-2.md
- [x] source-file-3.md
```

---

## Generating a Batch Plan

Triggered when: user says "create a batch plan" / "prepare for ingestion", or Phase 1 finds 10+ sources and the user agrees to batch mode.

**Step 1 — Inventory unprocessed sources.**
List all files in `raw/articles/`, `raw/papers/`, `raw/notes/`. Move any unsorted files from `raw/` root to the correct subfolder first. Exclude `.gitkeep` and `raw/assets/`. Check `wiki/sources/` to filter out already-ingested files.

**Step 2 — Identify natural ordering.**
Look for an inherent sequence: publication date, numbered chapters, alphabetical series, etc. If one exists, process in that order — later sources often reference earlier ones. If no ordering is obvious, group thematically.

**Step 3 — Estimate file sizes.**
Check line counts to gauge reading load:

```bash
wc -l raw/articles/*.md | sort -n
```

- Short (<300 lines): up to 5 per batch
- Medium (300–1000 lines): 3–4 per batch
- Long (>1000 lines): 2–3 per batch
- When in doubt, default to 3

**Step 4 — Draft the batch plan.**
Group files into batches under descriptive headings (era, theme, source type). Add a Post-Ingestion section at the end.

```markdown
# Batch Ingestion Plan

## Batch 1 — <brief description>

- [ ] source-a.md
- [ ] source-b.md
- [ ] source-c.md

## Batch 2 — <brief description>

- [ ] source-d.md
      ...

## Post-Ingestion

- [ ] Run lint health check
- [ ] Review and polish key pages
```

**Step 5 — Present to the user before writing.**
Show the proposed batching with batch sizes and rationale. Ask if the grouping makes sense and if any adjustments are needed. Write `wiki/_batch-plan.md` only after the user confirms.
