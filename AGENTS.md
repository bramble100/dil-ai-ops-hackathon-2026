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
    └── <topic-slug>/           # One folder per topic (e.g., ai-engineering)
        ├── TOPIC.md            # Optional: topic-specific rules/context
        ├── raw/                # Immutable source documents (human-curated)
        │   ├── articles/       # Web articles, blog posts (markdown)
        │   ├── papers/         # Research papers, whitepapers
        │   ├── notes/          # Personal notes, observations, quick thoughts
        │   └── assets/         # Downloaded images, diagrams, PDFs
        └── wiki/               # LLM-generated pages (agent-owned)
            ├── index.md        # Master catalog of all wiki pages
            ├── log.md          # Chronological activity log
            ├── concepts/       # Concept & topic pages
            ├── entities/       # Entity pages (tools, people, orgs, products)
            ├── sources/        # Source summary pages (one per raw source)
            ├── syntheses/      # Analyses, comparisons, connections
            └── questions/      # Open questions, contradictions to investigate
```

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
date_ingested: "YYYY-MM-DD"
tags: [tag1, tag2]
---
```

Content structure:

1. **Summary** - 3-5 sentence overview of the source
2. **Key Claims** - Bulleted list of specific, verifiable claims
3. **Relevance** - Why this source matters, what it adds to existing knowledge
4. **Related** - Links to concept/entity pages this source informs

### Concept Page (`wiki/concepts/`)

A topic/idea/pattern/technique that appears across multiple sources. Evolves as new sources are ingested.

```yaml
---
title: "<Concept name>"
type: concept
source_count: <number of sources informing this page>
last_updated: "YYYY-MM-DD"
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

```yaml
---
title: "<Entity name>"
type: entity
entity_kind: "tool | person | organization | product | framework | service"
last_updated: "YYYY-MM-DD"
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
date_created: "YYYY-MM-DD"
last_updated: "YYYY-MM-DD"
tags: [tag1, tag2]
---
```

Free-form structure. Should include clear citations to source summaries and concept/entity pages.

### Question Page (`wiki/questions/`)

An open question, contradiction, or gap identified during ingest or lint. Stays until resolved.

```yaml
---
title: "<Question as a sentence>"
type: question
status: "open | investigating | resolved"
date_opened: "YYYY-MM-DD"
date_resolved: "YYYY-MM-DD"
tags: [tag1, tag2]
---
```

Content structure:

1. **Question** - The specific question or contradiction
2. **Context** - What triggered this (which sources, which claims conflict)
3. **Current Thinking** - Best understanding so far
4. **Resolution** - (When resolved) The answer, with sources

When a question is resolved, move the answer into the appropriate concept/entity/synthesis page and update the question's status. Keep the question page as an audit trail.

---

## Cross-Linking Conventions (Obsidian)

Use Obsidian-style wikilinks for all cross-references within a topic's wiki:

```markdown
See [[concepts/prompt-engineering]] for more on this technique.
This builds on [[entities/claude-code|Claude Code]]'s agent capabilities.
As noted in [[sources/karpathy-llm-wiki-2026|Karpathy's LLM Wiki gist]], ...
```

Format: `[[relative-path-from-wiki-root|Optional Display Text]]`

Rules:

- Links are relative to the `wiki/` directory
- Always include the subfolder: `[[concepts/name]]`, not just `[[name]]`
- Use display text when the filename isn't reader-friendly: `[[entities/vscode|VS Code]]`
- Do NOT add wikilinks in `log.md` (it pollutes the Obsidian graph with low-value connections)
- Source summary pages are leaf nodes in the graph - they link OUT to concepts/entities but other pages link to them via footnotes, not wikilinks

---

## Operations

Detailed operational workflows live in `.claude/skills/`. Each skill has a `SKILL.md` with YAML frontmatter (name, description, trigger phrases) and a step-by-step workflow.

| Operation      | Skill                                | When to Use                                                                                       |
| -------------- | ------------------------------------ | ------------------------------------------------------------------------------------------------- |
| **Ingest**     | `.claude/skills/ingest/SKILL.md`     | Process a raw source into the wiki (create summary, update concepts/entities, update index & log) |
| **Query**      | `.claude/skills/query/SKILL.md`      | Answer a question by searching and synthesizing from wiki pages                                   |
| **Lint**       | `.claude/skills/lint/SKILL.md`       | Health-check for unprocessed sources, contradictions, orphans, broken links, staleness            |
| **Init Topic** | `.claude/skills/init-topic/SKILL.md` | Create a new topic with full directory structure and initial files                                |

---

## Index Maintenance (`wiki/index.md`)

The index is the LLM's primary navigation tool. Keep it current.

Format:

```markdown
# Index

## Sources

- [[sources/karpathy-llm-wiki-2026]] - Karpathy's LLM Wiki pattern for persistent knowledge bases (2026-04-02)

## Concepts

- [[concepts/prompt-engineering]] - Techniques for crafting effective LLM prompts (3 sources)

## Entities

- [[entities/claude-code]] - Anthropic's CLI coding agent (2 sources)

## Syntheses

- [[syntheses/rag-vs-wiki-comparison]] - Comparison of RAG and wiki approaches (2026-04-05)

## Open Questions

- [[questions/wiki-scaling-limits]] - At what scale does the markdown wiki pattern break down? (open)
```

Rules:

- One line per page: wikilink + one-sentence summary + metadata hint (date or source count)
- Organized by page type
- Updated on every ingest operation
- Source count on concept/entity pages helps the LLM (and human) gauge depth

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
- NO wikilinks in log entries (keeps Obsidian graph clean)
- Keep entries concise: 2-5 lines each
- After 50+ entries, compact older entries (sessions > 30 days become one-line summaries)

---

## Growth Management

Unchecked growth kills wikis. These rules prevent the "append-only trap."

### Page Budgets

| Page type      | Target max lines | Strategy                                     |
| -------------- | ---------------- | -------------------------------------------- |
| index.md       | ~100             | Reorganize sections as they grow             |
| log.md         | ~60 active       | Compact entries older than 30 days           |
| Source summary | ~50              | Fixed at ingest time                         |
| Concept page   | ~80              | Split into sub-concepts if growing beyond    |
| Entity page    | ~60              | Focus on significance, not exhaustive detail |
| Synthesis page | ~100             | These can be longer (analyses need space)    |
| Question page  | ~30              | Delete or archive when resolved              |

### Compaction Rules

When a page exceeds its budget:

1. **Source summaries** - These are fixed. If too long, the summary wasn't tight enough. Rewrite.
2. **Concept/entity pages** - Split into a hub page + sub-pages. The hub has the overview; sub-pages have depth.
3. **Log** - Sessions older than 30 days become one-line summaries. Original detail is preserved in git history.
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

[^1]: [[sources/karpathy-llm-wiki-2026]] - "The wiki is a persistent, compounding artifact"
```

### What the Agent Should NOT Do

- Never modify files in `raw/`. Sources are immutable.
- Never fabricate claims. If unsure, say so and file a question.
- Never remove content without the human's approval (move to archive section or git handles history).
- Never add wikilinks to `log.md`.

---

## Topic-Specific Configuration

Each topic can optionally have a `TOPIC.md` file at `topics/<slug>/TOPIC.md` with:

- Domain-specific terminology and definitions
- Preferred categorization axes (e.g., for AI: by tool type, by problem domain, by maturity level)
- Key questions the wiki should help answer
- Specific sources or source types to prioritize

The global schema (this file) always applies. `TOPIC.md` adds domain-specific guidance on top.

---

## Session Workflow

When starting a new session on a topic:

1. **Read `wiki/index.md`** to understand current state
2. **Read `wiki/log.md`** (last 10 entries) to understand recent activity
3. **Ask the human** what they want to do: ingest new sources, ask questions, or run maintenance

When ending a session:

1. Ensure all changes are written to files
2. Log the session in `wiki/log.md`
3. Remind the human to commit to git if they haven't
