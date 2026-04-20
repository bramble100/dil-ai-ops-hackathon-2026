# Amsterdam & Netherlands Travel Wiki - Schema

## Purpose

This wiki compiles and cross-references travel information about **Amsterdam and the Netherlands** from multiple multilingual sources (Spanish, Hungarian, Dutch, English). The goal is to transform scattered travel blog posts, hotel pages, and museum sites into an interconnected, navigable travel knowledge base -- organized by place, theme, and practical concern.

## Wiki Layout

- `sources/` - One summary per raw source document (always present)
- `places/` - Specific locations, attractions, neighborhoods (Amsterdam + Greater Netherlands)
- `themes/` - Cross-cutting thematic pages (culture, cuisine, cycling, canals, etc.)
- `practical/` - How-to and logistics (transport, budget, timing, itineraries)
- `hotels/` - Specific accommodation pages

## Page Conventions

### Frontmatter

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

## Key Questions

1. What are the must-see attractions in Amsterdam and the Netherlands?
2. How to plan a trip: timing, transport, budget, itineraries?
3. What's beyond the tourist highlights — hidden gems, local culture, day trips?

## Ingest Notes

- Route place-specific content to `places/` (one page per attraction or city)
- Route cross-cutting topics (cuisine, cycling, culture) to `themes/`
- Route logistics and how-to content to `practical/`
- Cross-reference multilingual sources: if Spanish and Hungarian sources agree, it strengthens the recommendation
- Flag date-sensitive info: prices, opening hours, and seasonal availability change
- Distinguish must-sees from hidden gems: consensus picks vs. single-source recommendations
- **Practical over promotional** — prioritize actionable travel info over marketing language

5. **Respect source diversity** - different cultural perspectives notice different things
