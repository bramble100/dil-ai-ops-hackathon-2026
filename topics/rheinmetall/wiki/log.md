---
title: Activity Log
type: overview
created: 2026-04-16
updated: 2026-04-19
---

# Activity Log

## [2026-04-16] init | Wiki bootstrapped

- Initial wiki creation
  **Sources inventoried:** 12 PDFs (8 annual reports 2017-2023+2025, 4 earnings call transcripts FY17-FY20)
  **Pages created:** SCHEMA.md, index.md, log.md, overview.md
  **Notes:** Raw corpus is ~3,000 pages. Annual reports 2017-2020 are in German (Geschaeftsbericht format with ISIN-based filenames). Reports from 2021 onward use English titles but content remains German. Earnings call transcripts are in English. No 2024 annual report in corpus -- gap noted.

## [2026-04-16] ingest | Full batch (12 sources)

- Full batch ingest of all 12 source documents
  **Sources processed:**

- DE0007030009-JA-2017-EQ-D-01.pdf (Annual Report 2017, 226pp)
- DE0007030009-JA-2018-EQ-D-00.pdf (Annual Report 2018, 230pp)
- DE0007030009-JA-2019-EQ-D-01.pdf (Annual Report 2019, 248pp)
- DE0007030009-JA-2020-PN-EQ-D-00.pdf (Annual Report 2020, 264pp)
- RheinmetallAGAnnualReport2021.pdf (Annual Report 2021, 261pp)
- RheinmetallAGAnnualReport2022.pdf (Annual Report 2022, 286pp)
- RheinmetallAGAnnualReport2023.pdf (Annual Report 2023, 283pp)
- RheinmetallAGAnnualReport2025.pdf (Annual Report 2025, 286pp)
- 2018-03-15_Rheinmetall_Transcript_Conference_Call_FY17.pdf (22pp)
- 2019-03-13_Rheinmetall_Transcript_Conference_Call_FY18.pdf (18pp)
- 2020-03-19_Rheinmetall_Transcript_Conference_Call_Q4.pdf (25pp)
- 2021-03-18_Rheinmetall_Transcript_Conference_Call_FY2020.pdf (32pp)

**Pages created (28 total):**

- Source summaries: ar-2017 through ar-2025 (8), transcript-fy17 through transcript-fy20 (4)
- Entity pages: armin-papperger, helmut-merch, rheinmetall-ag, key-subsidiaries (4)
- Concept pages: automotive-to-defense-pivot, european-defense-spending, ukraine-war-impact, ammunition-capacity-expansion, digitalization-and-ai, export-controls (6)
- Financial analysis: revenue-trends, profitability-trends, order-backlog-analysis, segment-performance, capex-and-investments, employee-growth (6)
- Comparisons: pre-vs-post-ukraine, guidance-vs-actuals (2)
- Working documents: \_extraction_notes.md, \_transcript_insights.md

**Key data extracted:**

- Complete financial time series 2011-2025 (revenue, operating result, margins, EBIT, ROCE, FCF, employees)
- Segment breakdown: Defence/Automotive (2013-2019) and Vehicle Systems/W&A/Electronic Solutions (2024-2025)
- Full order data evolution: backlog from EUR 6.4bn (2017) to EUR 63.8bn (2025)
- Management guidance and actual results across four years of earnings calls
- Strategic evolution: from dual-segment to pure defense, 2030 target of EUR 50bn revenue
- Global defence budget data by country (2024-2025)
- R&D employees by segment (2024-2025): 5,205 FTE in continuing operations

**Gaps identified:**

- No 2024 annual report in corpus
- 2023-2025 detailed employee counts (total FTE) not fully extracted
- 2025 year-end stock price not in corpus
- Expal acquisition date not confirmed
- Service business actual 2025 figure vs EUR 1bn target not confirmed
- 2023-2024 division-level segment data (full breakdown) not yet extracted from those reports
- 2025 dividend/payout ratio not extracted

**Contradictions noted:**

- 2020 revenue: originally reported as 5,875 MioEUR (full group), later restated to 5,405 MioEUR (continuing) after IFRS 5 reclassification
- 2022 operating result: reported as 754 MioEUR in 2022 AR but restated to 769 MioEUR in later reports (likely PPA adjustment)
- 2023 ROCE: reported as 19.9% in 2023 AR but as 21.4% in 2025 AR (restatement)

## [2026-04-18] lint | Health check

- Unprocessed: 0 files (all 12 sources ingested)
- Found: 1 important, 2 medium, 1 minor
- Important: 3 contradictions logged but no questions/ pages filed (2020 revenue 5,875 vs 5,405 MioEUR; 2022 operating result 754 vs 769 MioEUR; 2023 ROCE 19.9% vs 21.4%)
- Medium: concepts/nato-2-percent-target.md in TOPIC.md but never created; over-budget pages (segment-performance.md 130 lines, rheinmetall-ag.md 96 lines)
- Minor: log format inconsistent with schema
- Open: File contradiction question pages; decide nato-2-percent-target

## [2026-04-19] maintenance | Standardize conventions

- Renamed frontmatter field raw_file to source_path across all 12 source summaries
- Standardized link format to markdown links (confirmed compatible with Obsidian graph view)
- Standardized log entry headers to [date] operation | title format

## [2026-04-19] maintenance | File contradiction questions + TOPIC cleanup

- Created: questions/2022-operating-result-discrepancy (754 vs 769 MioEUR operating result)
- Created: questions/2023-roce-discrepancy (19.9% vs 21.4% ROCE)
- Updated index.md with Open Questions section
- Removed nato-2-percent-target.md from TOPIC.md (content merged into european-defense-spending)
