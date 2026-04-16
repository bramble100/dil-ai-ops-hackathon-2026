# Amsterdam & Netherlands Travel Wiki - Schema

## Purpose

This wiki compiles and cross-references travel information about **Amsterdam and the Netherlands** from multiple multilingual sources (Spanish, Hungarian, Dutch, English). The goal is to transform scattered travel blog posts, hotel pages, and museum sites into an interconnected, navigable travel knowledge base -- organized by place, theme, and practical concern.

## Directory Structure

```
wiki/
  index.md              # Master catalog - every page listed with link and one-line summary
  log.md                # Chronological activity log (append-only)
  overview.md           # Amsterdam & Netherlands travel overview (the "front page")

  sources/              # One summary per raw source document
    viajeros-callejeros.md
    sinohasviajado.md
    amszterdam-nevezetessegek.md
    amszterdam-latnivalok.md
    cosas-tipicas.md
    port-van-cleve.md
    cruquius-wikipedia.md
    cruquius-nieuws.md
    top10-amszterdam.md
    keukenhof.md
    amsterdam-wiki-pdf.md

  places/               # Pages for specific locations, attractions, neighborhoods
    # Amsterdam
    rijksmuseum.md
    anne-frank-house.md
    van-gogh-museum.md
    vondelpark.md
    dam-square.md
    canal-ring.md
    red-light-district.md
    jordaan.md
    albert-cuyp-market.md
    ndsm-wharf.md
    a-dam-lookout.md

    # Greater Netherlands
    keukenhof.md
    zaanse-schans.md
    kinderdijk.md
    giethoorn.md
    delft.md
    hague.md
    rotterdam.md
    utrecht.md
    maastricht.md
    volendam-marken.md
    cruquius-museum.md

  themes/               # Thematic cross-cutting pages
    dutch-culture-and-traditions.md
    cycling-in-amsterdam.md
    canal-cruises.md
    dutch-cuisine.md
    museums-overview.md
    day-trips-from-amsterdam.md
    nightlife.md
    tulips-and-flowers.md
    windmills.md
    water-management.md

  practical/            # Practical travel information
    getting-around.md
    where-to-stay.md
    best-time-to-visit.md
    budget-tips.md
    itineraries.md

  hotels/               # Specific accommodation pages
    die-port-van-cleve.md
```

## Page Conventions

### Frontmatter

Every wiki page starts with YAML frontmatter:

```yaml
---
title: Page Title
type: source | place | theme | practical | hotel
sources: [viajeros-callejeros, top10-amszterdam]
created: 2026-04-16
updated: 2026-04-16
status: draft | complete
tags: [amsterdam, museum, must-see]
---
```

### Cross-linking

- Use standard markdown links: `[link text](../relative/path.md)`
- When referencing another wiki page, always link on first mention in a section
- Place names should link to their place page on first mention

### Claim Attribution

Label practical info by reliability:

- **[Source]** - Direct from a raw document (cite which article)
- **[Multi-source]** - Confirmed across 2+ independent sources
- **[Unverified]** - Mentioned in one source, not cross-checked
- **[Gap]** - Known missing information

### Language

- Wiki pages are written in **English**, even when sources are in Spanish, Hungarian, or Dutch
- Original-language names are preserved for proper nouns (e.g., "Rijksmuseum", "Vondelpark")
- Practical tips (prices, hours) note the source date since they change

## Ingest Workflow

When processing a new raw source:

1. **Read** the source document thoroughly
2. **Create** a source summary page in `wiki/sources/`
3. **Update** the index (`wiki/index.md`) with the new source entry
4. **Create or update** relevant place pages in `wiki/places/`
5. **Create or update** relevant theme pages in `wiki/themes/`
6. **Create or update** practical information pages in `wiki/practical/`
7. **Log** the ingest activity in `wiki/log.md`

## Quality Principles

1. **Cross-reference multilingual sources** - if Spanish and Hungarian sources agree on a recommendation, it's stronger
2. **Flag date-sensitive info** - prices, opening hours, and seasonal availability change
3. **Distinguish must-sees from hidden gems** - consensus picks vs. single-source recommendations
4. **Practical over promotional** - prioritize actionable travel info over marketing language
5. **Respect source diversity** - different cultural perspectives notice different things
