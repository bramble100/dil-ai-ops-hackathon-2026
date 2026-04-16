# LLM Wiki

A personal knowledge base built and maintained by LLM agents. Based on [Andrej Karpathy's LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

## How It Works

1. **You** curate raw sources (articles, papers, notes, images) into `topics/<slug>/raw/`
2. **The LLM** processes sources into a structured, interlinked wiki at `topics/<slug>/wiki/`
3. **You** browse the wiki in Obsidian (or VS Code), ask questions, and direct the analysis
4. **The LLM** keeps everything cross-referenced, consistent, and up to date

The wiki is a **persistent, compounding artifact**. Knowledge is compiled once and kept current - not re-derived on every query.

## Getting Started

### Prerequisites

- An LLM agent with filesystem access (VS Code + GitHub Copilot, Claude Code, Cursor, etc.)
- [Obsidian](https://obsidian.md/) for viewing (open this folder as a vault)
- Git for version history

### First Steps

1. Open this folder in your LLM agent environment
2. Open this folder as an Obsidian vault
3. Drop a source file into `topics/ai-engineering/raw/articles/`
4. Tell the agent: "Ingest `topics/ai-engineering/raw/articles/<filename>`"
5. Watch the wiki grow

### Adding a New Topic

```bash
mkdir -p topics/<slug>/{raw/{articles,papers,notes,assets},wiki/{concepts,entities,sources,syntheses,questions}}
```

Then create the initial `wiki/index.md` and `wiki/log.md` (copy from an existing topic).

## Structure

```
topics/
└── ai-engineering/          # Each topic is self-contained
    ├── raw/                 # Your sources (immutable)
    │   ├── articles/        # Web articles, blog posts
    │   ├── papers/          # Research papers
    │   ├── notes/           # Quick thoughts, observations
    │   └── assets/          # Images, diagrams, PDFs
    └── wiki/                # LLM-generated (don't edit manually)
        ├── index.md         # Master catalog
        ├── log.md           # Activity timeline
        ├── concepts/        # Ideas, patterns, techniques
        ├── entities/        # Tools, people, orgs, products
        ├── sources/         # One summary per raw source
        ├── syntheses/       # Analyses, comparisons
        └── questions/       # Open questions, contradictions
```

## Operations

| Command    | What happens                                                                                             |
| ---------- | -------------------------------------------------------------------------------------------------------- |
| **Ingest** | "Process this source" - LLM reads it, creates summary, updates concept/entity pages, updates index & log |
| **Query**  | "What do we know about X?" - LLM searches wiki, synthesizes answer, optionally files it                  |
| **Lint**   | "Health check" - LLM finds contradictions, orphans, broken links, stale content                          |

## Schema

The LLM's instructions live in:

- `AGENTS.md` - Full schema (VS Code Copilot, primary)
- `CLAUDE.md` - Quick reference (Claude Code, references AGENTS.md)

## Obsidian Tips

- **Graph view** shows wiki structure and connections
- **Dataview plugin** can query page frontmatter (all pages have YAML frontmatter)
- **Web Clipper extension** converts web articles to markdown for easy source collection
- Exclude `raw/` from graph view if it gets noisy (Settings > Files & Links > Excluded files)

## References

- [Karpathy's original gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
- [Original tweet](https://x.com/karpathy/status/2039805659525644595) (April 2, 2026)
- [Follow-up tweet with gist](https://x.com/karpathy/status/2040470801506541998) (April 4, 2026)
- [Yuchen Jin's architecture diagram](https://x.com/Yuchenj_UW/status/2040482771576197377) (April 4, 2026)
