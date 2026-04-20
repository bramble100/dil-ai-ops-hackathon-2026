---
name: ingest
description: Processes raw source documents into the wiki - creates source summaries, updates wiki pages, maintains index and log. Discovers unprocessed sources when no file is specified. Supports batch mode for topics with many sources (uses _batch-plan.md). Use when the user adds a source, drops a file into raw/, or says "ingest". Triggers on phrases like "ingest", "process this source", "process this file", "add this to the wiki", "what's unprocessed", "process the next batch".
---

# Ingest Source

**Purpose:** Process a raw source document into a structured, interlinked wiki entry - creating a summary, updating wiki pages, and maintaining the index and log.

## When to Apply

- User says "ingest", "process", or "add to the wiki"
- User drops a file into `raw/` and asks to process it
- User provides a URL to capture and process
- User asks what sources haven't been processed yet

---

## Workflow Overview

1. **Discover** - If no file specified, find unprocessed sources
2. **Read** - Read the source completely
3. **Discuss** - Brief conversation about key takeaways
4. **Summarize** - Create a source summary page
5. **Integrate** - Update wiki pages
6. **Bookkeep** - Update index and log

→ **Batch mode** (10+ sources or `_batch-plan.md` exists): read `.claude/skills/ingest/BATCH.md` before proceeding.

---

## Phase 1: Discover Unprocessed Sources

**When:** The user says "ingest" without naming a specific file.

**Step 1 — Sort unsorted files.** Check for files dropped directly into `raw/` (the root, not a subfolder). Before moving anything, read `source_path` frontmatter from every file in `wiki/sources/` and treat any root-level file already referenced there as already ingested. Only classify and move root-level files that are **not** present in any `source_path`, moving them to the appropriate subfolder (`articles/`, `papers/`, `notes/`, or `assets/`). Leave referenced files in place unless you are also updating every affected `wiki/sources/*` `source_path`. Confirm the classification with the user before moving. This keeps `raw/` organized without breaking existing source references.

**Step 2 — Find unprocessed sources.**

1. List all files in `raw/` across subdirectories (articles, papers, notes). Exclude `.gitkeep` and files in `assets/`.
2. Read `source_path` frontmatter from every file in `wiki/sources/`.
3. Compare the two lists. Present unprocessed files as a numbered list.
4. **Batch plan check:** If there are 10 or more unprocessed files, or if a quick size check (`wc -l` across the files) suggests a total reading load above ~3000 lines, pause before asking which files to process. Explain the context degradation risk and recommend batch mode: _"There are N unprocessed sources — processing them all at once risks degraded quality as context fills up. Would you like to create a batch plan first?"_ If yes, load `.claude/skills/ingest/BATCH.md` and follow the Generating a Batch Plan workflow. If no, proceed — but cap the session at 3–5 sources.
5. Ask the user which file(s) to process.

**Skip this phase** when the user names a specific file or provides a URL.

---

## Phase 2: Read the Source

- **First:** Read `TOPIC.md` to understand the topic's wiki layout, page conventions, and domain-specific rules. This determines where new pages are filed in Phase 5. Also list the actual `wiki/` subdirectories — if they diverge from what `TOPIC.md` declares, use the actual structure and note the discrepancy to the user.
- Read the entire source file.
- If the user provides a **URL** instead of a file: fetch the URL, convert to clean markdown, save to `raw/articles/<slug>.md`, then proceed.
- Note the source's original URL if present (often in frontmatter from Obsidian Web Clipper).

---

## Phase 3: Discuss with the User

Brief exchange, not a monologue. Cover:

- 2-3 most interesting takeaways
- Does the user want any particular emphasis or angle?
- Any connections to existing wiki content you've noticed?

**Auto-mode (default):** Skip this phase — proceed with best judgment. This avoids doubling interactions and cost, especially during batch processing. If genuine input is needed (ambiguous domain, contradictory sources, unclear categorization), ask the user inline rather than pausing for a discussion turn.

**Discussion mode:** The user can say "discuss mode" or "let's discuss" to opt into this phase for a specific session or source.

---

## Phase 4: Create Source Summary

Create `wiki/sources/<slug>.md` using the Source Summary page type from `AGENTS.md`.

**Slug naming:** Lowercase, hyphenated, descriptive. Include author or source if helpful:

- `karpathy-llm-wiki-2026`
- `anthropic-claude-4-announcement`
- `personal-note-agent-patterns`

**Gotchas:**

- No clear title? Derive one from the content. Don't use the filename.
- Tweet or thread? The title should capture the key claim, not "Tweet by @handle".
- Keep summaries tight: 3-5 sentences for the overview. Full content lives in `raw/`.
- **Obsidian Web Clipper sources** may have `tags: - "clippings"` in their frontmatter (a Web Clipper artifact). Before ingesting, normalize this tag to something meaningful — or ask the user what tag to use. Don't carry the `clippings` artifact into source summary frontmatter.
- **Citation prefixes in Key Claims:** Do NOT prefix claims with `[YYYY Letter]` or `[Source Name]` in source summary pages — the file itself is about that single source, so the attribution is redundant. Reserve attribution labels (e.g., `[1981 Letter]`, `[Multi-year]`, `[Analysis]`) for wiki content pages (principles, entities, case studies, etc.) where claims from multiple sources appear together.

---

## Phase 5: Update Existing Pages

Read the wiki layout from `TOPIC.md` (or use the defaults: `concepts/`, `entities/`, `syntheses/`, `questions/`). For each key idea, person, place, or thing in the source, determine which wiki folder it belongs to based on the layout. Create or update pages in the appropriate folder.

- **Existing page:** Update with new information, add source to footnotes, increment `source_count` if applicable.
- **No page exists:** Create one in the appropriate folder with proper frontmatter.

**Contradiction handling:** When the new source contradicts existing wiki content:

1. Note the contradiction explicitly on the affected page (don't silently overwrite).
2. Create a question page in `wiki/questions/` documenting both claims and their sources.
3. Mention the contradiction in the log entry.

**Judgment calls:**

- Not every noun deserves an entity page. Create pages for things the user would plausibly look up later.
- Not every idea deserves a concept page. If a concept appears in only one source and isn't central, mention it in the source summary's "Key Claims" section instead.
- When updating an existing page, preserve the existing structure. Add to sections, don't reorganize unless it's clearly broken.

---

## Phase 6: Update Index and Log

**Index (`wiki/index.md`):** Add the new source summary and any new pages. Follow the existing format: markdown link + one-sentence summary + metadata.

**Log (`wiki/log.md`):** Append an entry:

```markdown
## [YYYY-MM-DD] ingest | <Source Title>

- Source: raw/articles/<filename>.md
- Created: sources/<slug>, <folder>/<new-page>, <folder>/<new-page>
- Updated: <folder>/<existing-page>, <folder>/<existing-page>
```

No links in log entries.

---

## Batch Mode

**When:** A `wiki/_batch-plan.md` exists, there are 10+ unprocessed sources, or the user says "create a batch plan" / "prepare for ingestion" / "process the next batch".

**Load:** Read `.claude/skills/ingest/BATCH.md` — it covers session state recovery, batch workflow, log format, progress tracking, and plan generation.

---

## Edge Cases

- **Duplicate source:** If `wiki/sources/` already has a summary for this source (same URL or same content), tell the user. Offer to update the existing summary if the source has changed.
- **Source in wrong folder:** Article in `raw/notes/` or note in `raw/articles/` - doesn't matter for processing. Mention it but don't refuse to ingest.
- **Very long source (>5000 words):** Focus on the most novel/important claims. Note in the summary which sections you focused on.
- **Source with images:** Read text first. If diagrams or charts seem important, tell the user you can view them separately for additional context.
- **Non-markdown source (PDF, image):** Note in the summary that the raw source is not markdown. Extract what you can.
