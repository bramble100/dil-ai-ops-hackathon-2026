---
name: init-topic
description: Creates a new wiki topic with the full directory structure, index, log, and topic configuration file. Use when the user wants to start tracking a new knowledge domain. Triggers on phrases like "new topic", "create topic", "initialize topic", "add a topic about", "start a wiki on".
---

# Initialize New Topic

**Purpose:** Create a new topic with the full directory structure, initial files (index, log), and a topic configuration file - ready for the first source ingest.

## When to Apply

- User wants to start a wiki on a new subject area
- User says "new topic", "create topic", or "add a topic about X"

---

## Workflow Overview

1. **Confirm** - Gather topic slug and domain description
2. **Discover layout** - Help the user choose a wiki structure
3. **Scaffold** - Create directory structure with .gitkeep files
4. **Create index** - Empty master catalog matching the layout
5. **Create log** - Bootstrap entry
6. **Create TOPIC.md** - Domain-specific configuration with wiki layout
7. **Confirm** - Summarize what was created

---

## Phase 1: Confirm Topic Details

Ask the user (if not already provided):

- **Slug:** Short, lowercase, hyphenated folder name (e.g., `ai-engineering`, `product-analytics`, `competitive-analysis`). Must be URL-safe and unique - check that `topics/<slug>/` doesn't already exist.
- **Domain description:** One sentence describing what this topic covers. Goes into TOPIC.md and the index.md header.

---

## Phase 1.5: Discover Layouts

Help the user choose between the **default** and a **custom layout** for both `raw/` (input) and `wiki/` (output).

**Ask about:**

- Expected source types (web articles, PDFs, financial reports, transcripts, analytics exports, daily notes, etc.)
- Key questions the wiki should answer
- Optionally: "Drop 2-3 example sources into `raw/` and I'll analyze them to suggest a structure"

**If example sources provided:** Read them, identify natural groupings (people, places, financial data, technical concepts, timelines, etc.), and propose folders with rationale for both `raw/` and `wiki/`.

**Propose a raw layout:**

- "Based on your source types, I suggest organizing `raw/` as: `articles/`, `reports/`, `notes/`. Does this work?"
- User confirms or adjusts.
- **Subfolder option:** If the user's sources are diverse (e.g. articles + reports + notes + data), propose subfolders fitted to the domain.
- **Fallback:** If the user has no strong opinions, use flat layout (all sources in `raw/` root, no subfolders).

**Propose a wiki layout:**

- "Based on [rationale], I suggest: `sources/`, `entities/`, `financials/`, `comparisons/`. Does this work?"
- User confirms or adjusts.
- **Fallback:** If the user has no examples and no strong opinions, use the default wiki layout: `concepts/`, `entities/`, `syntheses/`, `questions/`.

Record the chosen layouts for use in Phases 2, 3, and 5.

---

## Phase 2: Create Directory Structure

Create the `raw/` directories matching the chosen raw layout and `wiki/` directories matching the chosen wiki layout:

```bash
# Raw directories (layout-dependent)
# Default (flat layout — no subfolders):
mkdir -p topics/<slug>/raw
# Subfolder example (general knowledge):
mkdir -p topics/<slug>/raw/{articles,papers,notes,assets}
# Subfolder example (competitor analysis):
mkdir -p topics/<slug>/raw/{competitor-snapshots,analytics-exports,market-research,assets}

# Originals directory (binary originals of converted sources — never ingested)
mkdir -p topics/<slug>/originals

# Wiki directories (layout-dependent)
mkdir -p topics/<slug>/wiki/sources
# + one directory per layout folder, e.g.:
mkdir -p topics/<slug>/wiki/{concepts,entities,syntheses,questions}  # default
# or:
mkdir -p topics/<slug>/wiki/{places,themes,practical,hotels}         # custom example
```

Create `.gitkeep` files in each empty directory so git tracks them.

---

## Phase 3: Create wiki/index.md

Section headings match the chosen layout. For the default layout:

```markdown
# Index

> <domain description from Phase 1>

## Sources

_No sources ingested yet. Drop a file into `raw/` and ask the agent to ingest it._

## Concepts

_No concept pages yet. These will be created as sources are ingested._

## Entities

_No entity pages yet. These will be created as sources are ingested._

## Syntheses

_No synthesis pages yet. Ask the agent a question and request it be filed._

## Open Questions

_No open questions yet._
```

For a custom layout, replace section names to match (e.g., `## Places`, `## Themes`, `## Practical` for a travel topic).

---

## Phase 4: Create wiki/log.md

```markdown
# Activity Log

## [YYYY-MM-DD] init | Wiki bootstrapped

- Topic created: <slug>
- Domain: <domain description>
- Created: index.md, log.md, directory structure
```

---

## Phase 5: Create TOPIC.md

````markdown
# <Topic Name> - Topic Configuration

## Purpose

<Domain description - expanded to 2-3 sentences explaining the scope and perspective.>

## Raw Layout

- `<folder>/` - <description>
- `<folder>/` - <description>
- <...one line per raw subfolder chosen in Phase 1.5>
- <For flat layout, write: "Flat — all sources in `raw/` root, no subfolders.">

## Wiki Layout

- `sources/` - One summary per raw source (always present)
- `<folder>/` - <description>
- `<folder>/` - <description>
- <...one line per layout folder chosen in Phase 1.5>

## Page Conventions

### Frontmatter

```yaml
---
title: Page Title
type: source # or a custom type matching the layout
created: YYYY-MM-DD
updated: YYYY-MM-DD
status: draft # allowed: draft | complete
tags: [tag1, tag2]
---
```

<Add domain-specific conventions: claim attribution labels, language rules, number formatting, etc.>

## Key Questions

1. <question 1>
2. <question 2>
3. <...3-5 questions that define the topic's direction>
````

Work with the user to fill in layout, conventions, and questions. If the user doesn't have strong opinions, propose reasonable defaults based on the domain.

---

## Phase 6: Confirm

Tell the user:

- What was created (list the files and the chosen raw + wiki layouts)
- Optionally create `wiki/overview.md` with placeholder text: "Overview will be populated after the first ingest session." (frontmatter: `type: overview`)
- How to start adding sources: drop files into `raw/` (or the appropriate subfolder if using a subfolder layout), then say "ingest"
- Remind them to commit to git

---

## Edge Cases

- **Slug already exists:** Refuse to create. Tell the user and ask for a different name.
- **Don't create sample content.** The wiki starts empty. Placeholder text in index.md is fine; fake source summaries or concept pages are not.
- **TOPIC.md is optional but valuable.** If the user wants to skip it, that's fine - the wiki works without it. But domain-specific guidance helps the agent make better judgment calls during ingest.
