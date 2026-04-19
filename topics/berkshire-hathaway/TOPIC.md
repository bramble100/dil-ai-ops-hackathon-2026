# Buffett's Letters — Investor Education Wiki

## Purpose

This wiki distills **Warren Buffett's annual shareholder letters** (1977-2025) into a structured knowledge base designed to **make the reader a better investor and company analyst**. The primary goal is not to document Berkshire Hathaway's corporate history, but to extract, organize, and cross-reference the **investment principles, mental models, analytical frameworks, and hard-won lessons** that Buffett (and Munger) teach through 49 years of candid writing.

Berkshire's story and financials serve as the living case study through which these principles are demonstrated. The wiki should help a reader answer: "What would Buffett think about this situation, and why?"

## Wiki Layout

- `sources/` - One summary per shareholder letter (always present)
- `principles/` - Core investment principles and mental models (intrinsic value, margin of safety, moats, Mr. Market, circle of competence, owner earnings, look-through earnings, float economics, capital allocation, management quality assessment). These are the wiki's most important pages — they should be rich, multi-source documents by the end.
- `entities/` - Key people and companies — biographical and factual context (Buffett, Munger, Abel, Jain, GEICO, See's Candies, Coca-Cola, Apple). Entity pages exist to support principle pages, not as standalone corporate profiles.
- `case-studies/` - Acquisitions and investment decisions as teaching examples. Each page covers rationale, price discipline, long-term outcome, Buffett's own retrospective assessment, and — most importantly — **what principle or lesson the case illustrates**.
- `financials/` - Berkshire's financial track record as a compounding case study (book value per share, operating earnings, float growth, portfolio composition, returns vs S&P 500). These pages demonstrate principles in action over decades.
- `questions/` - Open threads: contradictions between letters, philosophy shifts, unresolved debates, numbers that don't reconcile.

## Page Conventions

### Frontmatter

```yaml
---
title: Page Title
type: source | entity | principle | case-study | financial | question | overview
sources: [buffett-letter-1977, buffett-letter-1990]
created: 2026-04-19
updated: 2026-04-19
status: draft | complete
tags: [insurance, acquisition, philosophy]
---
```

### Claim Attribution

Label claims by provenance:

- **[YYYY Letter]** - Direct from a specific year's shareholder letter (cite the year)
- **[Multi-year]** - Pattern observed across multiple letters (cite the range)
- **[Analysis]** - Inference drawn by connecting claims from multiple letters
- **[Gap]** - Information not available in the corpus (e.g., Buffett doesn't discuss it)

### Numbers and Currency

- All financial figures in **USD** — use millions (M) or billions (B) as appropriate
- Always include the year when citing any specific number
- For time-series data, use markdown tables with years as columns or rows
- Book value per share is the key recurring metric — track it precisely
- When Buffett restates a prior year's number, note both the original and restated figure

### Temporal Context

- Every claim should be dated: "In the 1990 letter, Buffett wrote..."
- Track how views evolve: if Buffett changes his position on something (e.g., tech stocks, share buybacks), document both the original and revised view with dates
- Early letters (1977-1985) are shorter and more focused on textiles/insurance — foundational principles emerge here
- Middle letters (1986-2005) expand as Berkshire grows — richer philosophy, more case studies
- Late letters (2006-2025) are increasingly reflective — succession, legacy, market commentary, revisited wisdom

### Source Naming Convention

Source files follow: `buffett-letter-YYYY.md`
Source summary pages follow: `sources/buffett-letter-YYYY.md`

### Buffett Quotes

Buffett's aphorisms are a signature feature of the letters. When a quote captures a principle memorably:

1. Include it on the relevant **principle page** with year attribution: `> "Quote text" — YYYY Letter`
2. Also add it to the standalone **`quotes.md`** page (see Standalone Pages below) for cross-reference

### Tag Strategy

Use tags to enable rich querying across the wiki. Three tag dimensions:

**Topic tags** (what area of investing) — starting vocabulary, extend freely when justified:
`valuation`, `capital-allocation`, `insurance`, `management`, `risk`, `compounding`, `market-psychology`, `corporate-governance`, `accounting`, `moats`, `dividends`, `buybacks`, `debt`, `taxes`, `ethics`

**Type tags** (what kind of content) — starting vocabulary, extend freely when justified:
`principle`, `mental-model`, `mistake`, `lesson`, `prediction`, `retrospective`, `warning`, `anecdote`, `rule-of-thumb`

**Era tags** (when — use only on source summaries):
`early-berkshire` (1977-1985), `growth-era` (1986-1998), `mega-cap-era` (1999-2012), `late-buffett` (2013-2025)

These lists are **starting vocabulary, not closed lists**. Add new tags whenever existing ones don't fit the content well. Prefer extending existing tags over creating near-synonyms (e.g., use `management` not `managers` or `ceo`). Consistency across pages matters more than completeness.

### Standalone Pages

These live at `wiki/` root alongside `index.md` and `overview.md`:

- **`quotes.md`** — Master collection of Buffett's best aphorisms and memorable passages. Organized by theme (not chronologically). Each quote includes the year, the context, and a link to the relevant principle page. This is the wiki's "greatest hits" — the page a reader bookmarks.
- **`timeline.md`** — Chronological master timeline of key events: major acquisitions, portfolio moves, management changes, market crises, philosophy milestones. One line per event with year and link to relevant case-study or entity page. Gives readers the big picture at a glance.
- **`acquisitions-timeline.md`** — Dedicated chronological list of all acquisitions and major investments: date, company, price paid, one-line outcome, and link to the detailed case-study page. Serves readers interested specifically in Berkshire's corporate/acquisition history.

### Principle Page Structure

Principle pages are the wiki's core product. They should follow this structure:

1. **Definition** — What this principle means, in clear terms
2. **Buffett's Teaching** — How Buffett explains it across the letters (with quotes and year citations)
3. **Evolution** — How the principle was refined, expanded, or reconsidered over time
4. **Case Studies** — Links to case-study pages that demonstrate this principle in action
5. **Practical Application** — How an investor can apply this principle to their own analysis
6. **Sources** — Footnotes linking to source summaries

### Case Study Page Structure

Case study pages teach through specific examples:

1. **Overview** — What was acquired/invested in, when, and for how much
2. **Buffett's Rationale** — Why he bought it, in his own words (with year citations)
3. **Outcome** — How it performed over time (with financial data where available)
4. **Buffett's Retrospective** — His later reflections on the decision (if any)
5. **Lessons** — What principles this case illustrates, with links to principle pages
6. **Sources** — Footnotes

## Key Questions

1. **What makes a great business?** What are Buffett's criteria for identifying wonderful companies — and how can an investor apply them?
2. **How to think about valuation?** What frameworks does Buffett use to determine intrinsic value, and how have they evolved from Graham-era net-nets to owner earnings?
3. **What are the biggest mistakes to avoid?** What errors does Buffett confess to, warn about, or illustrate through Berkshire's history?
4. **How does compounding actually work?** What does Berkshire's 49-year track record teach about the mathematics and patience of long-term wealth creation?
5. **What makes a great capital allocator?** Beyond stock-picking, what does Buffett teach about deploying capital across insurance float, wholly-owned businesses, and marketable securities?
6. **How to assess management?** What does Buffett look for in managers, and what red flags does he warn about?
7. **What changed and what didn't?** Which principles remained constant across 49 years, and where did Buffett meaningfully shift his thinking?

## Ingest Notes

- **Chronological processing is critical** — process letters in order (1977 → 2025) so that principle pages compound naturally. Each new letter should enrich existing pages, not just add new ones.
- **Adaptive batch size by era:**
  - **1977-1986** (short letters, ~400-1600 lines): 5 per batch
  - **1987-1997** (medium letters, ~1100-1700 lines): 4 per batch
  - **1998-2025** (long PDFs, 15-30 pages each): 3 per batch
  - As wiki pages accumulate, reduce batch size further if context gets tight
- **Principles over facts** — when ingesting, prioritize extracting investment wisdom over documenting corporate minutiae. A paragraph where Buffett explains why he values pricing power matters more than the specific revenue figure of a subsidiary.
- **Track Buffett's self-assessments** — he frequently revisits past decisions. "I was wrong about X" or "this turned out better than expected" is high-value content for case-study pages.
- **The per-share performance table** (book value vs S&P 500) appears in most letters — extract it into `financials/` and build a master time-series.
- **Quotes are gold** — capture the best aphorisms on relevant principle pages AND on `quotes.md`. A quote that perfectly captures a principle is worth more than a paragraph of explanation.
- **Maintain standalone pages** — after each batch, update `quotes.md`, `timeline.md`, and `acquisitions-timeline.md` with new entries from the ingested letters.
- **Entity page judgment** — not every company or person mentioned needs a page. At batch time, ask: "Would a reader plausibly look this up to understand a principle or the Berkshire story?" Create entity pages for: (a) Berkshire operating subsidiaries (even if only introduced once — they recur), (b) major portfolio holdings with meaningful discussion, (c) named people who hold decision-making roles at Berkshire (managers, board, key partners). If an entity page already exists from a previous batch, always update it — even for minor new mentions. Passing references to third-party companies, competitors, or one-off examples belong in source summaries, not entity pages.
- **Compound, don't duplicate** — by the time you reach 2025, principle pages like "intrinsic value" or "margin of safety" should be rich multi-source documents tracing how Buffett taught the concept across decades.
- **Code-fenced letters (1977-1996)** — most early markdown letters have their body wrapped in a single code fence (Web Clipper artifact). The text is fully readable; the formatting is cosmetic. Don't let it slow ingestion.
- **PDF-converted letters (1998-2025)** — converted to markdown via PyMuPDF text extraction. The performance table at the start of each letter has broken formatting (each cell on a separate line instead of tabular). The actual letter body text reads cleanly. Focus on the letter content; the master performance table will be built separately in `financials/`.
- **Source discovery** — only process files from `raw/articles/` (markdown). Original PDFs are archived in `raw/assets/` and should NOT be ingested (they are duplicates of the markdown versions).
