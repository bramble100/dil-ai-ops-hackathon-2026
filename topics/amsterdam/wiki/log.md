---
title: Activity Log
type: overview
created: 2026-04-16
updated: 2026-04-19
---

# Activity Log

## [2026-04-16] init | Wiki bootstrapped

- Initial wiki creation
  **Sources inventoried:** 11 files (10 HTML web pages + 1 PDF)
  **Languages:** Spanish (3), Hungarian (3), Dutch (1), English (3), PDF (1)
  **Pages created:** SCHEMA.md, index.md, log.md, overview.md
  **Notes:** Sources are saved web pages (HTML + assets). Topics span Amsterdam attractions, Netherlands day trips, Dutch culture, specific venues (Keukenhof, Cruquius Museum, Die Port van Cleve hotel). Content appears to be trip-planning research for a 2026 visit.

## [2026-04-16] ingest | Full batch (11 sources)

- Full batch ingest of all 11 source documents
  **Sources processed:**

- 10 lugares que ver en Holanda (Viajeros Callejeros, Spanish, ~28,800 chars) -- 10 must-see places
- 30 cosas que hacer y ver en los Paises Bajos (Sinohasviajado, Spanish, 61 chars) -- **extraction failed**
- Amszterdam Nevezetessegek, latnivalok 2026 (Hungarian, ~66,700 chars) -- comprehensive Amsterdam guide
- Amszterdam latnivalok: amiket ne hagyj ki [2026-ban] (Hungarian, ~17,900 chars) -- sights + transport
- Top 10 latnivalo Amszterdamban (Hungarian, ~11,600 chars) -- top 10 with cultural context
- Cosas tipicas de Holanda (Spanish, ~18,000 chars) -- Dutch culture (**partial extraction, UI noise**)
- Die Port van Cleve (English, ~4,400 chars) -- historic hotel official site
- Museum De Cruquius - Wikipedia (English, ~7,400 chars) -- steam pumping station history
- Nieuws | Cruquius Museum (Dutch, ~3,100 chars) -- museum news 2025-2026
- Welcome to Keukenhof (English, ~17,800 chars) -- flower garden 2026 season
- AmsterdamWiki.pdf (24 pages, **IMAGE-ONLY**, 0 extractable text) -- needs OCR

**Pages created (48 total):**

- Navigation: index.md, overview.md
- Source summaries (11): viajeros-callejeros, sinohasviajado, amszterdam-nevezetessegek, amszterdam-latnivalok, top10-amszterdam, cosas-tipicas, port-van-cleve, cruquius-wikipedia, cruquius-nieuws, keukenhof, amsterdam-wiki-pdf
- Amsterdam places (11): rijksmuseum, van-gogh-museum, anne-frank-house, dam-square, canal-ring, vondelpark, nemo-science-museum, red-light-district, albert-cuyp-market, begijnhof, bloemenmarkt
- Greater Netherlands places (11): keukenhof, zaanse-schans, delft, utrecht, kinderdijk, rotterdam, volendam-marken, giethoorn, hague, maastricht, cruquius-museum
- Theme pages (10): dutch-culture-and-traditions, cycling-in-amsterdam, canal-cruises, dutch-cuisine, museums-overview, day-trips-from-amsterdam, tulips-and-flowers, windmills, water-management, nightlife
- Practical pages (5): getting-around, where-to-stay, best-time-to-visit, budget-tips, itineraries
- Hotels (1): die-port-van-cleve
- Working documents: \_extraction_notes.md

**Key data extracted:**

- Complete museum pricing and hours for 2026 (Van Gogh EUR 25, Rijksmuseum EUR 25, Anne Frank EUR 15.50, Royal Palace EUR 13.50, Oude Kerk EUR 14.50, etc.)
- Transport pass pricing (GVB 24h EUR 10 to 96h EUR 27.50; I Amsterdam Card 1d EUR 67 to 4d EUR 130)
- Keukenhof 2026 season dates: March 19 - May 10 with event calendar
- Canal stats: 165 canals, 100+ km, 1,281 bridges (3x Venice), ~25,000 bikes/year in water
- Cruquius Museum reopened March 31, 2026 after 2+ years of bridge works
- Die Port van Cleve: 122 rooms, 8 themes, founded 1864 with Heineken connection
- Cross-source attraction consensus: Rijksmuseum, Van Gogh, Anne Frank, canals mentioned in 5/11 sources

**Gaps identified:**

- Sinohasviajado source: extraction yielded only 61 chars (noscript tags broke parser)
- AmsterdamWiki.pdf: image-only, 0 extractable text without OCR
- Cosas tipicas: partial extraction, much UI noise captured instead of article content
- No 2024 annual report equivalent (this is travel, not finance -- N/A)
- NEMO Science Museum: entry price not captured
- Single-ride GVB ticket price not in sources
- Schiphol Airport transport details not covered by sources
- Accommodation pricing (beyond Die Port van Cleve) not available

**Extraction method:**

- HTML sources: Python BeautifulSoup (html.parser) with script/style/nav removal
- PDF: PyMuPDF + visual page reading (image-only content)
- 2 sources had extraction quality issues; 1 PDF not extractable

**Cross-source validation highlights:**

- Rijksmuseum, Van Gogh Museum, Anne Frank House confirmed as top-3 by Spanish + Hungarian + English sources
- Canal count (165) and bridge count (1,281) consistent across Hungarian sources
- Transport pricing consistent between the two Hungarian 2026 guides
- Royal Palace entry price (EUR 13.50) confirmed across two Hungarian sources

## [2026-04-18] lint | Health check

- Unprocessed: 0 files (all 11 sources ingested)
- Found: 2 medium, 2 minor issues
- Medium: Several source summaries over budget; TOPIC.md lists 3 unbuilt place pages (jordaan, ndsm-wharf, a-dam-lookout)
- Minor: log format inconsistent with schema; TOPIC.md cleanup needed

## [2026-04-19] maintenance | Standardize conventions

- Renamed frontmatter field raw_file to source_path across all 11 source summaries
- Standardized link format to markdown links (confirmed compatible with Obsidian graph view)
- Standardized log entry headers to [date] operation | title format

## [2026-04-19] maintenance | TOPIC.md cleanup

- Removed 3 unbuilt place pages from TOPIC.md directory listing: jordaan, ndsm-wharf, a-dam-lookout
- No source coverage exists for these locations; can be re-added if sources are ingested

## [2026-04-25] lint | Health check

- Unprocessed: 0 files (all 11 sources ingested)
- Found: 1 important (28 broken links across 9 source summaries), 2 medium, 1 minor
- Fixed: 28 broken links corrected — 3 wrong filenames (port-van-cleve, dutch-culture, food-and-drink), 7 repointed to correct existing pages (nemo, transport, day-trips, accommodation, passes-and-cards), 5 removed (pages never created: artis-zoo, maritime-museum, rembrandt-house, royal-palace, events-calendar)
- Fixed: overview.md missing status field; index.md stale updated date
- Open: 2 incomplete source extractions (sinohasviajado, amsterdam-wiki-pdf) — needs human decision on re-extraction
