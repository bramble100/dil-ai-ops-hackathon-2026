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

## [2026-04-20] ingest-batch | Batch 3 — 1984-1985 (scale drag, textile closure, Capital Cities/ABC)

- Sources: raw/articles/buffett-letter-1984.md, buffett-letter-1985.md
- Created: sources/buffett-letter-1984, sources/buffett-letter-1985, entities/capital-cities-abc, entities/scott-fetzer
- Updated: principles/return-on-equity, principles/business-quality, principles/capital-allocation, principles/underwriting-discipline, principles/four-filters, principles/owner-earnings, principles/inflation-and-investing, principles/economic-goodwill, entities/geico, entities/sees-candies, entities/nebraska-furniture-mart, entities/buffalo-evening-news, entities/national-indemnity, entities/charlie-munger, case-studies/textile-operations, case-studies/geico-investment, financials/book-value-per-share, quotes, timeline, acquisitions-timeline, index.md
- Notes: 1984 introduces scale drag warning, dividend policy framework, WPPSS bonds-as-business, greenmail condemnation, loss-reserving errors. 1985 is a milestone: textile closure (Burlington comparison, capex trap, boat metaphor), Capital Cities/ABC ($517.5M), Scott & Fetzer (~$320M), General Foods sale, incentive comp critique, Halley's Comet year (48.2%). Book value extended to $1,643.71 (23.2% CAGR).

## [2026-04-20] ingest-batch | Batch 4 — 1986-1987 (owner earnings formula, Mr. Market, permanent holdings)

- Sources: raw/articles/buffett-letter-1986.md, buffett-letter-1987.md
- Created: sources/buffett-letter-1986, sources/buffett-letter-1987, principles/mr-market, entities/fechheimer, entities/salomon-inc
- Updated: principles/return-on-equity, principles/owner-earnings, principles/four-filters, principles/business-quality, principles/underwriting-discipline, principles/inflation-and-investing, principles/capital-allocation, principles/economic-goodwill, entities/geico, entities/sees-candies, entities/national-indemnity, entities/charlie-munger, entities/nebraska-furniture-mart, entities/buffalo-evening-news, entities/capital-cities-abc, entities/scott-fetzer, case-studies/geico-investment, financials/book-value-per-share, quotes, timeline, acquisitions-timeline, index.md
- Notes: 1986 is foundational: owner earnings equation (a+b-c) in appendix demolishes "cash flow" fallacy; Scott Fetzer Company O vs N; Fechheimer acquisition via annual-report ad; "be fearful when others are greedy"; GEICO moat metaphor; Tax Reform Act analysis (franchise vs commodity tax incidence); "not more statesmen, but less corn." 1987 is one of the most quotable letters: full Mr. Market parable from Graham; Sainted Seven at 57% ROE; three permanent holdings designated; CEO capital allocation critique (Carnegie Hall/Fed); debt philosophy ("loaded gun"); Salomon $700M convertible preferred; portfolio insurance demolition after Black Monday. Book value extended to $2,477.47 (23.1% CAGR).

## [2026-04-20] ingest-batch | Batch 6 — 1990-1992 (look-through earnings, Wells Fargo, Salomon scandal, DCF synthesis)

- Sources: raw/articles/buffett-letter-1990.md, buffett-letter-1991.md, buffett-letter-1992.md
- Created: sources/buffett-letter-1990, sources/buffett-letter-1991, sources/buffett-letter-1992, entities/wells-fargo, entities/hh-brown, entities/general-dynamics
- Updated: principles/return-on-equity, principles/owner-earnings, principles/four-filters, principles/business-quality, principles/underwriting-discipline, principles/capital-allocation, principles/mr-market, entities/geico, entities/sees-candies, entities/national-indemnity, entities/charlie-munger, entities/nebraska-furniture-mart, entities/buffalo-evening-news, entities/capital-cities-abc, entities/scott-fetzer, entities/fechheimer, entities/salomon-inc, entities/coca-cola, entities/borsheims, entities/gillette, financials/book-value-per-share, quotes, timeline, acquisitions-timeline, index.md
- Notes: 1990 introduces the float-cost framework (1.6% vs government bond yield), Wells Fargo contrarian purchase ($290M, <5x earnings), junk bond demolition, and media franchise erosion identified as structural. 1991 is the richest letter: H.H. Brown acquisition (July), See's 20-year $410M retrospective, franchise 3-test definition (needed/no substitute/no price regulation), Salomon scandal with Buffett as Interim Chairman (Aug 18), Fannie Mae omission (~$1.4B forgone), Gillette preferred converted to common. 1992 delivers the growth-value synthesis (DCF as universal valuation; "value investing is redundant"), General Dynamics arbitrage-to-conviction (14% in one month), stock-option expensing critique, Central States Indemnity acquisition, Mrs. B returns at 99 with non-compete, Hurricane Andrew $125M loss. Book value extended: $4,612.06 (7.3%), $6,437.00 (39.6%), $7,745.00 (20.3%).

## [2026-04-20] ingest-batch | Batch 7 — 1993-1994 (Dexter Shoe, beta demolished, Scott Fetzer 9-year masterclass)

- Sources: raw/articles/buffett-letter-1993.md, buffett-letter-1994.md
- Created: sources/buffett-letter-1993, sources/buffett-letter-1994, entities/dexter-shoe, entities/american-express
- Updated: principles/return-on-equity, principles/owner-earnings, principles/four-filters, principles/business-quality, principles/underwriting-discipline, principles/capital-allocation, principles/mr-market, entities/geico, entities/sees-candies, entities/nebraska-furniture-mart, entities/capital-cities-abc, entities/scott-fetzer, entities/coca-cola, entities/gillette, entities/wells-fargo, entities/hh-brown, financials/book-value-per-share, quotes, timeline, acquisitions-timeline, index.md, \_batch-plan.md
- Notes: 1993 introduces the beta demolition (5 real risk factors), Li'l Abner deferred-tax demonstration of look-through earnings ($856M total), Dexter Shoe acquisition for 25,203 Berkshire shares (later Buffett's worst stock deal), corporate governance three-situation framework, float $2.6B at negative cost, Mrs. B turns 100. 1994 delivers the fat wallet problem ($11.9B net worth → $100M minimum position), Scott Fetzer definitive 9-year retrospective (near-100% pre-tax ROE, would rank #1 Fortune 500), Mistake Du Jour Silver (Cap Cities at $63 = $222.5M forgone) and Gold (USAir writedown), American Express rebuilt to 27.76M shares, compensation rationality without consultants. Book value extended: $8,854 (14.3%), $10,083 (13.9%) — 30-year CAGR 23.0%.

## [2026-04-20] ingest-batch | Batch 5 — 1988-1989 (Coca-Cola, institutional imperative, "Mistakes of First 25 Years")

- Sources: raw/articles/buffett-letter-1988.md, buffett-letter-1989.md
- Created: sources/buffett-letter-1988, sources/buffett-letter-1989, principles/institutional-imperative, entities/coca-cola, entities/borsheims, entities/gillette
- Updated: principles/return-on-equity, principles/owner-earnings, principles/four-filters, principles/business-quality, principles/underwriting-discipline, principles/inflation-and-investing, principles/capital-allocation, principles/economic-goodwill, principles/mr-market, entities/geico, entities/sees-candies, entities/national-indemnity, entities/charlie-munger, entities/nebraska-furniture-mart, entities/buffalo-evening-news, entities/capital-cities-abc, entities/scott-fetzer, entities/fechheimer, entities/salomon-inc, case-studies/geico-investment, case-studies/textile-operations, financials/book-value-per-share, quotes, timeline, acquisitions-timeline, index.md
- Notes: 1988 introduces two landmark topics: complete arbitrage framework (four questions, Rockwood & Co.) and EMT demolition (63-year record, $97M vs $405K). Coca-Cola purchase ($592.5M) marks Berkshire's most famous investment; "favorite holding period is forever" coined. Sainted Seven reach 67% ROE. GAAP manifesto (three questions). 1989 is one of the most philosophically rich letters: "Mistakes of First 25 Years" crystallizes cigar butt critique, institutional imperative (new principle), cockroach theory, one-foot hurdles. Zero-coupon/PIK takedown extends owner earnings into debt markets. Three convertible preferreds (Gillette $600M, USAir $358M, Champion $300M). Rip Van Winkle tax math. CAT reinsurance $250M capacity. Mrs. B departs NFM at 96. Borsheim's acquired. Book value extended to $4,296.01 (23.8% CAGR over 25 years).
