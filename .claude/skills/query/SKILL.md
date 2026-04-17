---
name: query
description: Answers questions by searching the wiki, synthesizing from relevant pages, and optionally filing the answer as a synthesis page. Use when the user asks a knowledge question about any topic in the wiki. Triggers on phrases like "what do we know about", "compare", "summarize", "explain", "how does X relate to Y".
---

# Query Wiki

**Purpose:** Answer questions by searching the wiki's compiled knowledge, synthesizing from relevant pages with citations, and optionally filing substantial answers back into the wiki.

## When to Apply

- User asks a question about any topic domain covered by the wiki
- User asks to compare, summarize, or explain something
- User asks "what do we know about X?"

---

## Workflow Overview

1. **Search** - Find relevant pages via the index
2. **Read** - Read identified pages in depth
3. **Synthesize** - Compose an answer with citations
4. **File** - Offer to save substantial answers as synthesis pages

---

## Phase 1: Search the Index

Read `wiki/index.md` to identify relevant pages. Look for:

- **Direct matches** - The question mentions a concept or entity that has a page
- **Indirect matches** - The question relates to themes covered by existing pages
- **Source summaries** that might contain relevant claims

---

## Phase 2: Read Relevant Pages

Read the identified pages. For complex questions, follow cross-references to additional pages.

**Depth judgment:**

- Simple question ("What is X?") - One or two pages may suffice.
- Synthesis question ("How does X relate to Y?") - Read all pages touching both topics.
- Broad overview - Skim many pages, read key ones deeply.

---

## Phase 3: Synthesize an Answer

- Answer with **inline citations** to wiki pages using footnotes.
- If the wiki doesn't have enough information: **say so explicitly.** Suggest what sources might fill the gap.
- If the wiki has **contradictory information**: present both sides with their sources. Don't silently pick one.

**Match the answer format to the question:**

| Question Type | Format                                   |
| ------------- | ---------------------------------------- |
| Factual       | Direct answer with citations             |
| Comparison    | Table or structured comparison           |
| Overview      | Narrative synthesis with section headers |
| List          | Enumerated items with brief explanations |

---

## Phase 4: Offer to File

If the answer is substantial and reusable (not a simple fact lookup), offer to save it:

> "This comparison might be useful to reference later. Want me to file it as a synthesis page?"

If yes:

1. Create `wiki/syntheses/<slug>.md` with proper frontmatter (see Synthesis page type in `AGENTS.md`).
2. Add it to `wiki/index.md`.
3. Log the query and filing in `wiki/log.md`.

---

## Edge Cases

- **Wiki has no information:** Say "the wiki doesn't have information on this yet" rather than answering from general knowledge. The wiki is the source of truth.
- **General knowledge needed:** If you must supplement, label it clearly: "The wiki doesn't cover this, but generally speaking..."
- **Stale pages:** If relevant pages haven't been updated recently and the topic is fast-moving, mention this: "Note: the relevant pages were last updated on [date]."
- **Cross-topic queries:** If the question spans multiple topics, check indexes from both topics. Mention which topic each piece of information comes from.
