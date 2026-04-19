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
