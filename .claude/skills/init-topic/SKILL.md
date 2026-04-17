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
2. **Scaffold** - Create directory structure with .gitkeep files
3. **Create index** - Empty master catalog
4. **Create log** - Bootstrap entry
5. **Create TOPIC.md** - Domain-specific configuration
6. **Confirm** - Summarize what was created

---

## Phase 1: Confirm Topic Details

Ask the user (if not already provided):

- **Slug:** Short, lowercase, hyphenated folder name (e.g., `ai-engineering`, `product-analytics`, `competitive-analysis`). Must be URL-safe and unique - check that `topics/<slug>/` doesn't already exist.
- **Domain description:** One sentence describing what this topic covers. Goes into TOPIC.md and the index.md header.

---

## Phase 2: Create Directory Structure

```bash
mkdir -p topics/<slug>/{raw/{articles,papers,notes,assets},wiki/{concepts,entities,sources,syntheses,questions}}
```

Create `.gitkeep` files in each empty directory so git tracks them:

- `topics/<slug>/raw/articles/.gitkeep`
- `topics/<slug>/raw/papers/.gitkeep`
- `topics/<slug>/raw/notes/.gitkeep`
- `topics/<slug>/raw/assets/.gitkeep`
- `topics/<slug>/wiki/concepts/.gitkeep`
- `topics/<slug>/wiki/entities/.gitkeep`
- `topics/<slug>/wiki/sources/.gitkeep`
- `topics/<slug>/wiki/syntheses/.gitkeep`
- `topics/<slug>/wiki/questions/.gitkeep`

---

## Phase 3: Create wiki/index.md

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

```markdown
# <Topic Name> - Topic Configuration

## Domain

<Domain description - expanded to 2-3 sentences explaining the scope and perspective.>

## Scope

- **<Category 1>:** <what it includes>
- **<Category 2>:** <what it includes>
- <...3-6 categories that cover the topic's main dimensions>

## Categorization Axes

When creating concept pages, consider categorizing along these dimensions:

- **<Axis 1>:** <values> (e.g., maturity: experimental | emerging | established)
- **<Axis 2>:** <values>

## Key Questions This Wiki Should Help Answer

1. <question 1>
2. <question 2>
3. <...3-5 questions that define the topic's direction>
```

Work with the user to fill in scope and questions. If the user doesn't have strong opinions, propose reasonable defaults based on the domain.

---

## Phase 6: Confirm

Tell the user:

- What was created (list the files)
- How to start adding sources: drop files into `raw/articles/`, then say "ingest"
- Remind them to commit to git

---

## Edge Cases

- **Slug already exists:** Refuse to create. Tell the user and ask for a different name.
- **Don't create sample content.** The wiki starts empty. Placeholder text in index.md is fine; fake source summaries or concept pages are not.
- **TOPIC.md is optional but valuable.** If the user wants to skip it, that's fine - the wiki works without it. But domain-specific guidance helps the agent make better judgment calls during ingest.
