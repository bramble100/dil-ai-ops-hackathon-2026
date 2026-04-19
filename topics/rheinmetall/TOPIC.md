# Rheinmetall AG Corporate Intelligence Wiki - Schema

## Purpose

This wiki compiles and cross-references publicly available financial, strategic, and operational information about **Rheinmetall AG** (XETRA: RHM, ISIN: DE0007030009) from annual reports, earnings call transcripts, and other primary sources spanning 2017-2025.

The goal is to transform ~3,000 pages of dense German/English corporate filings into an interconnected, navigable knowledge base that surfaces trends, contradictions, and strategic shifts over time.

## Directory Structure

```
wiki/
  index.md              # Master catalog - every page listed with link and one-line summary
  log.md                # Chronological activity log (append-only)
  overview.md           # Executive summary of Rheinmetall AG (the "front page")

  sources/              # One summary per raw source document
    ar-2017.md          # Annual Report 2017 (Geschaeftsbericht)
    ar-2018.md
    ar-2019.md
    ar-2020.md
    ar-2021.md
    ar-2022.md
    ar-2023.md
    ar-2025.md          # Note: no 2024 report in corpus
    transcript-fy17.md  # Earnings call transcript FY2017
    transcript-fy18.md
    transcript-fy19.md
    transcript-fy20.md

  entities/             # Pages for people, organizations, subsidiaries
    armin-papperger.md
    helmut-merch.md
    rheinmetall-ag.md   # Corporate profile: HQ, listing, governance
    key-subsidiaries.md # RBSL, Rheinmetall MAN, Expal, Loc Performance, etc.

  concepts/             # Thematic and strategic concept pages
    automotive-to-defense-pivot.md
    european-defense-spending.md
    ukraine-war-impact.md
    ammunition-capacity-expansion.md
    digitalization-and-ai.md
    export-controls.md

  financials/           # Financial analysis and comparison pages
    revenue-trends.md           # Revenue 2017-2025 with segment breakdown
    profitability-trends.md     # Margins, EBIT, ROCE over time
    order-backlog-analysis.md   # Backlog evolution and book-to-bill
    segment-performance.md      # Division-level deep dive
    capex-and-investments.md    # Capital expenditure trends
    employee-growth.md          # Headcount evolution

  comparisons/          # Cross-cutting analysis
    pre-vs-post-ukraine.md      # Before/after Feb 2022 inflection
    guidance-vs-actuals.md      # Track record of management guidance
```

## Page Conventions

### Frontmatter

Every wiki page starts with YAML frontmatter:

```yaml
---
title: Page Title
type: source | entity | concept | financial | comparison | overview
sources: [ar-2025, transcript-fy20] # which raw sources informed this page
created: 2026-04-16
updated: 2026-04-16
status: draft | complete
tags: [defense, revenue, strategy]
---
```

### Cross-linking

- Use standard markdown links: `[link text](../relative/path.md)`
- When referencing another wiki page, always link on first mention in a section
- Entity names should link to their entity page on first mention

### Claim Attribution

Label claims by their provenance:

- **[Source]** - Direct from a raw document (cite specific report + page/section)
- **[Analysis]** - Inference drawn by connecting multiple sources
- **[Unverified]** - Plausible but no authoritative source in corpus
- **[Gap]** - Explicitly flagged as missing from available sources

### Numbers and Currency

- All financial figures in **MioEUR** (millions of euros) unless stated otherwise
- Use European decimal convention in running text (e.g., "1.841 MioEUR") to match source documents
- Include the year and source when citing any specific number
- For time-series data, use markdown tables with years as columns

### Language

- Wiki pages are written in **English**, even when sources are in German
- German terms are included in parentheses on first use when they are standard industry/financial terms
- Direct quotes from German sources are translated with original in footnotes

## Ingest Workflow

When processing a new raw source:

1. **Read** the source document thoroughly
2. **Create** a source summary page in `wiki/sources/`
3. **Update** the index (`wiki/index.md`) with the new source entry
4. **Create or update** relevant entity pages in `wiki/entities/`
5. **Create or update** relevant concept pages in `wiki/concepts/`
6. **Create or update** financial data pages in `wiki/financials/`
7. **Update** comparison pages if the new data changes cross-cutting analyses
8. **Log** the ingest activity in `wiki/log.md`

## Index Maintenance

`index.md` organizes all pages by category with a one-line description. It is the LLM's first read when answering queries. Keep it current after every ingest.

## Log Format

Each log entry in `log.md`:

```
## YYYY-MM-DD HH:MM - [Operation Type]

**Sources processed:** list of files
**Pages created:** list of new pages
**Pages updated:** list of modified pages
**Notes:** any observations, contradictions found, gaps identified
```

## Quality Principles

1. **Accuracy over speed** - verify numbers against source documents
2. **Trends over snapshots** - always contextualize a data point within its time series
3. **Flag contradictions** - if two sources disagree, note both and flag the discrepancy
4. **Track what's missing** - gaps are valuable information; tag them explicitly
5. **Compound knowledge** - each new source should enrich existing pages, not just add new ones
