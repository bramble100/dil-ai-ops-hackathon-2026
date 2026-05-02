# LLM Wiki

A personal knowledge base built and maintained by LLM agents, based on [Andrej Karpathy's LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

> This repository was created for the **Diligent AI Ops Hackathon 2026**. For the original framing, motivation, and architecture overview, see [`docs/hackathon-brief.md`](docs/hackathon-brief.md).

---

## What is it?

Instead of asking an AI to re-search your documents from scratch every time you have a question, you have the AI **build and maintain a personal wiki** from those documents. The model reads your sources, writes organized summaries, connects related ideas, and keeps everything up to date. You browse the result like Wikipedia, except it is built around your material and your questions.

Short version: **RAG is a search engine. LLM Wiki is a librarian who has read the library and keeps it organized.**

## Why this pattern exists

The common workflow for "chat with my documents" looks like this:

1. Upload a pile of files.
2. Ask a question.
3. The model searches chunks, assembles an answer, and stops there.
4. Ask a new question and the process starts over.

That works, but it has clear limits:

- **No accumulation.** Good answers do not become durable structure.
- **No maintained relationships.** Cross-document links, overlaps, and contradictions are usually not tracked.
- **Limited global context.** As the corpus grows, each query sees less of the whole picture.
- **Ephemeral output.** The value lives in chat history instead of a maintained artifact.

**LLM Wiki flips this model.** Knowledge is compiled once, organized into pages, then kept current as new material arrives.

## How it works

1. **Create** - Start a topic and its structure with [`init-topic`](.claude/skills/init-topic/SKILL.md).
2. **Build** - Add raw sources, then turn them into wiki pages with [`ingest`](.claude/skills/ingest/SKILL.md).
3. **Use** - Browse the wiki anywhere you read Markdown, and ask questions against it with [`query`](.claude/skills/query/SKILL.md).
4. **Maintain** - Periodically health-check and tighten the wiki with [`lint`](.claude/skills/lint/SKILL.md).

The result is a **persistent, compounding artifact** rather than a stream of one-off answers.

Those four skills are already included in this repo, so each phase has an explicit operating surface: create with `init-topic`, build with `ingest`, maintain with `lint`, and use with `query`.

![LLM Wiki architecture diagram by Yuchen Jin](docs/yuchen-jin-diagram.png)

## See it in practice

This repo includes three fully worked example topics:

- [`topics/amsterdam/`](topics/amsterdam/) — a travel wiki organized around places, themes, practical advice, and hotels
- [`topics/berkshire-hathaway/`](topics/berkshire-hathaway/) — Buffett shareholder letters distilled into principles, case studies, financial pages, and open questions
- [`topics/rheinmetall/`](topics/rheinmetall/) — a company-analysis wiki covering entities, concepts, financials, comparisons, and research questions

They are useful both as demonstrations of the pattern and as templates for new topics.

## Use cases

This approach works anywhere knowledge accumulates over time and benefits from structure:

- **Research deep-dives** — ingest papers, articles, and notes over weeks or months, then maintain an evolving knowledge base instead of a pile of highlights.
- **Engineering initiative knowledge bases** — connect PRDs, architecture notes, investigations, and decisions so coding agents can work from structured context instead of scattered docs.
- **Competitive intelligence** — track competitors, launches, and market shifts across recurring snapshots and synthesize trends over time.
- **Corporate analysis / due diligence** — turn annual reports, earnings transcripts, and filings into linked pages for financials, entities, claims, and contradictions.
- **Investor research** — process shareholder letters, filings, and analyst material into durable principles, case studies, and timelines.
- **Trip planning** — clip guides, hotel pages, attraction sites, and local references in multiple languages, then organize them into a coherent trip wiki.
- **Learning and reading notes** — build pages for concepts, characters, themes, and questions as you work through books or courses.

### Extensions mentioned by Lex Fridman

In a reply to [Karpathy's original tweet](docs/karpathy-tweet.md), Fridman described a similar setup and highlighted two useful extensions:

1. **Dynamic HTML outputs** for sorting, filtering, and interactive visual exploration.
2. **Temporary focused mini knowledge bases** for voice-mode learning, such as long-form question-and-answer sessions while walking or running.

For broader context and more examples, see [`docs/pattern-overview.md`](docs/pattern-overview.md).

---

## Why this instead of the usual alternatives?

| Approach                            | How it works                                                   | Main limitation                                                                       |
| ----------------------------------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| **ChatGPT / Claude file uploads**   | Upload files and ask questions. The model searches each time.  | No accumulation; the model rediscovers context on every query.                        |
| **NotebookLM**                      | Hosted document-grounded summaries and Q&A.                    | Useful, but constrained to one product surface and workflow.                          |
| **Traditional RAG**                 | Embeddings, vector search, retrieval, then answer generation.  | Strong for retrieval, but still centered on per-query search rather than maintenance. |
| **Manual wiki / Notion / Obsidian** | You organize and update everything yourself.                   | The bookkeeping cost grows quickly and most human-maintained wikis decay.             |
| **LLM Wiki**                        | The LLM maintains the wiki while the human curates and thinks. | Requires setup and editorial judgment, but knowledge compounds instead of resetting.  |

> "The tedious part of maintaining a knowledge base is not the reading or the thinking — it's the bookkeeping. Updating cross-references, keeping summaries current, noting when new data contradicts old claims, maintaining consistency across dozens of pages. Humans abandon wikis because the maintenance burden grows faster than the value. LLMs don't get bored, don't forget to update a cross-reference, and can touch 15 files in one pass."
>
> — Karpathy, "Why this works"

The split of labor is simple: **you curate sources, direct the analysis, and decide what matters; the LLM handles the bookkeeping.**

### Alongside enterprise search

Tools like Glean are essentially RAG over company knowledge bases such as Confluence: useful for fast lookup, but stateless at query time. LLM Wiki adds a different layer.

| Aspect              | Glean                                | Personal LLM Wiki                                     |
| ------------------- | ------------------------------------ | ----------------------------------------------------- |
| **Scope**           | Broad company corpus                 | Your curated working set                              |
| **Knowledge type**  | Retrieval                            | Synthesis and maintained structure                    |
| **Personalization** | Shared search surface                | Deeply personal framing, priorities, and questions    |
| **Cross-domain**    | Usually limited to connected systems | Can combine internal documents with external research |
| **State**           | Stateless queries                    | Stateful, compounding knowledge                       |

Practical workflow: use enterprise search to find important material, then save the pieces worth keeping into your wiki's `raw/` folder so they become part of a maintained body of knowledge.

If you want to connect the two systems more directly, Glean also has a [Remote MCP Server](https://developers.glean.com/guides/mcp), and Glean's [MCP use cases post](https://www.glean.com/blog/mcp-servers-septdrop-2025) gives concrete examples of how that bridge can be used in practice.

## Getting started

### Prerequisites

- An LLM agent with filesystem access, such as VS Code + GitHub Copilot, Claude Code, or Cursor
- Git for version history
- [Obsidian](https://obsidian.md/) for browsing, if you want graph view and a better wiki-reading experience

No runtime, build step, or package manager is required for this repository itself.

> "Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase." — Andrej Karpathy

### First steps

1. Open this repository in your LLM agent environment.
2. Optionally open the repo root as an [Obsidian](https://obsidian.md/) vault.
3. Tell the agent: _"Create a new topic called `my-topic` about `<what you want to research>`"_. That runs the [`init-topic`](.claude/skills/init-topic/SKILL.md) skill.
4. Let the agent create the topic structure and `TOPIC.md` configuration.
5. Drop source files into `topics/my-topic/raw/`.
6. Tell the agent: _"Ingest"_. That runs the [`ingest`](.claude/skills/ingest/SKILL.md) skill, which finds unprocessed sources, creates summaries, and updates the wiki.
7. Ask questions such as _"What do we know about X?"_ or _"Compare X and Y"_. The agent answers from the wiki and can file new synthesis pages when useful.
8. Repeat as you add new sources. The wiki improves cumulatively.

There is no app to build or run. The core workflow is source curation, ingest, maintenance, and query.

## Structure

Every topic follows the same high-level skeleton, but both the raw-source layout and the wiki layout are domain-specific:

```text
topics/<slug>/
├── TOPIC.md                 # Purpose, layout, conventions, key questions
├── raw/                     # Human-curated source material
│   └── ...                  # Flat by default; subfolders when TOPIC.md declares them
├── originals/               # Optional binary originals of converted sources; never ingested
└── wiki/                    # LLM-generated pages
    ├── index.md             # Master catalog
    ├── log.md               # Activity timeline
    ├── overview.md          # Optional front page
    ├── sources/             # One summary per source
    └── <layout folders>/    # Domain-specific page folders
```

The [`init-topic`](.claude/skills/init-topic/SKILL.md) skill helps choose both layouts for the domain.

**Raw layout** defaults to a flat `raw/` folder, but topics can define subfolders in `TOPIC.md` when source types differ substantially.

**Wiki layout** defaults to `concepts/`, `entities/`, `syntheses/`, and `questions/`, but topics can override that with a domain-fitted layout. In this repo:

| Topic                | Layout                                                                   | Why it fits                                                               |
| -------------------- | ------------------------------------------------------------------------ | ------------------------------------------------------------------------- |
| `amsterdam`          | `places/`, `themes/`, `practical/`, `hotels/`                            | Travel questions cluster naturally around destinations and logistics      |
| `berkshire-hathaway` | `principles/`, `entities/`, `case-studies/`, `financials/`, `questions/` | The material is best understood through investing principles and examples |
| `rheinmetall`        | `entities/`, `concepts/`, `financials/`, `comparisons/`, `questions/`    | Company analysis benefits from dedicated financial and comparison pages   |

## Core operations

Four operations cover the lifecycle: **create → build → maintain → use**.

| Command        | Skill                                              | What it does                                                                                          |
| -------------- | -------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| **Init Topic** | [`init-topic`](.claude/skills/init-topic/SKILL.md) | Creates a new topic, helps choose layouts, and writes the initial structure and configuration         |
| **Ingest**     | [`ingest`](.claude/skills/ingest/SKILL.md)         | Processes raw sources into summaries and wiki pages, then updates `index.md` and `log.md`             |
| **Lint**       | [`lint`](.claude/skills/lint/SKILL.md)             | Audits the wiki for issues such as broken links, contradictions, stale pages, or unprocessed sources  |
| **Query**      | [`query`](.claude/skills/query/SKILL.md)           | Searches the wiki, synthesizes an answer, and can optionally file that answer as a new synthesis page |

Each skill has a detailed workflow in `.claude/skills/`.

## Schema and instructions

The repo-level instructions live in:

- `AGENTS.md` — the primary schema for wiki structure, page types, conventions, and quality rules
- `CLAUDE.md` — a shorter Claude-oriented entry point that points back to `AGENTS.md`
- `.claude/skills/` — operational workflows for topic creation, ingest, lint, and query

## Obsidian setup

[Obsidian](https://obsidian.md/) is the recommended way to browse the wiki because it renders links well, exposes graph view and backlinks, and updates immediately as the LLM edits Markdown files.

### Opening the vault

1. Install [Obsidian](https://obsidian.md/).
2. Choose **Open folder as vault**.
3. Select the repository root.

Use a single vault at the repo root so you can browse all topics, schema files, and supporting docs together.

### Recommended plugins

- **Graph view** (built-in) — good for visualizing relationships; exclude `raw/` if the graph gets noisy
- **Dataview** — useful for querying frontmatter such as tags, dates, or source counts
- **Marp** — useful if you want to turn wiki content into slide decks

### Search

At small scale, `index.md` is often enough. As the wiki grows, local search becomes more important. Karpathy specifically mentions [`qmd`](https://github.com/tobi/qmd), a local search tool that combines full-text search, vector search, and reranking.

`qmd` is optional and separate from this repo. If you use it, treat it as an add-on for search, not as a dependency of the wiki itself.

### Obsidian Web Clipper

[Obsidian Web Clipper](https://obsidian.md/clipper) is the fastest way to get web material into `raw/`.

1. Install the browser extension.
2. Configure it to save into `topics/<your-topic>/raw/` or the appropriate subfolder for that topic.
3. Clip articles, docs, or threads into the vault.
4. Ask the agent to ingest them.

### Settings tips

- Set a fixed attachment folder so downloaded images land next to the topic's source material in a predictable place.
- Bind a hotkey for downloading attachments so diagrams and embedded images are stored locally when useful.

## Further reading

- [docs/hackathon-brief.md](docs/hackathon-brief.md) — original hackathon framing, motivation, and architecture overview
- [docs/karpathy-gist.md](docs/karpathy-gist.md) — local copy of Karpathy's full gist
- [docs/pattern-overview.md](docs/pattern-overview.md) — deeper context on the pattern, alternatives, use cases, and future directions
- [docs/publishing-options.md](docs/publishing-options.md) — ways to publish the wiki as a website
- [presentation.md](presentation.md) — hackathon presentation sketch

## References

- [Karpathy's original gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
- [Original tweet](https://x.com/karpathy/status/2039805659525644595) (April 2, 2026)
- [Follow-up tweet with gist](https://x.com/karpathy/status/2040470801506541998) (April 4, 2026)
- [Yuchen Jin's architecture diagram](https://x.com/Yuchenj_UW/status/2040482771576197377) (April 4, 2026)
