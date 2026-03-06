# Quarto Anonymous Learning Journal – Implementation Plan

An anonymous, typography-first learning journal built with Quarto. No author profiles. Articles grouped by topic on the homepage, with built-in search/tag filtering, light/dark mode, and strong privacy controls.

## User Review Required

> [!IMPORTANT]
> This is a **pure Quarto static site** (no React/Redux). The tech stack differs from the user's global rules (React 18, RTL, etc.) — those rules apply to app projects, not Quarto document sites.

> [!NOTE]
> The `index.qmd` homepage uses Quarto's built-in **`listing`** feature (type: `default`) grouped by `categories`. This is the native Quarto way to render snippet cards with snippet text + clickable title hyperlinks. No custom JS needed for the core listing.

> [!WARNING]
> Quarto's built-in `search` is **site-wide** and appears in the navbar by default. A **homepage tag filter** requires either: (a) relying on the listing's built-in category filter chips, or (b) adding lightweight vanilla JS for live tag filtering. **Plan uses option (b)** for richer UX as specified.

## Proposed Changes

---

### Global Configuration

#### [MODIFY] [_quarto.yml](file:///c:/Users/ZakiZ/Documents/01_Projects/zakizulham.github.io/_quarto.yml)

- Sets `type: website`
- Activates **Bootswatch `flatly` (light) + `darkly` (dark)** as the base for a neutral, readable palette — these are the closest to R4DS-style clean book layouts
- Enables `search: true` in navbar
- Sets `light-toggle: true` for dark/light mode
- Sets `repo-url` and `back-to-top-button` for a book-like feel
- Defines `navbar` with logo/title only (no author identity)
- Defines `sidebar: false` (flat nav, not book-style)

---

### Theme & Typography

#### [NEW] [styles/custom.scss](file:///c:/Users/ZakiZ/Documents/01_Projects/zakizulham.github.io/styles/custom.scss)

- Imports `et-book` or system serif stack for body text (like R4DS book feel)
- Sets comfortable `line-height: 1.75`, `max-width: 680px` reader column
- Reduces navbar visual weight
- Snippet cards on homepage: clean border, no drop-shadows
- Tag pills: muted grey, no color accents

---

### Homepage

#### [MODIFY] [index.qmd](file:///c:/Users/ZakiZ/Documents/01_Projects/zakizulham.github.io/index.qmd)

- YAML frontmatter: `listing` block with `type: default`, `contents: posts`, `sort: "date desc"`, `categories: true`, `fields: [date, title, description, categories]`
- No author profile block
- Short intro paragraph (site purpose, no identity)
- Vanilla JS tag filter: reads URL hash (`#tag=xxx`) or button click to filter listing cards
- Search bar: delegates to Quarto's built-in site search

---

### Post Structure

#### [NEW] [posts/_metadata.yml](file:///c:/Users/ZakiZ/Documents/01_Projects/zakizulham.github.io/posts/_metadata.yml)

- Default `author: ""` (blank, anonymous)
- Default `freeze: true`

#### [NEW] [posts/example-post.qmd](file:///c:/Users/ZakiZ/Documents/01_Projects/zakizulham.github.io/posts/example-post.qmd)

- Demonstrates full frontmatter: `title`, `date`, `description` (snippet shown on homepage), `categories`, and privacy-controlled `include-in-header`

---

### Privacy & Security

#### [NEW] [robots.txt](file:///c:/Users/ZakiZ/Documents/01_Projects/zakizulham.github.io/robots.txt)

Blocks known AI training scrapers:
- `GPTBot` (OpenAI)
- `Google-Extended` (Gemini training)
- `CCBot` (Common Crawl – used by many LLMs)
- `anthropic-ai` / `Claude-Web`
- `Omgilibot`, `FacebookBot`, `Applebot-Extended`
- Generic `Disallow: /` for all listed bots

#### Per-page noindex (frontmatter pattern)

For articles tagged `privacy` or sensitive content, the user adds this to individual `.qmd` frontmatter:

```yaml
include-in-header:
  - text: |
      <meta name="robots" content="noindex, nofollow">
```

This injects directly into that page's `<head>`, invisible to readers, effective for Google & AI crawlers.

---

### Documentation

#### [NEW] [README.md](file:///c:/Users/ZakiZ/Documents/01_Projects/zakizulham.github.io/README.md)

Usage guide: how to add posts, how to apply per-page noindex, robots.txt maintenance.

---

## Verification Plan

### Manual Verification

1. **Install Quarto** if not present: https://quarto.org/docs/get-started/
2. From project root, run:
   ```bash
   quarto preview
   ```
3. Verify homepage shows article listing grouped by category with snippet text.
4. Verify search icon appears in navbar and returns results.
5. Click dark mode toggle — verify neutral dark theme activates.
6. Navigate to `http://localhost:PORT/robots.txt` — verify AI bot rules are present.
7. Open sample post — verify clean single-column layout with comfortable line spacing.
8. On a post with `include-in-header` noindex, open DevTools → Elements → `<head>` — verify `<meta name="robots" content="noindex, nofollow">` is present.
