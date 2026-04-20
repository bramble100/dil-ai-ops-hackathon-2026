# LLM Wiki

A personal knowledge base built and maintained by LLM agents. Based on [Andrej Karpathy's LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

> This repository was created for the **Diligent AI Ops Hackathon 2026**. For the original hackathon framing, motivation, and architecture overview, see [`docs/hackathon-brief.md`](docs/hackathon-brief.md).

Live topics in this repo:

- [`topics/ai-engineering/`](topics/ai-engineering/) - AI from a software engineer's perspective (scaffold)
- [`topics/amsterdam/`](topics/amsterdam/) - Amsterdam & Netherlands travel wiki (fully populated)
- [`topics/rheinmetall/`](topics/rheinmetall/) - Rheinmetall company analysis (fully populated)

![LLM Wiki architecture diagram by Yuchen Jin](docs/yuchen-jin-diagram.png)

## How It Works

1. **You** curate raw sources (articles, papers, notes, images) into `topics/<slug>/raw/`
2. **The LLM** processes sources into a structured, interlinked wiki at `topics/<slug>/wiki/`
3. **You** browse the wiki in Obsidian (or VS Code), ask questions, and direct the analysis
4. **The LLM** keeps everything cross-referenced, consistent, and up to date

The wiki is a **persistent, compounding artifact**. Knowledge is compiled once and kept current - not re-derived on every query.

## Getting Started

### Prerequisites

- An LLM agent with filesystem access (VS Code + GitHub Copilot, Claude Code, Cursor, etc.)
- [Obsidian](https://obsidian.md/) for browsing the wiki (optional but recommended — see [Obsidian Setup](#obsidian-setup))
- Git for version history

> "Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase." - Andrej Karpathy

### First Steps

1. Open this folder in your LLM agent environment
2. Optionally, open this folder as an [Obsidian](https://obsidian.md/) vault for graph-view browsing (see [Obsidian Setup](#obsidian-setup))
3. Tell the agent: _"Create a new topic called `my-topic` about `<what you want to research>`"_ (runs the [`init-topic`](.claude/skills/init-topic/SKILL.md) skill)
4. The agent will help you choose a wiki structure, then create the full directory tree
5. Drop source files into `topics/my-topic/raw/` — the agent will sort them into the right subfolders
6. Tell the agent: _"Ingest"_ (runs the [`ingest`](.claude/skills/ingest/SKILL.md) skill) — it will find and process unprocessed sources
7. Watch the wiki grow

You can also explore the existing topics (`amsterdam`, `rheinmetall`) to see what a populated wiki looks like before creating your own.

## Structure

Every topic follows the same skeleton — `TOPIC.md` + `raw/` + `wiki/` — but the `wiki/` subfolders are domain-specific:

```
topics/<slug>/
├── TOPIC.md                 # Purpose, wiki layout, page conventions, key questions
├── raw/                     # Your sources (immutable once sorted)
│   ├── articles/            # Web articles, blog posts
│   ├── papers/              # Research papers
│   ├── notes/               # Quick thoughts, observations
│   └── assets/              # Images, diagrams, PDFs
└── wiki/                    # LLM-generated (don't edit manually)
    ├── index.md             # Master catalog
    ├── log.md               # Activity timeline
    ├── overview.md          # Topic overview (optional)
    ├── sources/             # One summary per raw source
    └── <layout folders>/    # Domain-specific (see below)
```

The `init-topic` skill helps you choose a **wiki layout** fitted to your domain. The default is `concepts/`, `entities/`, `syntheses/`, `questions/` — but topics can declare a custom layout in `TOPIC.md`:

| Topic            | Layout                                                                | Why                                            |
| ---------------- | --------------------------------------------------------------------- | ---------------------------------------------- |
| `ai-engineering` | `concepts/`, `entities/`, `syntheses/`, `questions/`                  | Default — general knowledge domain             |
| `amsterdam`      | `places/`, `themes/`, `practical/`, `hotels/`                         | Travel wiki — organized by what you'd look up  |
| `rheinmetall`    | `entities/`, `concepts/`, `financials/`, `comparisons/`, `questions/` | Corporate analysis — dedicated financial pages |

## Operations

| Command        | Skill                                              | What happens                                                                                 |
| -------------- | -------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| **Init Topic** | [`init-topic`](.claude/skills/init-topic/SKILL.md) | _"New topic about X"_ — helps choose a wiki layout, creates directory structure + config     |
| **Ingest**     | [`ingest`](.claude/skills/ingest/SKILL.md)         | _"Process this source"_ — reads it, creates summary, updates wiki pages, updates index & log |
| **Query**      | [`query`](.claude/skills/query/SKILL.md)           | _"What do we know about X?"_ — searches wiki, synthesizes answer, optionally files it        |
| **Lint**       | [`lint`](.claude/skills/lint/SKILL.md)             | _"Health check"_ — finds contradictions, orphans, broken links, stale content                |

Each operation has a detailed skill file in `.claude/skills/` with step-by-step workflows, edge cases, and gotchas.

## Schema

The LLM's instructions live in:

- `AGENTS.md` - Wiki structure, page types, conventions, quality standards (VS Code Copilot, primary)
- `CLAUDE.md` - Quick reference (Claude Code, references AGENTS.md)
- `.claude/skills/` - Operational workflows (ingest, query, lint, init-topic) with detailed steps and edge cases

## Obsidian Setup

[Obsidian](https://obsidian.md/) is a free markdown editor that opens a folder of `.md` files as an interconnected "vault". It's the recommended way to browse the wiki because it renders links as clickable navigation, shows a graph view of how pages connect, and updates in real time as the LLM edits files. Karpathy: _"Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase."_

### Opening the vault

1. Download and install [Obsidian](https://obsidian.md/)
2. On the vault chooser screen, click **"Open folder as vault"** and select this repository's root folder (`dil-ai-ops-hackathon-2026/`)
3. That's it. You'll see the full folder tree in the left sidebar

**Use a single vault at the repo root.** This gives you visibility into all topics, the README, and schema files in one place. Obsidian handles subfolders well, and the graph view works across all topics.

### Recommended plugins

- **Graph view** (built-in) - Shows wiki structure and connections. Hover over nodes to see what links where. Exclude `raw/` from the graph if it gets noisy (Settings > Files & Links > Excluded files)
- **Dataview** (community plugin) - Runs queries over page frontmatter. Since all wiki pages have YAML frontmatter (tags, dates, source counts), Dataview can generate dynamic tables and lists. Example: list all concept pages sorted by source count
- **Marp** (community plugin) - Renders markdown-based slide decks. Useful for generating presentations from wiki content

### Obsidian Web Clipper

[Obsidian Web Clipper](https://obsidian.md/clipper) is a browser extension (Chrome, Firefox, Edge, Safari) that converts any web page to a clean markdown file and saves it directly into your vault. It's the fastest way to get sources into `raw/`.

**How to use:**

1. Install the extension from your browser's extension store
2. Configure it to save files to the right folder (e.g., `topics/<your-topic>/raw/articles/`)
3. When you find an article, tweet, or page worth saving: click the extension icon, then "Add to Obsidian" - 2 clicks total
4. The page is saved as a `.md` file in your vault, ready for the agent to ingest

Works well on long-form articles, blog posts, documentation pages, and even Twitter/X threads. The extension extracts clean text content and preserves headings, links, and images.

### Settings tips

- **Attachment folder:** In Settings > Files and links > "Attachment folder path", set it to a fixed directory (e.g., `raw/assets/`) so downloaded images land in a predictable place
- **Download images hotkey:** In Settings > Hotkeys, search for "Download attachments for current file" and bind it to a hotkey (e.g., Ctrl+Shift+D). After clipping an article, hit the hotkey to download all images locally - useful for offline access and for giving the LLM access to diagrams

## Further Reading

- [docs/hackathon-brief.md](docs/hackathon-brief.md) - Original hackathon framing, motivation, and 30-second/deep explanations
- [docs/karpathy-gist.md](docs/karpathy-gist.md) - Karpathy's full idea file with reference links (local copy)
- [docs/pattern-overview.md](docs/pattern-overview.md) - Deeper context: the problem, alternatives comparison, use cases, community insights, future directions
- [presentation.md](presentation.md) - Hackathon presentation sketch

## References

- [Karpathy's original gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
- [Original tweet](https://x.com/karpathy/status/2039805659525644595) (April 2, 2026)
- [Follow-up tweet with gist](https://x.com/karpathy/status/2040470801506541998) (April 4, 2026)
- [Yuchen Jin's architecture diagram](https://x.com/Yuchenj_UW/status/2040482771576197377) (April 4, 2026)
