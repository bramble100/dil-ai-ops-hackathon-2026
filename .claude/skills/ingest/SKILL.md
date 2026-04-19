---
name: ingest
description: Processes raw source documents into the wiki - creates source summaries, updates concept/entity pages, maintains index and log. Discovers unprocessed sources when no file is specified. Use when the user adds a source, drops a file into raw/, or says "ingest". Triggers on phrases like "ingest", "process this source", "process this file", "add this to the wiki", "what's unprocessed".
---

# Ingest Source

**Purpose:** Process a raw source document into a structured, interlinked wiki entry - creating a summary, updating concept and entity pages, and maintaining the index and log.

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
5. **Integrate** - Update concept and entity pages
6. **Bookkeep** - Update index and log

---

## Phase 1: Discover Unprocessed Sources

**When:** The user says "ingest" without naming a specific file.

1. List all files in `raw/` across subdirectories (articles, papers, notes). Exclude `.gitkeep` and files in `assets/`.
2. Read `source_path` frontmatter from every file in `wiki/sources/`.
3. Compare the two lists. Present unprocessed files as a numbered list.
4. Ask the user which file(s) to process.

**Skip this phase** when the user names a specific file or provides a URL.

---

## Phase 2: Read the Source

- Read the entire source file.
- If the user provides a **URL** instead of a file: fetch the URL, convert to clean markdown, save to `raw/articles/<slug>.md`, then proceed.
- Note the source's original URL if present (often in frontmatter from Obsidian Web Clipper).

---

## Phase 3: Discuss with the User

Brief exchange, not a monologue. Cover:

- 2-3 most interesting takeaways
- Does the user want any particular emphasis or angle?
- Any connections to existing wiki content you've noticed?

**Skip this phase** if the user says "just process it" or similar. Use your best judgment on emphasis.

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

---

## Phase 5: Update Existing Pages

**Concepts:** For each key concept in the source:

- Existing page: update with new information, add source to footnotes, increment `source_count`.
- No page exists: create one with `source_count: 1`.

**Entities:** For each named entity (tool, person, org, product):

- Existing page: update, add source reference.
- No page exists: create one.

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
- Created: sources/<slug>, concepts/<new-concept>, entities/<new-entity>
- Updated: concepts/<existing-concept>, entities/<existing-entity>
```

No links in log entries.

---

## Batch Ingest

For multiple sources, process 3-5 at a time. After each batch:

- Pause and let the user review the new/updated pages.
- Ask if emphasis or direction should be adjusted before continuing.

This prevents context degradation on large batches.

---

## Edge Cases

- **Duplicate source:** If `wiki/sources/` already has a summary for this source (same URL or same content), tell the user. Offer to update the existing summary if the source has changed.
- **Source in wrong folder:** Article in `raw/notes/` or note in `raw/articles/` - doesn't matter for processing. Mention it but don't refuse to ingest.
- **Very long source (>5000 words):** Focus on the most novel/important claims. Note in the summary which sections you focused on.
- **Source with images:** Read text first. If diagrams or charts seem important, tell the user you can view them separately for additional context.
- **Non-markdown source (PDF, image):** Note in the summary that the raw source is not markdown. Extract what you can.
