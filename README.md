# LLM Wiki - Hackathon Brief 

**Date:** 2026-04-14 
**Source:** Andrej Karpathy (April 2, 2026) - [original tweet](https://x.com/karpathy/status/2039805659525644595), [idea file (gist)](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 
**Purpose:** Hackathon exploration & implementation guide 

--- 

## What is it? (The 30-second version) 

Instead of asking an AI to search through your documents every time you have a question, you have the AI **build and maintain a personal wiki** from those documents. The AI reads your stuff, writes organized summaries, connects related ideas, and keeps everything up to date. You browse the wiki like Wikipedia - except it's _your_ knowledge, organized by _your_ AI. 

Think of it this way: **RAG is a search engine. LLM Wiki is a librarian who has read everything and keeps the library organized.** 

--- 

## The Problem It Solves 

Most people use AI with documents like this: 

1. Upload a pile of files (articles, notes, PDFs, etc.) 
2. Ask a question 
3. AI scrambles through the pile, finds relevant chunks, generates an answer 
4. Next question? Start from scratch again. Nothing was learned or built up. 

This is called **RAG** (Retrieval-Augmented Generation). It works, but has fundamental limitations: 

- **No accumulation.** The AI rediscovers knowledge from scratch on every question. 
- **No structure.** Connections between documents aren't tracked. Contradictions aren't flagged. 
- **Context window limits.** As your collection grows, the AI sees less of the whole picture per query. 
- **Knowledge is ephemeral.** Good answers disappear into chat history. Nothing compounds. 

**The LLM Wiki flips this.** Knowledge is compiled once, then kept current - not re-derived on every query. 

--- 

## How It Works 

### Architecture (3 layers) 

``` 
┌──────────────────┐ reads           ┌──────────────────┐ guides        ┌──────────────────┐ 
│ Raw Sources      │ ──────────────→ │ The Wiki         │ ←──────────── │ The Schema       │ 
│ (immutable)      │                 │ (LLM-owned .md)  │               │ (CLAUDE.md etc.) │ 
│                  │                 │                  │               │                  │ 
│ articles, papers │                 │ summaries, entity│               │ conventions,     │ 
│ repos, images,   │                 │ pages, concept   │               │ workflows,       │ 
│ datasets, notes  │                 │ pages, index,    │               │ structure rules  │ 
│                  │                 │ comparisons      │               │                  │ 
└──────────────────┘                 └──────────────────┘               └──────────────────┘ 
 YOU curate LLM writes YOU + LLM co-evolve 
``` 

1. **Raw Sources** - Your curated collection of source documents. Immutable. The LLM reads from them but never modifies them. This is your source of truth. 

2. **The Wiki** - A directory of LLM-generated markdown files. Summaries, entity pages, concept pages, comparisons, an overview. The LLM owns this layer entirely. 

3. **The Schema** - A document (like `CLAUDE.md` or `AGENTS.md`) that tells the LLM how the wiki is structured, what conventions to follow, what workflows to use. This is what makes the LLM a disciplined wiki maintainer rather than a generic chatbot. 

**Storage:** Everything is plain `.md` files on your filesystem, inside a git repo. Git gives you version history and diffing for free. Obsidian opens the same folder as a "vault" and provides graph view, backlinks, and a nice reading experience - it's a viewer, not a storage layer. This is why Notion is a poor fit: proprietary cloud format, API rate limits for every LLM read/write, no git diffs, no offline access. Karpathy: _"I have the LLM agent open on one side and Obsidian open on the other. The LLM makes edits based on our conversation, and I browse the results in real time - following links, checking the graph view, reading the updated pages. Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase."_ 

**Multiple topics:** Use separate wiki directories per topic. Each gets its own `raw/`, `wiki/`, and `SCHEMA.md`. Keeps indexes focused and schemas domain-specific. Exception: closely related topics that benefit from cross-pollination can share a wiki with subdirectories. 

### Core Operations 

| Operation | What happens | Analogy | 
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | 
| **Ingest** | You add a new source. LLM reads it, writes a summary, updates the index, updates relevant entity/concept pages. A single source might touch 10-15 wiki pages. | New book arrives at the library. The librarian reads it, files it, updates the catalog, and adds cross-references. | 
| **Query** | You ask questions. LLM searches relevant wiki pages, reads them, synthesizes an answer with citations. Good answers get filed back into the wiki. | You ask the librarian a question. They pull relevant materials, give you a thorough answer, and if it's a good question, write up a reference sheet. | 
| **Lint** | Periodic health-check. Find contradictions, stale claims, orphan pages, missing cross-references, data gaps. LLM suggests new questions to investigate. | Quarterly audit - checking for outdated books, broken references, missing topics, and inconsistencies. | 

**Ingest strategies:** 

- **One-at-a-time (recommended for starting)** - You stay involved, guide emphasis, catch mistakes early. Best for high-value sources and learning the workflow. 
- **Batch ingest** - "Process all unprocessed files in `raw/articles/`." Less control but much faster. Good for lower-stakes sources where the schema gives enough guidance. 
- **Scripted pipeline** - A script that watches `raw/` for new files and triggers the LLM. Most automated. Best for mature wikis where the schema is well-tuned. 

Karpathy's preference: _"Personally I prefer to ingest sources one at a time and stay involved - I read the summaries, check the updates, and guide the LLM on what to emphasize."_ 

**"Ralph Wiggum technique" for bulk ingest:** _(Our own idea, not from the original gist.)_ Break large batch ingests into smaller focused batches of 3-5 sources to avoid LLM context degradation. Review each batch before proceeding. This prevents the LLM from getting sloppy mid-way through a huge batch where attention degrades. 

**Source collection methods:** 

- **Manual** - [Obsidian Web Clipper](https://obsidian.md/clipper) (browser extension, Karpathy recommends), copy-paste to `.md`, save PDFs/images to `raw/assets/` 
- **Semi-automated** - Tell the LLM to fetch a URL and save clean markdown to `raw/` 
- **Fully automated** - RSS feeds, GitHub webhooks, scrapers on a schedule (overengineering for a hackathon) 

### Navigation 

Two special files keep things navigable: 

- **index.md** - Content-oriented catalog. Every page listed with a link, one-line summary, organized by category. The LLM reads this first when answering queries. Works surprisingly well at moderate scale (~100 sources, ~hundreds of pages). 
- **log.md** - Chronological record. Append-only timeline of ingests, queries, lint passes. Parseable with simple tools. 

### Tooling 

At small scale, `index.md` is sufficient for navigation. As the wiki grows, proper search becomes valuable. [qmd](https://github.com/tobi/qmd) is Karpathy's recommended option (explicitly in the gist) - a local search engine by Tobi Lutke (Shopify CEO) with: 

- **Hybrid search** - BM25 full-text (SQLite FTS5) + vector semantic search (embeddinggemma-300M) 
- **LLM re-ranking** - results re-ranked by a small local model (qwen3-reranker-0.6b) 
- **Two integration modes** - CLI (`qmd search "query"`) or MCP server (native LLM tool) 
- **Fully local** - no cloud, no API keys. Requires Node.js >= 22, ~2GB disk for models 

Other useful Obsidian plugins: **Dataview** (queries over page frontmatter), **Marp** (markdown-based slide decks). Both mentioned in Karpathy's gist. 

**MCP connectors** (for LLM integrations without direct filesystem access): 

| Tool | Purpose | 
| --------------------------------------------------------------------------------- | --------------------------------------------------------------- | 
| [obsidian-mcp](https://github.com/StevenStavrakis/obsidian-mcp) (StevenStavrakis) | Full CRUD for Obsidian vaults - search, read, write, organize | 
| [Construe](https://github.com/mattjoyce/Construe) (mattjoyce) | Intelligent vault context management with frontmatter filtering | 
| [Basic Memory](https://github.com/basicmachines-co/basic-memory) | Local-first knowledge management, semantic graph from .md files | 
| [qmd](https://github.com/tobi/qmd) | Local search engine with MCP server support | 

_For a hackathon, you likely don't need MCP connectors - the LLM already has filesystem access. These become useful when integrating with clients that lack direct file access._ 

--- 

## Why Is This Better Than Alternatives? 

| Approach | How it works | Weakness | 
| ----------------------------------- | -------------------------------------------------------------- | ----------------------------------------------------------------------------------- | 
| **ChatGPT / Claude file uploads** | Upload files, ask questions. AI searches chunks each time. | No accumulation. Rediscovers knowledge from scratch every query. | 
| **NotebookLM** | Google's approach. Uploads sources, generates summaries. | Limited to Google's interface. Can't extend, customize, or integrate. | 
| **Traditional RAG** | Embeddings + vector DB + retrieval pipeline. | Complex infrastructure. Still retrieves per-query, doesn't synthesize persistently. | 
| **Manual wiki / Notion / Obsidian** | You write and maintain everything yourself. | Maintenance burden grows faster than the value. Humans abandon wikis. | 
| **LLM Wiki** | LLM builds & maintains the wiki. You curate sources and think. | Requires some setup. But: knowledge compounds, maintenance cost is near-zero. | 

> "The tedious part of maintaining a knowledge base is not the reading or the thinking - it's the bookkeeping. Updating cross-references, keeping summaries current, noting when new data contradicts old claims, maintaining consistency across dozens of pages. Humans abandon wikis because the maintenance burden grows faster than the value. LLMs don't get bored, don't forget to update a cross-reference, and can touch 15 files in one pass." 
> 
> -- Karpathy (gist, "Why this works" section) 

The human's job: **curate sources, direct the analysis, ask good questions, and think about what it all means.** 
The LLM's job: **everything else.** 

--- 

## Use Cases 

From Karpathy's gist: 

- **Research** - Going deep on a topic over weeks/months. Reading papers, articles, reports, and incrementally building a comprehensive wiki with an evolving thesis. 
- **Business/team knowledge** - Internal wiki fed by Slack threads, meeting transcripts, project docs, customer calls. Stays current because the LLM does the maintenance nobody wants to do. 
- **Competitive analysis** - Tracking competitors, market trends, product launches. The wiki synthesizes and cross-references automatically. 
- **Learning / book reading** - Filing each chapter, building pages for characters/themes/concepts. By the end, you have a rich companion wiki (think fan wikis like Tolkien Gateway - built automatically). 
- **Personal knowledge management** - Goals, health, self-improvement, journal entries, podcast notes, articles - structured and connected over time. 
- **Due diligence / trip planning / course notes / hobby deep-dives** - Anything where you accumulate knowledge over time and want it organized. 

### Lex Fridman's additions 

Fridman [replied under Karpathy's original tweet](https://x.com/karpathy/status/2039805659525644595) (April 3, 2026) describing a similar setup ("A mix of Obsidian, Cursor (for md), and vibe-coded web terminals as front-end") and shared two extensions: 

1. **Dynamic HTML outputs** - "I often have it generate dynamic html (with js) that allows me to sort/filter data and to tinker with visualizations interactively." 
2. **Mini knowledge bases for voice mode** - "I have the system generate a temporary focused mini-knowledge-base for a particular topic that I then load into an LLM for voice-mode interaction on a long 7-10 mile run. So it becomes an interactive podcast while I run, where I ask it questions and listen to the answers to learn more." 

### Extended example: Work documentation & brag list 

One of the strongest use cases - the pain point (nobody maintains their work log) is universal. 

**Raw sources (what you dump in):** PR descriptions, commit messages, ticket updates, meeting notes, Slack messages about decisions, quick daily notes ("Fixed the auth bug, mentored junior dev on testing"). 

**Wiki pages the LLM maintains:** Weekly summaries, project pages (contributions, decisions, impact), a skills/growth page (patterns the LLM notices), a **brag list** (accomplishments extracted and quantified for review season), a decision log, a people/collaboration page. 

**Daily workflow:** End of day, paste 2-3 sentences into raw sources -> tell LLM "process today's notes" -> LLM files into project pages, updates weekly summary, flags brag-list-worthy items. At review time, the brag list is already written. 

**Why it beats a spreadsheet:** The LLM does cross-referencing and pattern-recognition. It notices "you've shipped 4 performance improvements this quarter" - insights that are hard to see day-to-day. 

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
├── raw/ # Source documents (immutable) 
│ ├── articles/ 
│ ├── papers/ 
│ └── assets/ # Downloaded images 
├── wiki/ # LLM-generated (don't edit manually) 
│ ├── index.md # Master catalog 
│ ├── log.md # Activity timeline 
│ ├── concepts/ # Concept pages 
│ ├── entities/ # Entity pages (people, orgs, tools) 
│ └── sources/ # Source summaries 
└── SCHEMA.md # Instructions for the LLM 
``` 

**Steps:** 

1. **Pick a topic** (something with 10-20 available sources). Ideas: a technology area, a competitive landscape, a poorly documented internal knowledge area, a research interest. 

2. **Write the schema file** (`SCHEMA.md`) - directory structure, page conventions, what to create on ingest, index/log maintenance, formatting rules (frontmatter, cross-links). 

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

| Agent | Role | Permissions | 
| -------------- | ---------------------------------------------------------- | -------------------------- | 
| **Writer** | Ingests sources, writes pages, updates index | Read raw + Read/write wiki | 
| **Maintainer** | Cross-references, consistency, structural integrity | Read/write wiki + shell | 
| **Auditor** | Read-only lint passes, finds contradictions, suggests gaps | Read-only wiki | 

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
4. **Many extension paths** - Each team member can explore a different enhancement (search, visualization, multi-agent, Obsidian integration). 
5. **Applicable to any domain** - Pick a topic relevant to your company and the output doubles as actual deliverable. 
6. **Conceptually novel** - Most people still think of LLMs as chatbots. The "knowledge compiler" framing is a genuine mental model shift. 

--- 

## Hackathon Action Plan (3 Engineers, 2 Days) 

### Prep (before the hackathon) 

All three engineers: 

- Install Obsidian + LLM agent of choice (Claude Code, VS Code + Copilot, Cursor, etc.) 
- Agree on topic and each collect 5 raw sources (15 total) 
- One person creates the git repo + directory structure 

### Day 1: Build the Wiki 

| Time | Engineer A (Schema & Ingest) | Engineer B (Sources & Obsidian) | Engineer C (Tooling & Agents) | 
| --------- | ------------------------------------------------------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------------- | 
| **AM 1h** | Write `SCHEMA.md` (page conventions, frontmatter format, ingest workflow) | Format & organize all 15 raw sources in `raw/` | Set up Obsidian vault with plugins (Dataview, Marp, Web Clipper) | 
| **AM 2h** | Ingest first 5 sources one-by-one, refine schema based on quality | Clip 5 more web articles, prepare assets (download images) | Install & configure qmd search, test with first wiki pages | 
| **PM 2h** | Continue ingesting (target: 12-15 total) | Pick up parallel ingests (different sources, same schema) | Build multi-agent configs (Writer, Auditor, Maintainer modes) | 
| **PM 1h** | Run first Q&A queries, file good answers back into wiki | Run lint pass, flag issues | Run auditor pass on the wiki, collect findings | 

**Day 1 target:** 15 sources ingested, wiki with 40-60 pages, searchable, first lint pass complete. 

### Day 2: Polish & Demo 

| Time | Engineer A | Engineer B | Engineer C | 
| --------- | ----------------------------------------------------- | -------------------------------------------- | ----------------------------------------------------- | 
| **AM 1h** | Fix issues from Day 1 lint/audit | Ingest any remaining sources | Build visual outputs (Marp slides, matplotlib charts) | 
| **AM 2h** | Prepare 5 impressive demo queries + answers | Polish Obsidian graph view, take screenshots | Write demo script, rehearse flow | 
| **PM** | **All together:** Final lint, demo rehearsal, present | | | 

**Demo ingredients:** Before/after comparison (raw dump vs structured wiki), live Q&A against the wiki, graph view screenshot, one visual output (chart or slide deck), multi-agent lint demo. 

### Parallelization key insight 

The bottleneck is **schema quality and source collection**, not computation. Engineer A's schema work unblocks everyone else's ingests. Engineer B's source curation feeds Engineer A. Engineer C's tooling is fully independent until integration. By midday Day 1, all three can ingest in parallel using the stabilized schema. 

--- 

## Key Quotes & Framing for the Pitch 

> "Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase." - Karpathy 

> "Stop using LLMs as search engines over your docs. Use them as tireless knowledge engineers who compile, cross-reference, and maintain a living wiki. Humans curate and think." - Yuchen Jin 

> "The wiki keeps getting richer with every source you add and every question you ask." - Karpathy (gist) 

> "The part he [Vannevar Bush] couldn't solve was who does the maintenance. The LLM handles that." - Karpathy (gist) 

--- 

## Resources 

- [Karpathy's original tweet](https://x.com/karpathy/status/2039805659525644595) (19.7M views, 55K bookmarks) 
- [Karpathy's "idea file" gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) (5,000+ stars, 3,800+ forks) 
- [Yuchen Jin's diagram](https://x.com/Yuchenj_UW/status/2040482771576197377) 
- [qmd - local markdown search engine](https://github.com/tobi/qmd) (BM25/vector search + MCP server) 
- [Obsidian Web Clipper](https://obsidian.md/clipper) (browser extension for saving articles as markdown) 

--- 

## Community Insights Worth Noting 

The gist has 3,800+ forks and 450+ comments. Valuable patterns that emerged: 

**1. FUNGI Framework (FBoschman)** 
A five-stage critical thinking protocol for processing notes before they enter the wiki: **F**rame (restate in one sentence), **U**nearth (surface assumptions), **N**etwork (connect to 2+ existing notes), **G**row (generate a new question), **I**nterrogate (strongest counter-argument). Prevents the wiki from becoming an uncritical dump. 

**2. Four-layer architecture + staleness scoring (n7-ved)** 
Added an "infrastructure layer" beyond Karpathy's three for design records about the agents, rules, hooks, and conventions themselves. Also introduced staleness scoring - each file carries a score based on how far behind its outgoing dependencies it is. Update a source, every downstream file's score increases, the auditor surfaces the worst offenders. Mechanical, not LLM-dependent. 

**3. Entity registry for deduplication (SonicBotMan - wiki-kb)** 
A JSON registry tracking canonical names and aliases. When the LLM tries to create "OpenAI" when "Open AI" already exists, the registry catches it and merges. The #1 practical problem as wikis grow. 

**4. TL;DR dual-version files (gitdexgit)** 
Two versions of each wiki page: a detailed `.md` for the LLM (full context, citations, reasoning) and a human-readable summary (less words, more meaning). Uses the [caveman](https://github.com/JuliusBrussee/caveman) plugin for compression. 

**5. Context-protected architecture (jurajskuska - NONO_AIAGENT)** 
A 5-layer pipeline protecting the LLM's context window. Key insight: past sessions stored as JSONL files. Instead of loading full sessions (50-200KB each), use BM25 search across indexed JSONLs. "What did we decide about X?" returns only 3-5 matching paragraphs (~500 tokens) instead of the full conversation (12,000-50,000 tokens). Measured: 64% of data kept out of context, ~31K tokens saved per session. 

--- 

## Future Directions 

From Karpathy's original tweet (not in the gist): 

> "As the repo grows, the natural desire is to also think about synthetic data generation + finetuning to have your LLM 'know' the data in its weights instead of just context windows." 

This points toward a progression: raw docs -> compiled wiki -> distilled training data -> fine-tuned model that "knows" your domain natively. 

--- 

## Appendix: Project-Specific Application Ideas 

_The following sections explore how the LLM Wiki pattern could be applied to our specific workplace and project context. They are not sourced from Karpathy's gist or community - included here for our own hackathon brainstorming._ 

### Alongside Enterprise Search (DiligentGPT / Glean) 

DiligentGPT (Glean-based) is essentially RAG over Confluence - stateless search at query time. LLM Wiki adds a different layer: 

| Aspect | DiligentGPT / Glean | Personal LLM Wiki | 
| ------------------- | ------------------------------------ | ----------------------------------------------------------------- | 
| **Scope** | All of Confluence (company-wide) | Your curated selection | 
| **Knowledge type** | Retrieval (find existing docs) | Synthesis (compiled, cross-referenced, with your interpretations) | 
| **Personalization** | None (same for everyone) | Deeply personal (your framing, priorities, questions) | 
| **Cross-domain** | Stays within Confluence | Can combine internal docs with external sources | 
| **State** | Stateless (every query starts fresh) | Stateful (knowledge compounds, contradictions tracked) | 

**Practical workflow:** Use DiligentGPT for quick Confluence lookups. When you find something important, save a cleaned-up extract to your wiki's `raw/`. The wiki compiles the most important knowledge from many sources, including Confluence. 

Note: There's an official [Glean MCP server](https://github.com/gleanwork/mcp-server) that could bridge the two systems - the LLM agent could search Confluence via Glean and ingest results into the wiki. 

### Competitive Intelligence for GombaCsoda 

A strong fit for the hackathon demo topic. The current competitive research agent skill creates point-in-time snapshots. LLM Wiki would upgrade this to a living, evolving knowledge base. 

**Current approach:** Run competitive research -> agent fetches sites -> generates a static report -> next time starts from scratch. 

**LLM Wiki approach:** 

``` 
raw/ 
├── 2026-03-competitor-captures/ # Periodic website snapshots 
├── social-media/ # Facebook posts, tweets 
├── seo-reports/ # Search Console data 
└── market-research/ # Articles about mushroom apps/sites 

wiki/ 
├── competitors/ 
│ ├── gomba.hu.md # Living profile (features, strengths, changes over time) 
│ └── ... 
├── features/ 
│ └── filtering-comparison.md # Cross-competitor feature analysis 
├── trends/ 
│ └── 2026-q1-summary.md 
├── index.md 
└── log.md 
``` 

Each new data point enriches existing pages rather than generating a new standalone report. Trends emerge naturally: "3 competitors added mushroom edibility quizzes in Q1 2026." 

**Hackathon angle:** Existing competitive research agent and past reports serve as ready-made raw sources. The output is genuinely useful for GombaCsoda strategy. And you can demo the "before" (static report) vs "after" (living wiki) difference. 
