# LLM Wiki - Hackathon Brief

**Date:** 2026-04-14
**Source:** Andrej Karpathy (April 2, 2026) - [original tweet](https://x.com/karpathy/status/2039805659525644595), [idea file (gist)](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
**Purpose:** Hackathon framing - motivation, MVP recipe, and applied use cases.

> **Note:** This document is the original pre-hackathon brief that framed the project. The conceptual foundations (what LLM Wiki is, the problem it solves, how it compares to alternatives, community insights, future directions) live in [`pattern-overview.md`](pattern-overview.md). This brief focuses on what is specific to the hackathon: the elevator pitch, the MVP recipe, why it's a good hackathon project, and ideas for applying the pattern in our environment.

---

## What is it? (The 30-second version)

Instead of asking an AI to search through your documents every time you have a question, you have the AI **build and maintain a personal wiki** from those documents. The AI reads your stuff, writes organized summaries, connects related ideas, and keeps everything up to date. You browse the wiki like Wikipedia - except it's _your_ knowledge, organized by _your_ AI.

Think of it this way: **RAG is a search engine. LLM Wiki is a librarian who has read everything and keeps the library organized.**

For the full problem statement, comparisons with alternatives, use cases, and community insights, see [`pattern-overview.md`](pattern-overview.md).

---

## How to Implement It (Hackathon Game Plan)

### Minimum Viable Version (Day 1 goal)

**Tools needed:**

- An LLM agent with file system access (Claude Code, OpenAI Codex, Cursor, VS Code + Copilot, etc.)
- A markdown viewer (Obsidian is ideal, but VS Code works too)
- A folder for your project

**Directory structure:**

```
my-wiki/
├── raw/                 # Source documents (immutable)
│   ├── articles/
│   ├── papers/
│   └── notes/
├── originals/           # Binary originals of converted sources; never ingested
├── wiki/                # LLM-generated (don't edit manually)
│   ├── index.md         # Master catalog
│   ├── log.md           # Activity timeline
│   ├── concepts/        # Concept pages
│   ├── entities/        # Entity pages (people, orgs, tools)
│   └── sources/         # Source summaries
└── TOPIC.md             # Topic-specific domain, scope, and instructions for the LLM
```

> In this repo the same layout lives under `topics/<slug>/` so multiple topics can coexist. See [`../README.md`](../README.md) and [`../AGENTS.md`](../AGENTS.md) for the canonical multi-topic structure used here.

**Steps:**

1. **Pick a topic** (something with 10-20 available sources). Ideas: a technology area, a competitive landscape, a poorly documented internal knowledge area, a research interest.

2. **Write the topic file** (`TOPIC.md`) - domain, scope, categorization axes, and any topic-specific conventions beyond the global schema in `AGENTS.md`.

3. **Collect 5-10 raw sources** - articles, docs, papers. Use Obsidian Web Clipper or save as markdown.

4. **Ingest one source at a time** - "Process `raw/articles/source1.md`. Read it, create a summary in `wiki/sources/`, update the index, and create/update any relevant concept or entity pages."

5. **Ask questions** - once 5+ sources are ingested: "What are the key themes?", "Compare X and Y", "What contradictions exist?"

6. **Run a lint pass** - health-check for inconsistencies, missing links, gaps.

### Day 2 stretch goals

- **Search** - Integrate [qmd](https://github.com/tobi/qmd) for hybrid BM25/vector search over the wiki
- **Visual outputs** - Charts (matplotlib), slide decks (Marp), interactive HTML
- **Automated ingest** - Script that watches `raw/` and triggers ingest
- **Multi-agent setup** - Specialized agents with different permissions (see below)
- **Obsidian plugins** - Graph view for connections, Dataview for dynamic frontmatter queries

**Multi-agent architecture** (from n7-ved's experience in the gist comments):

| Agent          | Role                                                       | Permissions                |
| -------------- | ---------------------------------------------------------- | -------------------------- |
| **Writer**     | Ingests sources, writes pages, updates index               | Read raw + read/write wiki |
| **Maintainer** | Cross-references, consistency, structural integrity        | Read/write wiki + shell    |
| **Auditor**    | Read-only lint passes, finds contradictions, suggests gaps | Read-only wiki             |

Each agent gets its own instructions file. Enforcement via PreToolUse hooks at the agent boundary. In practice: Claude Code uses separate agent modes, VS Code uses `.agent.md` files, or just swap system prompts.

Simpler hackathon alternative: one agent with different "hats" - tell it to "switch to auditor mode" and review without making changes.

### Tips from the community

- **Frontmatter** - YAML frontmatter on wiki pages (tags, dates, source counts, status) enables programmatic queries and consistency checks
- **Entity registry** - A JSON file tracking canonical names and aliases prevents duplicate pages with slightly different titles (SonicBotMan's wiki-kb)
- **Claim types** - Label claims as `Source` (verbatim), `Analysis` (inference), `Unverified` (no authoritative source), or `Gap` (explicitly missing) to prevent paraphrasing bias (n7-ved)
- **Git is free versioning** - The wiki is just a git repo. Version history, branching, collaboration built-in
- **File answers back** - Good analyses and comparisons should become wiki pages. Your explorations compound

---

## What Makes This a Good Hackathon Project

1. **Low infrastructure** - No databases, no servers, no embeddings pipeline. Just folders, markdown files, and an LLM agent.
2. **Impressive in demo** - "I fed it 20 articles and it built this entire interconnected wiki automatically, and now I can ask it anything" is compelling.
3. **Immediately useful** - The wiki you build during the hackathon is actually useful afterward.
4. **Many extension paths** - Different team members can explore different enhancements (search, visualization, multi-agent, Obsidian integration).
5. **Applicable to any domain** - Pick a topic relevant to your company and the output doubles as actual deliverable.
6. **Conceptually novel** - Most people still think of LLMs as chatbots. The "knowledge compiler" framing is a genuine mental model shift.

---

## Appendix: Project-Specific Application Ideas

_The following sections explore how the LLM Wiki pattern could be applied to our workplace and project context. They are not sourced from Karpathy's gist or community - included here for our own hackathon brainstorming._

### Alongside Enterprise Search (DiligentGPT / Glean)

DiligentGPT (Glean-based) is essentially RAG over Confluence - stateless search at query time. LLM Wiki adds a different layer:

| Aspect              | DiligentGPT / Glean                  | Personal LLM Wiki                                                 |
| ------------------- | ------------------------------------ | ----------------------------------------------------------------- |
| **Scope**           | All of Confluence (company-wide)     | Your curated selection                                            |
| **Knowledge type**  | Retrieval (find existing docs)       | Synthesis (compiled, cross-referenced, with your interpretations) |
| **Personalization** | None (same for everyone)             | Deeply personal (your framing, priorities, questions)             |
| **Cross-domain**    | Stays within Confluence              | Can combine internal docs with external sources                   |
| **State**           | Stateless (every query starts fresh) | Stateful (knowledge compounds, contradictions tracked)            |

**Practical workflow:** Use DiligentGPT for quick Confluence lookups. When you find something important, save a cleaned-up extract to your wiki's `raw/`. The wiki compiles the most important knowledge from many sources, including Confluence.

Note: There's an official [Glean MCP server](https://github.com/gleanwork/mcp-server) that could bridge the two systems - the LLM agent could search Confluence via Glean and ingest results into the wiki.

### Competitive Intelligence (Generic Pattern)

Most competitive research today produces point-in-time snapshots: run the analysis, fetch sources, generate a static report, archive it, and start from scratch next quarter. LLM Wiki upgrades this into a living, evolving knowledge base.

**Current approach:** Run competitive research -> agent fetches sites -> generates a static report -> next time starts from scratch.

**LLM Wiki approach:**

```
raw/
├── competitor-captures/   # Periodic website/product snapshots
├── social-media/          # Announcements, posts, threads
├── analytics-reports/     # Market data, SEO, traffic, pricing
└── market-research/       # Articles, analyst reports, news

wiki/
├── competitors/
│   └── <competitor>.md    # Living profile (features, strengths, changes over time)
├── features/
│   └── <feature>-comparison.md   # Cross-competitor feature analysis
├── trends/
│   └── <period>-summary.md
├── index.md
└── log.md
```

Each new data point enriches existing pages rather than generating a new standalone report. Trends emerge naturally ("3 competitors shipped feature X in Q1") and regressions are caught ("competitor Y removed feature Z this month").

**Why it's a good hackathon demo:** any existing competitive research reports and tooling become ready-made raw sources for the wiki. You can demo the "before" (static report, thrown away) vs "after" (living wiki, compounding value) contrast with minimal setup.

---

## Key Quotes

> "Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase." - Karpathy

> "Stop using LLMs as search engines over your docs. Use them as tireless knowledge engineers who compile, cross-reference, and maintain a living wiki. Humans curate and think." - Yuchen Jin

---

## Resources

- [Karpathy's original tweet](https://x.com/karpathy/status/2039805659525644595) (April 2, 2026)
- [Karpathy's "idea file" gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
- [Yuchen Jin's diagram](https://x.com/Yuchenj_UW/status/2040482771576197377)
- [qmd - local markdown search engine](https://github.com/tobi/qmd)
- [Obsidian Web Clipper](https://obsidian.md/clipper)
- Local companion docs: [`pattern-overview.md`](pattern-overview.md), [`karpathy-gist.md`](karpathy-gist.md), [`karpathy-tweet.md`](karpathy-tweet.md)
