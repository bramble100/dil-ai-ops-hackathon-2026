# Rheinmetall AG Corporate Intelligence Wiki - Schema

## Purpose

This wiki compiles and cross-references publicly available financial, strategic, and operational information about **Rheinmetall AG** (XETRA: RHM, ISIN: DE0007030009) from annual reports, earnings call transcripts, and other primary sources spanning 2017-2025.

The goal is to transform ~3,000 pages of dense German/English corporate filings into an interconnected, navigable knowledge base that surfaces trends, contradictions, and strategic shifts over time.

## Raw Layout

Flat — all sources in `raw/` root, no subfolders. Sources are PDF annual reports and earnings call transcripts.

## Wiki Layout

- `sources/` - One summary per raw source document (always present)
- `entities/` - People, organizations, subsidiaries (Papperger, key JVs, etc.)
- `concepts/` - Thematic and strategic concept pages (defense pivot, Ukraine impact, etc.)
- `financials/` - Financial analysis and time-series pages (revenue, margins, backlog, etc.)
- `comparisons/` - Cross-cutting analysis (pre/post Ukraine, guidance vs actuals)
- `questions/` - Open contradictions and data quality issues

## Page Conventions

### Frontmatter

```yaml
---
title: Page Title
type: source | entity | concept | financial | comparison | question | overview
sources: [ar-2025, transcript-fy20]
created: 2026-04-16
updated: 2026-04-16
status: draft | complete
tags: [defense, revenue, strategy]
---
```

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

## Key Questions

1. How has Rheinmetall transformed from a dual-segment conglomerate to a pure defense company?
2. What are the financial trends (revenue, margins, ROCE, backlog) and what drives them?
3. How does management's guidance track against actual results?
4. What contradictions exist between reports, and what explains them?

## Ingest Notes

- **Accuracy over speed** — verify numbers against source documents before writing them into wiki pages
- **Trends over snapshots** — always contextualize a data point within its time series, not in isolation
- Always update financial time-series pages (`financials/`) when a new annual report brings updated numbers
- Update `comparisons/` pages if the new data changes cross-cutting analyses
- Flag contradictions: if two reports disagree on a number, note both and file a question page
- Track what's missing: gaps are valuable information; tag them with **[Gap]**
- Compound knowledge: each new source should enrich existing pages, not just add new ones
