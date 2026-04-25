# Publishing a Wiki as a Website

Options for turning an LLM Wiki (a folder of markdown files) into a publicly accessible website. Evaluated with the Berkshire Hathaway wiki as the reference case: ~88 interlinked pages, YAML frontmatter, `wiki/` subfolder structure, standard markdown links.

---

## The Two-Layer Choice

Publishing involves two separate decisions:

1. **Site generator** — what converts your markdown into a website (handles navigation, search, styling, SEO)
2. **Hosting** — where the generated site is served from

Obsidian Publish bundles both. The free/open options separate them: you pick a generator (Docusaurus, Starlight, Jekyll) and a host (GitHub Pages, Vercel, Netlify). The hosting choice barely matters — all free tiers are generous and well-managed for static sites.

---

## Options

### 1. Obsidian Publish

**What it is:** A managed publishing service built into Obsidian. Select pages from your vault, click Publish. The site is hosted on Obsidian's servers at `publish.obsidian.md/<your-site>` or a custom domain.

**Cost:** $8/month ($96/year), billed annually. One site per subscription.

**Setup time:** ~5 minutes. No code, no config, no build step.

**Wiki-specific strengths:**

- Reads your vault directly — no format conversion
- Renders the **graph view** on the web, showing the full network of interlinked pages
- **Backlinks** and **hover previews** for internal links — readers can explore connections the way you built them
- Understands both standard markdown links and Obsidian wikilinks
- YAML frontmatter is used natively for metadata and tags

**Quality:** Clean, modern look. Dark/light mode. 100% Lighthouse accessibility score. SEO, social cards, and sitemap out of the box. Mobile-first. Custom domain with HTTPS included.

**Limitations:** Limited visual customization (you can inject custom CSS but can't replace the layout). No self-hosting option. The look is recognizably "Obsidian Publish."

**Example:** [publish.obsidian.md/discretecs](https://publish.obsidian.md/discretecs)

---

### 2. Starlight (Astro)

**What it is:** A documentation site generator built on Astro. Designed for structured content — auto-generates sidebar from file structure, built-in search, frontmatter-driven.

**Cost:** Free. Deploy to Vercel, Netlify, or GitHub Pages (all free for public sites).

**Setup time:** ~20-30 minutes. `npm create astro -- --template starlight`, copy your markdown, configure `astro.config.mjs` with your sidebar structure.

**Wiki-specific strengths:**

- Frontmatter-driven — your existing `title`, `type`, `tags` fields work out of the box
- Auto-generates sidebar from directory structure, which maps well to `wiki/concepts/`, `wiki/entities/`, etc.
- Fast, modern, great defaults. Better out-of-box aesthetics than Docusaurus
- Full-text search included (Pagefind, runs locally — no Algolia account needed)

**Limitations:** No graph view or backlinks. Sidebar needs to be structured to match your wiki layout. Obsidian-style cross-links (standard markdown links) work fine; wikilinks don't render without a plugin. Needs a build step and CI/CD for republishing.

**Hosting options:**

- **GitHub Pages** — free, stays in the GitHub ecosystem, deploys via GitHub Actions. Add a workflow file and every `git push` rebuilds and republishes. Custom domain with HTTPS. Slightly more setup than Vercel.
- **Vercel** — free, zero-config deployment from GitHub. Fastest to get running. Custom domain with HTTPS.
- **Netlify** — free, same as Vercel. Either is fine; choose based on preference.

---

### 3. Docusaurus

**What it is:** A documentation site generator from Meta, built on React. Mature, widely used, highly customizable.

**Cost:** Free. Same hosting options as Starlight.

**Setup time:** ~30-45 minutes. More configuration than Starlight — sidebar is defined explicitly in `sidebars.js`, not auto-generated from file structure.

**Wiki-specific strengths:**

- React-based — if you're comfortable with React/TypeScript, you can customize anything
- Excellent ecosystem (plugins, themes, community)
- Full-text search via Algolia DocSearch (free for public sites) or local search plugin

**Limitations:** Heavier than Starlight. Sidebar config is more work. No graph view or backlinks. More opinionated about directory structure. If you're not planning to customize heavily, Starlight is a better starting point.

---

### 4. GitHub Pages with Jekyll

**What it is:** GitHub's built-in static site generator. Push markdown to a repo, enable Pages, Jekyll renders it automatically — no Actions workflow needed.

**Cost:** Free. Requires public repo (or GitHub Pro for private).

**Setup time:** ~10-15 minutes for basic setup. Add a `_config.yml`, enable Pages in repo settings, done.

**Wiki-specific strengths:**

- Minimal setup — if your repo is already on GitHub, this is the lowest-friction free option
- No build toolchain to manage locally

**Limitations:** Jekyll's default themes are dated. No built-in search. No sidebar navigation that maps well to a multi-folder wiki structure. Obsidian-style internal links don't render as clickable links in Jekyll by default. Frontmatter is used by Jekyll but only `title` and `date` are treated specially — your custom fields (`type`, `source_count`, etc.) are ignored. For a wiki with 88 pages across multiple subfolders, Jekyll alone produces a poor reading experience without significant theme customization.

**Best use:** As a hosting layer for Starlight or Docusaurus (via GitHub Actions), not as a standalone generator for this use case.

---

## Comparison Table

|                               | Obsidian Publish      | Starlight (Astro)                  | Docusaurus                      | Jekyll (GitHub Pages)      |
| ----------------------------- | --------------------- | ---------------------------------- | ------------------------------- | -------------------------- |
| **Cost**                      | $8/month              | Free                               | Free                            | Free                       |
| **Hosting**                   | Obsidian's servers    | Vercel / Netlify / GitHub Pages    | Vercel / Netlify / GitHub Pages | GitHub Pages (built-in)    |
| **Setup time**                | ~5 min                | ~20-30 min                         | ~30-45 min                      | ~10-15 min                 |
| **No-code**                   | Yes                   | No                                 | No                              | Mostly                     |
| **Graph view + backlinks**    | Yes                   | No                                 | No                              | No                         |
| **Auto sidebar from folders** | Yes                   | Yes                                | No (manual config)              | No                         |
| **Full-text search**          | Built-in              | Built-in (Pagefind)                | Plugin (Algolia or local)       | Plugin needed              |
| **SEO**                       | Built-in, zero config | Excellent                          | Excellent                       | Basic                      |
| **Mobile**                    | First-class           | Responsive                         | Responsive                      | Depends on theme           |
| **Custom domain**             | Yes (included)        | Yes (free via host)                | Yes (free via host)             | Yes (free)                 |
| **Customization**             | Limited (CSS only)    | Full (Astro/React)                 | Full (React)                    | Limited (Liquid templates) |
| **React stack**               | No                    | Astro (React components supported) | React-native                    | No                         |
| **Republishing workflow**     | Click in Obsidian     | `git push` → auto-deploy           | `git push` → auto-deploy        | `git push` → auto-deploy   |
| **Graph view**                | ✅                    | ❌                                 | ❌                              | ❌                         |
| **Wiki-feel**                 | Native                | Good                               | Good                            | Poor                       |

---

## Recommendation

**For this repo:** Obsidian Publish is the strongest choice despite the cost.

The Berkshire Hathaway wiki has 88 interlinked pages with cross-references that are the _point_ of the wiki. Obsidian Publish is the only option that renders the graph view on the web, shows backlinks, and lets readers explore the knowledge network the way it was built. Setup is 5 minutes with zero code. The $8/month is justified by the hours saved versus configuring a free alternative, and by the quality difference in how the wiki-nature of the content is presented.

**If free is a hard requirement:** Starlight + Vercel is the best alternative. ~30 minutes of setup, beautiful defaults, auto-sidebar from your folder structure, built-in search. The main loss is the graph view and backlinks — the site becomes a good-looking documentation site rather than an explorable knowledge graph.

**GitHub Pages** is best used as the hosting layer for Starlight or Docusaurus (via a GitHub Actions workflow) — not as a standalone generator for a multi-folder wiki. It keeps everything in the GitHub ecosystem and is free, but the built-in Jekyll setup produces a poor wiki experience without significant customization.

**Next.js + MDX** is over-engineering for a content-only site — it's for when you need a full custom app with server-side logic.
