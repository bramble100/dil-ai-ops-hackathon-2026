# LLM Wiki - Claude Code Schema

This project uses a shared schema. All wiki conventions, workflows, and rules are defined in `AGENTS.md`.

**Read `AGENTS.md` fully before any operation.**

## Quick Reference

- **Ingest:** Human adds a source to `topics/<slug>/raw/`, you process it into `wiki/` (see `.claude/skills/ingest/SKILL.md`)
- **Query:** Human asks a question, you read `TOPIC.md` then search `wiki/index.md` and synthesize from relevant pages (see `.claude/skills/query/SKILL.md`)
- **Lint:** Health check for contradictions, orphans, broken links, staleness (see `.claude/skills/lint/SKILL.md`)
- **Init Topic:** Create a new topic with full directory structure (see `.claude/skills/init-topic/SKILL.md`)

## Key Rules

1. Never modify source file content in `raw/` - sources are immutable (sorting into subfolders is fine)
2. Always update `wiki/index.md` and `wiki/log.md` after changes
3. Use standard markdown links: `[Display Text](relative/path.md)` (not Obsidian wikilinks)
4. No links in `log.md`
5. Respect page budgets (see AGENTS.md Growth Management section)
6. Cite sources via inline footnotes
7. Discuss key takeaways with the human before filing during ingest
