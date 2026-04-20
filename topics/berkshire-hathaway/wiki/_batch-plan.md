# Batch Ingestion Plan

This file tracks batch-by-batch ingestion progress. The agent reads this at the start of each session to find the next unchecked batch. The human can adjust batch assignments at any time.

**How to use:** Start a fresh session and say: _"Read TOPIC.md, index.md, log.md, and \_batch-plan.md. Process the next batch."_

**Sizing note:** Letters from 1982 onward average 1,000–2,300 lines each. Batches are deliberately kept to 2–3 letters (≤ ~3,000 source lines) to stay well under the context threshold once infrastructure files and existing wiki pages are added to the load. Do not merge batches.

## Batches

### Batch 1 — 1977-1981 (early Berkshire, short letters ~3,560 lines total)

- [x] buffett-letter-1977.md
- [x] buffett-letter-1978.md
- [x] buffett-letter-1979.md
- [x] buffett-letter-1980.md
- [x] buffett-letter-1981.md

### Batch 2 — 1982-1983 (~2,250 lines)

- [x] buffett-letter-1982.md
- [x] buffett-letter-1983.md

### Batch 3 — 1984-1985 (~3,170 lines)

- [x] buffett-letter-1984.md
- [x] buffett-letter-1985.md

### Batch 4 — 1986-1987 (~2,990 lines)

- [x] buffett-letter-1986.md
- [x] buffett-letter-1987.md

### Batch 5 — 1988-1989 (~3,200 lines)

- [x] buffett-letter-1988.md
- [x] buffett-letter-1989.md

### Batch 6 — 1990-1992 (~3,130 lines; 1990 is short at 644 lines)

- [x] buffett-letter-1990.md
- [x] buffett-letter-1991.md
- [x] buffett-letter-1992.md

### Batch 7 — 1993-1994 (~2,530 lines)

- [x] buffett-letter-1993.md
- [x] buffett-letter-1994.md

### Batch 8 — 1995-1996 (~2,700 lines)

- [ ] buffett-letter-1995.md
- [ ] buffett-letter-1996.md

### Batch 9 — 1997-1999 (~2,570 lines; 1997 is short at 486 lines)

- [ ] buffett-letter-1997.md
- [ ] buffett-letter-1998.md
- [ ] buffett-letter-1999.md

### Batch 10 — 2000-2001 (~2,520 lines)

- [ ] buffett-letter-2000.md
- [ ] buffett-letter-2001.md

### Batch 11 — 2002-2003 (~3,000 lines)

- [ ] buffett-letter-2002.md
- [ ] buffett-letter-2003.md

### Batch 12 — 2004 (~2,340 lines; longest single letter)

- [ ] buffett-letter-2004.md

### Batch 13 — 2005 (~1,930 lines)

- [ ] buffett-letter-2005.md

### Batch 14 — 2006 (~1,960 lines)

- [ ] buffett-letter-2006.md

### Batch 15 — 2007-2008 (~3,210 lines)

- [ ] buffett-letter-2007.md
- [ ] buffett-letter-2008.md

### Batch 16 — 2009-2010 (~2,960 lines)

- [ ] buffett-letter-2009.md
- [ ] buffett-letter-2010.md

### Batch 17 — 2011-2012 (~2,850 lines)

- [ ] buffett-letter-2011.md
- [ ] buffett-letter-2012.md

### Batch 18 — 2013 (~1,430 lines)

- [ ] buffett-letter-2013.md

### Batch 19 — 2014 (~2,230 lines)

- [ ] buffett-letter-2014.md

### Batch 20 — 2015 (~1,700 lines)

- [ ] buffett-letter-2015.md

### Batch 21 — 2016-2017 (~2,870 lines)

- [ ] buffett-letter-2016.md
- [ ] buffett-letter-2017.md

### Batch 22 — 2018-2020 (~2,510 lines)

- [ ] buffett-letter-2018.md
- [ ] buffett-letter-2019.md
- [ ] buffett-letter-2020.md

### Batch 23 — 2021-2023 (~1,950 lines)

- [ ] buffett-letter-2021.md
- [ ] buffett-letter-2022.md
- [ ] buffett-letter-2023.md

### Batch 24 — 2024-2025 (~1,790 lines)

- [ ] buffett-letter-2024.md
- [ ] buffett-letter-2025.md

## Post-Ingestion

After all batches are complete:

- [ ] Review and polish standalone pages (quotes.md, timeline.md, acquisitions-timeline.md)
- [ ] Run lint skill for full health check
- [ ] Create overview.md (topic front page)
- [ ] Human review pass on principle pages (the core product)
