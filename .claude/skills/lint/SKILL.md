---
name: lint
description: Health-checks the wiki for quality issues - unprocessed sources, contradictions, broken links, orphan pages, staleness, and growth problems. Produces a severity-grouped report with actionable findings. Use when the user wants a wiki audit. Triggers on phrases like "lint", "health check", "audit", "what needs attention", "check the wiki".
---

# Lint Wiki

**Purpose:** Health-check the wiki for quality issues and produce an actionable, severity-grouped report. Identifies unprocessed sources, contradictions, broken links, orphans, staleness, and growth problems.

## When to Apply

- User says "lint", "health check", or "audit"
- User asks "what needs attention?" or "is anything broken?"
- Periodic maintenance (suggest running every 10-15 ingests)

---

## Workflow Overview

1. **Scan raw/** - Find unprocessed sources
2. **Read wiki/** - Load all wiki pages
3. **Run checks** - Systematic quality checks by severity
4. **Report** - Present findings as an actionable checklist
5. **Fix** - Apply approved fixes
6. **Log** - Record the lint

---

## Phase 1: Check for Unprocessed Sources

1. List all files in `raw/` across subdirectories (articles, papers, notes). Exclude `.gitkeep` and files in `assets/`.
2. Read `source_path` frontmatter from every file in `wiki/sources/`.
3. Compare. Report: "X files in raw/, Y ingested, Z remaining" with the list of unprocessed files.

This surfaces the most common issue first - sources the user added but forgot to ingest.

---

## Phase 2: Read All Wiki Pages

Read every `.md` file in `wiki/` (index, log, and all subdirectories). This gives full context for the checks that follow.

---

## Phase 3: Run Checks

Check for each of the following. Rate findings by severity.

**Important (address soon):**

- Contradictions between pages (two pages making conflicting claims about the same topic)
- Broken links (links pointing to pages that don't exist)
- Pages exceeding budget (see Growth Management in `AGENTS.md`)

**Medium (address when convenient):**

- Stale claims - pages not updated in 60+ days where newer relevant sources exist
- Important concepts mentioned in page text but lacking their own dedicated page
- Missing cross-references between pages that clearly relate to each other
- Questions that might now be answerable with existing sources (status still "open" but evidence exists)

**Minor (nice to have):**

- Orphan pages with no inbound links (except source summaries, which are leaf nodes by design)
- Inconsistent frontmatter (missing fields, wrong format)
- Index entries that don't match their page's current title or summary
- Log entries with formatting issues

---

## Phase 4: Report Findings

Present as a checklist grouped by severity:

```markdown
## Lint Report - [topic] - [date]

### Unprocessed Sources (Z remaining)

- [ ] raw/articles/new-article.md

### Important

- [ ] Contradiction: concepts/X claims "A" but sources/Y says "B"
- [ ] Broken link: concepts/Z links to entities/nonexistent.md

### Medium

- [ ] Stale: entities/some-tool not updated since 2026-01-15 (2 newer relevant sources exist)
- [ ] Missing page: "prompt caching" mentioned 4 times but has no concept page

### Minor

- [ ] Orphan: syntheses/old-comparison has no inbound links
```

---

## Phase 5: Apply Fixes

Ask the user which findings to address. Apply fixes for approved items only.

**Never auto-fix without approval** - especially for contradictions where the user needs to decide which claim is correct.

**Mechanical fixes** (safe to apply with minimal confirmation): broken links, index inconsistencies, frontmatter formatting.

---

## Phase 6: Log the Lint

Append to `wiki/log.md`:

```markdown
## [YYYY-MM-DD] lint | Health check

- Unprocessed: Z files in raw/
- Found: X important, Y medium, Z minor issues
- Fixed: [list what was fixed]
- Open: [list what remains]
```

---

## Edge Cases

- **Don't create pages during lint.** Lint identifies gaps; the user decides whether to fill them. Exception: fixing broken links or index inconsistencies (mechanical fixes).
- **Source summaries are intentionally leaf nodes.** They link out to concepts/entities but shouldn't have inbound links (other pages cite them via footnotes). Don't flag these as orphans.
- **Stale != wrong.** A page from 3 months ago might still be perfectly accurate. Flag staleness only when newer relevant sources exist.
- **Growth budgets are guidelines, not hard limits.** A concept page at 90 lines that's well-organized is fine. A page at 60 lines that's a disorganized mess is worse. Use judgment.
