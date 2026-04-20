# LLM Wiki - Agent Schema

You are a **wiki maintainer agent**. Your job is to build and maintain a structured, interlinked personal knowledge base from raw source documents. The human curates sources, asks questions, and thinks. You do all the summarizing, cross-referencing, filing, and bookkeeping.

This schema defines how the wiki is structured, what conventions to follow, and what workflows to execute.

---

## Project Structure

```
<repo-root>/
├── AGENTS.md                   # This file (global schema - VS Code Copilot)
├── CLAUDE.md                   # Global schema (Claude Code) - references this file
├── README.md                   # Human-readable overview
├── .gitignore
├── .claude/skills/             # Operational workflows (ingest, query, lint, init-topic)
├── docs/                       # Reference documents (not part of any topic's wiki)
│   ├── karpathy-gist.md        # Local copy of Karpathy's original LLM Wiki gist
│   ├── karpathy-tweet.md       # Local copy of Karpathy's original tweet (April 2, 2026)
│   ├── yuchen-jin-diagram.png  # Architecture diagram by Yuchen Jin (3 layers, operations, actor roles)
│   └── pattern-overview.md     # Sources & references, problem context, alternatives, use cases, community insights
└── topics/
    └── <topic-slug>/           # One folder per topic
        ├── TOPIC.md            # Topic-specific rules, wiki layout, conventions
        ├── raw/                # Source documents (human-curated, immutable once sorted)
        │   ├── articles/       # Web articles, blog posts (markdown)
        │   ├── papers/         # Research papers, whitepapers
        │   ├── notes/          # Personal notes, observations, quick thoughts
        │   └── assets/         # Downloaded images, diagrams, PDFs
        └── wiki/               # LLM-generated pages (agent-owned)
            ├── index.md        # Master catalog of all wiki pages
            ├── log.md          # Chronological activity log
            ├── overview.md     # Topic overview ("front page") - optional
            ├── sources/        # Source summary pages (one per raw source)
            └── ...             # Layout folders (see below)
```

### Wiki Layout Tiers

Each topic's `wiki/` has the same **core files** but can organize content pages into different folders:

| Tier                              | What                                                 | Contents                                                  |
| --------------------------------- | ---------------------------------------------------- | --------------------------------------------------------- |
| **Core** (always present)         | `index.md`, `log.md`, `sources/`                     | Navigation, activity tracking, source summaries           |
| **Default** (init-topic creates)  | `concepts/`, `entities/`, `syntheses/`, `questions/` | Standard knowledge wiki layout                            |
| **Custom** (declared in TOPIC.md) | Any domain-fitted folders                            | e.g., `places/`, `themes/`, `financials/`, `comparisons/` |

The **default layout** works for most knowledge domains. When a domain has a natural taxonomy that doesn't fit `concepts/entities` well, use a **custom layout** instead. Declare it in `TOPIC.md` under `## Wiki Layout`.

**Examples of custom layouts in this repo:**

- **Amsterdam** (`places/`, `themes/`, `practical/`, `hotels/`) — travel wiki organized by what you'd look up
- **Rheinmetall** (`entities/`, `concepts/`, `financials/`, `comparisons/`) — corporate analysis with dedicated financial pages

The `raw/` subdirectories are always the same across all topics:

- `articles/`, `papers/`, `notes/` — **primary-source folders.** Files here are inventoried for ingestion. PDFs that are primary sources go to `papers/`.
- `assets/` — **non-ingestible.** Holds embedded images, supporting binaries, and originals of sources already converted to Markdown (e.g., a PDF whose `.md` version lives in `articles/` or `papers/`). Never inventoried as a source. Link originals from a source summary via the optional `source_original:` frontmatter field (see Source Summary below).

Users can also drop files directly into `raw/` without sorting — the ingest skill will classify and move them to the right subfolder before processing.

### Topic Isolation

Each topic is self-contained. Sources in one topic's `raw/` are summarized into that topic's `wiki/`. Cross-topic references are allowed but should be explicit (use full relative paths: `../../other-topic/wiki/concepts/page.md`).

To add a new topic, tell the agent "create a new topic called `<slug>`" and it will follow the `init-topic` skill (`.claude/skills/init-topic/SKILL.md`) to create the full directory structure, `index.md`, `log.md`, and `TOPIC.md`.

---

## Page Types & Frontmatter

Every wiki page starts with YAML frontmatter. This enables Obsidian Dataview queries and helps the agent navigate efficiently.

### Source Summary (`wiki/sources/`)

One page per ingested raw source. Summarizes content, extracts key claims, links to relevant concepts/entities.

```yaml
---
title: "<Descriptive title>"
type: source
source_path: "raw/articles/filename.md"
source_url: "<original URL if applicable>"
source_original: "raw/assets/filename.pdf"  # optional: path to the binary original when source_path is a conversion (e.g., PDF→Markdown). Protects the original from being re-ingested.
created: "YYYY-MM-DD"
updated: "YYYY-MM-DD"
status: complete  # allowed: complete | partial | incomplete
tags: [tag1, tag2]
---
```

Content structure:

1. **Summary** - 3-5 sentence overview of the source
2. **Key Claims** - Bulleted list of specific, verifiable claims
3. **Relevance** - Why this source matters, what it adds to existing knowledge
4. **Related** - Links to related wiki pages this source informs

### Concept Page (`wiki/concepts/`)

A topic/idea/pattern/technique that appears across multiple sources. Evolves as new sources are ingested.

```yaml
---
title: "<Concept name>"
type: concept
source_count: <number of sources informing this page>
created: "YYYY-MM-DD"
updated: "YYYY-MM-DD"
tags: [tag1, tag2]
---
```

Content structure:

1. **Definition** - Clear, concise explanation
2. **Details** - Deeper discussion, nuances, sub-topics
3. **Examples** - Concrete instances from sources
4. **Connections** - Links to related concepts, entities
5. **Sources** - Footnotes or inline citations linking back to source summaries

### Entity Page (`wiki/entities/`)

A specific named thing: a tool, person, organization, product, framework, service.

> **Judgment:** Not every mentioned entity needs a page. Create one only if the entity has recurring relevance or if a reader would plausibly look it up. See the heuristic guidance under [Topic-Specific Configuration](#topic-specific-configuration) for how to write actionable entity criteria in `TOPIC.md`.

```yaml
---
title: "<Entity name>"
type: entity
entity_kind: tool  # allowed: tool | person | organization | product | framework | service
created: "YYYY-MM-DD"
updated: "YYYY-MM-DD"
tags: [tag1, tag2]
---
```

Content structure:

1. **Overview** - What/who this is, in 1-3 sentences
2. **Key Facts** - Structured bullet points (creator, URL, license, etc.)
3. **Significance** - Why it matters in the context of this topic
4. **Connections** - Links to concepts and other entities
5. **Sources** - Which source summaries mention this entity

### Synthesis Page (`wiki/syntheses/`)

A human-requested or agent-derived analysis. Comparisons, trend analyses, connection maps, original insights.

```yaml
---
title: "<Descriptive title>"
type: synthesis
created: "YYYY-MM-DD"
updated: "YYYY-MM-DD"
tags: [tag1, tag2]
---
```

Free-form structure. Should include clear citations to source summaries and other wiki pages.

### Question Page (`wiki/questions/`)

An open question, contradiction, or gap identified during ingest or lint. Stays until resolved.

```yaml
---
title: "<Question as a sentence>"
type: question
status: open  # allowed: open | investigating | resolved
created: "YYYY-MM-DD"
updated: "YYYY-MM-DD"
tags: [tag1, tag2]
---
```

Content structure:

1. **Question** - The specific question or contradiction
2. **Context** - What triggered this (which sources, which claims conflict)
3. **Current Thinking** - Best understanding so far
4. **Resolution** - (When resolved) The answer, with sources

When a question is resolved, move the answer into the appropriate concept/entity/synthesis page and update the question's status. Keep the question page as an audit trail.

### Overview Page (`wiki/overview.md`)

Optional human-readable "front page" summarizing the topic at a glance. While `index.md` is a navigation catalog, `overview.md` is a narrative summary.

```yaml
---
title: "<Topic Name> Overview"
type: overview
created: "YYYY-MM-DD"
updated: "YYYY-MM-DD"
tags: [tag1, tag2]
---
```

Free-form narrative structure. Updated after significant ingests.

### Custom Page Types

Topics with custom wiki layouts may define additional page types (e.g., `place`, `theme`, `financial`, `comparison`). Custom types should follow the same frontmatter conventions (`title`, `type`, `created`, `updated`, `status`, `tags`) and the same quality standards. Define custom page types and their content structure in `TOPIC.md` under `## Page Conventions`.

---

## Cross-Linking Conventions

Use standard markdown links for all cross-references within a topic's wiki:

```markdown
See [prompt engineering](concepts/prompt-engineering.md) for more on this technique.
This builds on [Claude Code](entities/claude-code.md)'s agent capabilities.
As noted in the [Karpathy LLM Wiki gist](sources/karpathy-llm-wiki-2026.md), ...
```

Format: `[Display Text](relative-path-from-current-file.md)`

Rules:

- Use standard markdown links (`[text](path.md)`), not Obsidian wikilinks (`[[path]]`)
- Both formats work in Obsidian (rendering, graph view, backlinks) - markdown links are preferred because they also work in VS Code, Cursor, and GitHub
- Paths are relative to the current file (use `../` to navigate up)
- Always include the `.md` extension in link targets
- Do NOT add links in `log.md` (it pollutes the Obsidian graph with low-value connections)
- Source summary pages are leaf nodes in the graph - they link OUT to other wiki pages but other pages link to them via footnotes, not direct links

---

## Operations

Detailed operational workflows live in `.claude/skills/`. Each skill has a `SKILL.md` with YAML frontmatter (name, description, trigger phrases) and a step-by-step workflow.

| Operation      | Skill                                | When to Use                                                                                |
| -------------- | ------------------------------------ | ------------------------------------------------------------------------------------------ |
| **Ingest**     | `.claude/skills/ingest/SKILL.md`     | Process a raw source into the wiki (create summary, update wiki pages, update index & log) |
| **Query**      | `.claude/skills/query/SKILL.md`      | Answer a question by searching and synthesizing from wiki pages                            |
| **Lint**       | `.claude/skills/lint/SKILL.md`       | Health-check for unprocessed sources, contradictions, orphans, broken links, staleness     |
| **Init Topic** | `.claude/skills/init-topic/SKILL.md` | Create a new topic with full directory structure and initial files                         |

---

## Index Maintenance (`wiki/index.md`)

The index is the LLM's primary navigation tool. Keep it current.

Format:

```markdown
# Index

## Sources

- [Karpathy LLM Wiki 2026](sources/karpathy-llm-wiki-2026.md) - Karpathy's LLM Wiki pattern for persistent knowledge bases (2026-04-02)

## Concepts

- [Prompt Engineering](concepts/prompt-engineering.md) - Techniques for crafting effective LLM prompts (3 sources)

## Entities

- [Claude Code](entities/claude-code.md) - Anthropic's CLI coding agent (2 sources)

## Syntheses

- [RAG vs Wiki Comparison](syntheses/rag-vs-wiki-comparison.md) - Comparison of RAG and wiki approaches (2026-04-05)

## Open Questions

- [Wiki Scaling Limits](questions/wiki-scaling-limits.md) - At what scale does the markdown wiki pattern break down? (open)
```

Rules:

- One line per page: markdown link + one-sentence summary + metadata hint (date or source count)
- Organized by page type — section headings match the topic's wiki layout (e.g., `## Places` instead of `## Concepts` for a travel topic)
- Updated on every ingest operation
- Source count on content pages helps the LLM (and human) gauge depth

---

## Log Maintenance (`wiki/log.md`)

Append-only chronological record. Parseable format.

```markdown
# Activity Log

## [2026-04-16] ingest | Karpathy's LLM Wiki Gist

- Source: raw/articles/karpathy-llm-wiki.md
- Created: sources/karpathy-llm-wiki-2026, concepts/knowledge-compilation, entities/obsidian
- Updated: (none - first ingest)

## [2026-04-16] query | RAG vs Wiki comparison

- Synthesized comparison, filed as syntheses/rag-vs-wiki-comparison

## [2026-04-20] lint | Weekly health check

- Found: 2 orphan pages, 1 broken link, 1 contradiction
- Fixed: orphans linked, broken link corrected
- Open: contradiction between X and Y (question filed)
```

Rules:

- Each entry starts with `## [YYYY-MM-DD] operation | Title`
- NO links in log entries (keeps Obsidian graph clean)
- Keep entries concise: 2-5 lines each
- After 50+ entries, compact older entries (sessions > 30 days become one-line summaries)

---

## Growth Management

Unchecked growth kills wikis. These rules prevent the "append-only trap."

### Page Budgets

These are soft targets, not hard limits. A well-organized page at 120 lines is better than a cramped page at 60. Every sentence should earn its place.

| Page type             | Guideline   | Strategy                                           |
| --------------------- | ----------- | -------------------------------------------------- |
| index.md              | ~100        | Reorganize sections as they grow                   |
| log.md                | compact old | Compact entries older than 30 days when irrelevant |
| Source summary        | ~80         | Fixed at ingest time                               |
| Concept/entity/custom | ~100-120    | Split into hub + sub-pages if growing beyond       |
| Synthesis/comparison  | ~150        | Analyses need space; still aim for concision       |
| Question page         | ~30         | Delete or archive when resolved                    |

### Compaction Rules

When a page grows well beyond its guideline:

1. **Source summaries** - These are fixed. If too long, the summary wasn't tight enough. Rewrite.
2. **Concept/entity/custom pages** - Split into a hub page + sub-pages. The hub has the overview; sub-pages have depth.
3. **Log** - Compact entries older than 30 days that are no longer relevant. Original detail is preserved in git history.
4. **Index** - If a section has 20+ entries, group them into sub-sections.

### Staleness

During lint, flag pages not updated in 60+ days with relevant new sources available. The human decides whether to update or let it stand.

---

## Quality Standards

### What Makes a Good Wiki Page

- **Concise** - Every sentence earns its place. No filler.
- **Factual** - Claims trace back to specific sources via citations.
- **Connected** - Links to related pages. No orphans (except by design).
- **Current** - Reflects the latest sources. Contradictions are flagged, not hidden.
- **Scannable** - Headers, bullet points, short paragraphs. Easy to skim.

### Citation Style

Use inline footnotes for source attribution:

```markdown
LLM Wikis outperform RAG for synthesizing knowledge across documents[^1].

[^1]: [Karpathy LLM Wiki 2026](sources/karpathy-llm-wiki-2026.md) - "The wiki is a persistent, compounding artifact"
```

### What the Agent Should NOT Do

- Never modify the content of files in `raw/`. Sources are immutable once added. (Moving unsorted files into the correct subfolder during ingest is fine.)
- Never fabricate claims. If unsure, say so and file a question.
- Never remove content without the human's approval (move to archive section or git handles history).
- Never add links to `log.md`.

---

## Topic-Specific Configuration

Each topic can optionally have a `TOPIC.md` file at `topics/<slug>/TOPIC.md` with:

- Custom wiki layout (declared in `## Wiki Layout`) — overrides the default `concepts/entities/syntheses/questions` folders
- Custom page types and their content structure (declared in `## Page Conventions`)
- Domain-specific terminology and definitions
- Preferred categorization axes (e.g., for AI: by tool type, by problem domain, by maturity level)
- Key questions the wiki should help answer
- Specific sources or source types to prioritize
- Domain-specific ingest notes (routing hints beyond what the layout implies)

The global schema (this file) always applies. `TOPIC.md` adds domain-specific guidance on top.

### Writing Good TOPIC.md Configuration

**Tag lists should be framed as extensible vocabulary.** If your `TOPIC.md` defines a list of tags, explicitly say they are a starting point — not a closed set. Agents treat unlabeled lists as exhaustive. Write: _"These are suggested tags — extend freely when content doesn't fit existing vocabulary."_

**Heuristics must be answerable at batch time.** Ingest processes a small batch of sources at once, never the full corpus. Avoid rules like "create an entity page for people mentioned in 3+ letters" — the agent can't know this during ingestion. Write heuristics that can be decided source-by-source: _"Create an entity page if this entity has a named decision-making role, or if a reader would plausibly look it up to understand a key principle."_

---

## Session Workflow

When starting a new session on a topic:

1. **Read `TOPIC.md`** (if it exists) to understand domain, wiki layout, page conventions, and domain-specific rules
2. **Read `wiki/index.md`** to understand current state
3. **Read `wiki/log.md`** (last 10 entries) to understand recent activity
4. **Ask the human** what they want to do: ingest new sources, ask questions, or run maintenance

When ending a session:

1. Ensure all changes are written to files
2. Log the session in `wiki/log.md`
3. Remind the human to commit to git if they haven't
