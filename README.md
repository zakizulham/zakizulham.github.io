# 📓 Catatan Belajar

> An anonymous learning journal — built with [Quarto](https://quarto.org), deployed on GitHub Pages.

A personal, minimalist knowledge archive. No author name. No profile. Just notes.

**[→ Read the journal](https://zakizulham.github.io/)**

---

## Features

- **Topic groupings** — articles organised by category on the homepage
- **Live tag filter** — filter cards by topic with URL deep-link support (`#tag=Git`)
- **Site-wide search** — built-in Quarto search in the navbar
- **Light & dark mode** — neutral, typography-first palette; no accent colours
- **Reader-optimised layout** — 680 px column, 1.78 line-height (inspired by [R for Data Science](https://r4ds.hadley.nz/))
- **Privacy controls** — AI-scraper blocking via `robots.txt` + per-page `noindex` support

---

## Tech Stack

| Layer | Tool |
|---|---|
| Site generator | [Quarto](https://quarto.org) |
| Theme base | Bootswatch Flatly (light) / Darkly (dark) |
| Typography | Inter (sans-serif), JetBrains Mono (code) |
| Hosting | GitHub Pages |

---

## Project Structure

```
.
├── _quarto.yml              # Global config — theme, navbar, search
├── index.qmd                # Homepage — listing + tag filter
├── archive.qmd              # Full archive (table view)
├── about.qmd                # About page
├── robots.txt               # Blocks AI training scrapers
├── styles/
│   ├── custom-light.scss    # Light theme overrides
│   ├── custom-dark.scss     # Dark theme overrides
│   └── custom.css           # Runtime CSS (fonts, card reset)
└── posts/
    ├── _metadata.yml        # Default frontmatter for all posts
    └── YYYY-MM-DD-slug/
        └── index.qmd        # Article file
```

---

## Writing a New Post

1. Create a folder under `posts/`:
   ```
   posts/2026-03-07-my-note/index.qmd
   ```

2. Add frontmatter:
   ```yaml
   ---
   title: "Your Title"
   description: >
     One or two sentences — shown as the snippet on the homepage.
   date: 2026-03-07
   categories:
     - Topic
     - Tag
   ---
   ```

3. Write the article body in Markdown below the frontmatter.

> The `description` field is what appears as the article snippet on the homepage listing. Keep it to 1–2 sentences.

---

## Running Locally

```bash
# Requires Quarto — https://quarto.org/docs/get-started/
quarto preview
```

---

## Publishing to GitHub Pages

For automated deployment on every push, add a GitHub Actions workflow following the [official Quarto guide](https://quarto.org/docs/publishing/github-pages.html#github-action).

Or publish manually:

```bash
quarto render
git add .
git commit -m "publish"
git push
```

---

## Privacy

This site uses a two-layer privacy approach for sensitive content:

**Layer 1 — `robots.txt` (site-wide)**
Blocks known AI training crawlers: `GPTBot`, `Google-Extended`, `CCBot`, `ClaudeBot`, `Bytespider`, `PerplexityBot`, and others. Googlebot for normal search indexing remains allowed.

**Layer 2 — Per-page `noindex` (article-level)**
Add to a post's frontmatter to prevent Google from indexing that specific page while keeping it publicly accessible via direct link:

```yaml
include-in-header:
  - text: |
      <meta name="robots" content="noindex, nofollow">
```

> Maintain the AI bot list using [Dark Visitors](https://darkvisitors.com) or the community project [ai-robots-txt](https://github.com/ai-robots-txt/ai.robots.txt).

---

*Built with Quarto. Hosted on GitHub Pages.*
