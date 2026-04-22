# LLM Wiki - Pattern Overview

A deeper look at the LLM Wiki pattern: the problem it solves, how it compares to alternatives, use cases, practical tips, and ideas for evolution.

This is a curated companion to Karpathy's original idea file (see [karpathy-gist.md](karpathy-gist.md)). It draws from the [original gist's](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) community discussion (3,800+ forks, 450+ comments) and our own exploration.

## Sources & References

- **Original gist:** [karpathy/llm-wiki.md](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) - Andrej Karpathy's GitHub Gist (local copy: [karpathy-gist.md](karpathy-gist.md))
- **Original tweet:** [Andrej Karpathy's tweet](https://x.com/karpathy/status/2039805659525644595) (April 2, 2026) - Where the LLM Wiki idea was first shared publicly (local copy: [karpathy-tweet.md](karpathy-tweet.md))
- **Follow-up tweet with gist link:** [Karpathy's follow-up](https://x.com/karpathy/status/2040470801506541998) (April 4, 2026) - Sharing the gist, with notes on "idea files" as a concept
- **Diagram by Yuchen Jin:** [Yuchen Jin's visual overview](https://x.com/Yuchenj_UW/status/2040482771576197377) (April 4, 2026) - Diagram summarizing the LLM Wiki architecture (3 layers), operations (ingest, query, lint), and actor roles (local copy: [yuchen-jin-diagram.png](yuchen-jin-diagram.png))

---

## The Problem

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

Think of it this way: **RAG is a search engine. LLM Wiki is a librarian who has read everything and keeps the library organized.**

## How It Compares to Alternatives

| Approach                            | How it works                                                   | Weakness                                                                            |
| ----------------------------------- | -------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| **ChatGPT / Claude file uploads**   | Upload files, ask questions. AI searches chunks each time.     | No accumulation. Rediscovers knowledge from scratch every query.                    |
| **NotebookLM**                      | Google's approach. Uploads sources, generates summaries.       | Limited to Google's interface. Can't extend, customize, or integrate.               |
| **Traditional RAG**                 | Embeddings + vector DB + retrieval pipeline.                   | Complex infrastructure. Still retrieves per-query, doesn't synthesize persistently. |
| **Manual wiki / Notion / Obsidian** | You write and maintain everything yourself.                    | Maintenance burden grows faster than the value. Humans abandon wikis.               |
| **LLM Wiki**                        | LLM builds & maintains the wiki. You curate sources and think. | Requires some setup. But: knowledge compounds, maintenance cost is near-zero.       |

> "The tedious part of maintaining a knowledge base is not the reading or the thinking - it's the bookkeeping. Updating cross-references, keeping summaries current, noting when new data contradicts old claims, maintaining consistency across dozens of pages. Humans abandon wikis because the maintenance burden grows faster than the value. LLMs don't get bored, don't forget to update a cross-reference, and can touch 15 files in one pass."
>
> -- Karpathy

**Why plain files over Notion?** Everything is plain `.md` files on your filesystem, inside a git repo. Git gives you version history and diffing for free. Obsidian opens the same folder as a "vault" and provides graph view, backlinks, and a nice reading experience - it's a viewer, not a storage layer. Notion is a poor fit: proprietary cloud format, API rate limits for every LLM read/write, no git diffs, no offline access.

## Use Cases

From Karpathy's gist:

- **Research** - Going deep on a topic over weeks/months. Reading papers, articles, reports, and incrementally building a comprehensive wiki with an evolving thesis.
- **Business/team knowledge** - Internal wiki fed by Slack threads, meeting transcripts, project docs, customer calls. Stays current because the LLM does the maintenance nobody wants to do.
- **Competitive analysis** - Tracking competitors, market trends, product launches. The wiki synthesizes and cross-references automatically.
- **Learning / book reading** - Filing each chapter, building pages for characters/themes/concepts. By the end, you have a rich companion wiki (think fan wikis like Tolkien Gateway - built automatically).
- **Personal knowledge management** - Goals, health, self-improvement, journal entries, podcast notes, articles - structured and connected over time.
- **Due diligence / trip planning / course notes / hobby deep-dives** - Anything where you accumulate knowledge over time and want it organized.

### Work documentation & brag list

One of the strongest use cases - the pain point (nobody maintains their work log) is universal.

**Raw sources:** PR descriptions, commit messages, ticket updates, meeting notes, Slack messages about decisions, quick daily notes.

**Wiki pages the LLM maintains:** Weekly summaries, project pages, a skills/growth page (patterns the LLM notices), a **brag list** (accomplishments extracted and quantified for review season), a decision log.

**Daily workflow:** End of day, paste 2-3 sentences into raw sources -> tell LLM "process today's notes" -> LLM files into project pages, updates weekly summary, flags brag-list-worthy items. At review time, the brag list is already written.

### Extensions (from Lex Fridman)

Fridman [replied under Karpathy's original tweet](https://x.com/karpathy/status/2039805659525644595) describing a similar setup and shared two ideas:

1. **Dynamic HTML outputs** - Have the LLM generate interactive HTML with JS for sorting/filtering data and tinkering with visualizations.
2. **Mini knowledge bases for voice mode** - Generate a temporary focused mini-knowledge-base for a topic, load it into an LLM for voice-mode interaction on a long run. An interactive podcast where you ask questions and listen to answers.

## Ingest Strategies

- **One-at-a-time (recommended for starting)** - You stay involved, guide emphasis, catch mistakes early. Best for high-value sources. Karpathy: _"Personally I prefer to ingest sources one at a time and stay involved - I read the summaries, check the updates, and guide the LLM on what to emphasize."_
- **Batch ingest** - "Process all unprocessed files in `raw/articles/`." Less control but faster. Good for lower-stakes sources where the schema gives enough guidance.
- **Batched batch ingest** - Break large batch ingests into smaller focused batches of 3-5 sources to avoid LLM context degradation. Review each batch before proceeding.

**Source collection methods:**

- [Obsidian Web Clipper](https://obsidian.md/clipper) (browser extension, recommended) - 2 clicks to save any web page as markdown
- Copy-paste to `.md` manually
- Tell the LLM to fetch a URL and save clean markdown to `raw/`
- PDFs that are primary sources saved to `raw/papers/` (or `raw/articles/` if a Markdown conversion is the canonical form); binary originals of already-converted sources go to `originals/` at the topic root (non-ingestible — link originals from a source summary via `source_original:`)

## Tooling

At small scale, `index.md` is sufficient for navigation. As the wiki grows, proper search becomes valuable.

**[qmd](https://github.com/tobi/qmd)** - Karpathy's recommended option. A local search engine by Tobi Lutke (Shopify CEO):

- Hybrid search: BM25 full-text + vector semantic search
- LLM re-ranking of results
- CLI (`qmd search "query"`) or MCP server (native LLM tool)
- Fully local, no cloud, no API keys

**MCP connectors** (for LLM integrations without direct filesystem access):

| Tool                                                             | Purpose                                                         |
| ---------------------------------------------------------------- | --------------------------------------------------------------- |
| [obsidian-mcp](https://github.com/StevenStavrakis/obsidian-mcp)  | Full CRUD for Obsidian vaults - search, read, write, organize   |
| [Basic Memory](https://github.com/basicmachines-co/basic-memory) | Local-first knowledge management, semantic graph from .md files |
| [qmd](https://github.com/tobi/qmd)                               | Local search engine with MCP server support                     |

## Community Insights

Notable patterns from the gist's community discussion:

**FUNGI Framework (FBoschman)** - A five-stage critical thinking protocol for processing notes: **F**rame (restate in one sentence), **U**nearth (surface assumptions), **N**etwork (connect to 2+ existing notes), **G**row (generate a new question), **I**nterrogate (strongest counter-argument). Prevents the wiki from becoming an uncritical dump.

**Four-layer architecture + staleness scoring (n7-ved)** - Added an "infrastructure layer" beyond Karpathy's three for design records about the agents, rules, and conventions themselves. Also introduced staleness scoring - each file carries a score based on how far behind its dependencies it is. Update a source, every downstream file's score increases, the auditor surfaces the worst offenders.

**Entity registry for deduplication (SonicBotMan)** - A JSON registry tracking canonical names and aliases. When the LLM tries to create "OpenAI" when "Open AI" already exists, the registry catches it and merges. The #1 practical problem as wikis grow.

**TL;DR dual-version files (gitdexgit)** - Two versions of each wiki page: a detailed `.md` for the LLM (full context, citations) and a human-readable summary. Uses the [caveman](https://github.com/JuliusBrussee/caveman) plugin for compression.

**Context-protected architecture (jurajskuska)** - A 5-layer pipeline protecting the LLM's context window. Key insight: past sessions stored as JSONL files with BM25 search across them. "What did we decide about X?" returns only matching paragraphs (~500 tokens) instead of full conversations (12,000-50,000 tokens).

**Claim types (n7-ved)** - Label claims as `Source` (verbatim), `Analysis` (inference), `Unverified` (no authoritative source), or `Gap` (explicitly missing) to prevent paraphrasing bias.

**Multi-agent architecture (n7-ved)** - Specialized agents: Writer (ingests, writes), Maintainer (cross-references, consistency), Auditor (read-only lint). Each with different permissions and instruction sets.

## Future Directions

From Karpathy:

> "As the repo grows, the natural desire is to also think about synthetic data generation + finetuning to have your LLM 'know' the data in its weights instead of just context windows."

This points toward a progression: raw docs -> compiled wiki -> distilled training data -> fine-tuned model that "knows" your domain natively.

## Why This Pattern Works Well for Individual and Team Use

The setup is minimal: a folder, some markdown files, an LLM agent, and a schema document. No databases, no servers, no embeddings pipeline. Yet the output is substantial: a structured, interlinked knowledge base that grows with every source you add.

For **individuals**, it means your research, reading, and explorations compound instead of evaporating into chat history. The wiki is always there, always current, always cross-referenced.

For **teams**, the low-friction setup means anyone can start a topic wiki without DevOps or infrastructure approval. The wiki is a git repo - collaboration, reviews, and version history come for free. The LLM does the maintenance that no one on the team wants to do, keeping the knowledge base alive where traditional wikis die.

The conceptual shift: **stop using LLMs as search engines over your docs. Use them as tireless knowledge engineers who compile, cross-reference, and maintain a living wiki. You curate and think.**

## Key Quotes

> "Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase." - Karpathy

> "The wiki keeps getting richer with every source you add and every question you ask." - Karpathy

> "The part he [Vannevar Bush] couldn't solve was who does the maintenance. The LLM handles that." - Karpathy
