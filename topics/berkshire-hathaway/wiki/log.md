# Activity Log

## [2026-04-19] init | Wiki bootstrapped

- Topic created: berkshire-hathaway
- Domain: Warren Buffett's annual shareholder letters (1977-2025) — investor education wiki
- Layout: sources/, principles/, entities/, case-studies/, financials/, questions/
- Created: index.md, log.md, TOPIC.md, directory structure
- Sources queued: 21 markdown letters (1977-1997), 28 PDF letters (1998-2025)

## [2026-04-19] maintenance | Structure revision for investor-education focus

- Renamed concepts/ to principles/ (actionable investment wisdom, not abstract concepts)
- Renamed acquisitions/ to case-studies/ (teaching examples, not corporate history)
- Rewrote TOPIC.md: purpose, key questions, and ingest notes reoriented toward investor education
- Fixed frontmatter on all 21 markdown letters (title, author, published, description)
- Removed .gitkeep from raw/articles/ and raw/papers/ (now have real files)
- Revised batch sizing: 5 early, 4 mid, 3 late (adaptive to letter length and wiki growth)

## [2026-04-20] maintenance | Tag normalization and TOPIC.md refinements

- Fixed "clippings" tag on all 21 web-clipped letters (1977-1997) → shareholder-letter
- Removed raw/assets/.gitkeep (folder now has 28 archival PDFs)
- Moved buffett-letter-2009.pdf and 2010.pdf from raw/papers/ to raw/assets/ (previously locked)
- Removed raw/papers/ directory (now empty)
- Reframed tag lists in TOPIC.md as suggested vocabulary (extensible, not closed)
- Replaced impractical "3+ letters" entity criterion with batch-time heuristic (named decision-making role)

## [2026-04-20] maintenance | Batch mode infrastructure

- Created wiki/\_batch-plan.md: 14 batches covering all 49 letters (5-5-4-4-3 then 3s, plus post-ingestion tasks)
- Added Batch Mode section to ingest skill (reusable for any topic with 10+ sources)
- Added Obsidian Web Clipper tag gotcha to ingest skill
- Added "Writing Good TOPIC.md" guidance to AGENTS.md (tag extensibility, batch-time heuristics)

## [2026-04-20] maintenance | Structure refinements and PDF conversion cleanup

- Converted 28 PDF letters (1998-2025) to markdown via PyMuPDF; all 49 letters now in raw/articles/ as .md
- Moved original PDFs from raw/papers/ to raw/assets/ (archival; excluded from ingest discovery)
- Added tag strategy to TOPIC.md: topic tags, type tags, era tags for rich querying
- Added standalone page guidance: quotes.md, timeline.md, acquisitions-timeline.md
- Updated index.md: changed "Expected:" definitive lists to "e.g." illustrative examples
- Updated ingest notes: source discovery limited to raw/articles/, PDF formatting caveats documented

## [2026-04-20] ingest-batch | Batch 1 — 1977-1981 (early Berkshire, foundational letters)

- Sources: raw/articles/buffett-letter-1977.md, buffett-letter-1978.md, buffett-letter-1979.md, buffett-letter-1980.md, buffett-letter-1981.md
- Created: sources/buffett-letter-1977 through 1981, principles/return-on-equity, principles/owner-earnings, principles/four-filters, principles/business-quality, principles/underwriting-discipline, principles/inflation-and-investing, principles/capital-allocation, entities/geico, entities/national-indemnity, entities/sees-candies, entities/charlie-munger, case-studies/textile-operations, case-studies/geico-investment, financials/book-value-per-share, quotes, timeline, acquisitions-timeline
- Updated: index.md
- Notes: All principle pages marked draft — will be enriched as later batches add sources. Batch establishes foundational framework: ROE yardstick, four filters, owner earnings, business quality, underwriting discipline, inflation analysis, capital allocation. GEICO and textiles are the anchor case studies (positive and negative).

## [2026-04-20] ingest-batch | Batch 2 — 1982-1983 (commodity business framework, Owner's Manual, Economic Goodwill)

- Sources: raw/articles/buffett-letter-1982.md, buffett-letter-1983.md
- Created: sources/buffett-letter-1982, sources/buffett-letter-1983, principles/economic-goodwill, entities/nebraska-furniture-mart, entities/buffalo-evening-news
- Updated: principles/return-on-equity, principles/owner-earnings, principles/business-quality, principles/capital-allocation, principles/underwriting-discipline, principles/inflation-and-investing, principles/four-filters, entities/geico, entities/sees-candies, entities/national-indemnity, entities/charlie-munger, case-studies/textile-operations, case-studies/geico-investment, financials/book-value-per-share, quotes, timeline, acquisitions-timeline, index.md
- Notes: 1982 introduces commodity business framework and equity issuance discipline. 1983 is a landmark: 13 Owner's Manual principles, Economic Goodwill appendix, NFM acquisition, Blue Chip merger. Book value track record extended to $975.83 (22.6% CAGR). All existing pages enriched with 1982-83 content.
