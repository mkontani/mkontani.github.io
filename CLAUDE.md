# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Static GitHub Pages site at https://mkontani.github.io/ — a public hub for app/extension **landing pages**, **privacy policies**, and **support pages**. Pure HTML/CSS, no build system, no JS framework, no tests. Pushing to `main` deploys automatically via GitHub Pages. `.nojekyll` disables Jekyll processing.

## Local preview

All internal links are root-absolute (`/assets/style.css`, `/jwt-lens/`), so `file://` won't work. Serve from the repo root:

```bash
python3 -m http.server 8000
```

## Structure & page patterns

```
/index.html              landing page listing product cards
/<product>/index.html    product marketing/landing page (self-contained)
/<product>/privacy.html  privacy policy
/<product>/support.html  support page
/<product>/icon.png      product icon (used as favicon + OG image)
/assets/style.css        shared styles for utility pages only
```

There are **two distinct page styles** — don't mix them:

1. **Utility pages** (`privacy.html`, `support.html`, root `index.html`): minimal pages that link `/assets/style.css` and use its classes (`.wrap`, `.site` header, `.card`, `.muted`, `.badge`). Light/dark via `prefers-color-scheme`. Header breadcrumb pattern: `<a href="/">mkontani</a> · <a href="/<product>/">Name</a>`.

2. **Product landing pages** (`<product>/index.html`): self-contained marketing pages with a large inline `<style>` block, Google Fonts (Bricolage Grotesque / Hanken Grotesk / JetBrains Mono), dark themed, and full OG/Twitter meta tags pointing at `https://mkontani.github.io/<product>/`. They do NOT use `/assets/style.css`.

`lang` attribute matches the product's audience: `sakura-grade` (サクラ診断) and `nokori` (Japanese market) use `lang="ja"`; `jwt-lens` uses `lang="en"`.

**Exception — `nokori/`** (migrated from the former `nokori-site` repo): its `privacy.html` is also fully self-contained with its own inline CSS (Zen Old Mincho / Zen Kaku Gothic New fonts, `--paper`/`--ink`/`--sage` design tokens duplicated in both files — keep both `:root` blocks in sync). Images live in `nokori/assets/` (`app-icon.png`, `screenshots/*.png`) and are referenced with relative paths. Its landing page reveal animation degrades gracefully without JS (`<html class="js">` gate + IntersectionObserver fallback) — preserve that. Only `nokori/support.html` uses the shared hub style.

## Adding a new product

1. Copy an existing `/<product>/` folder (e.g. `jwt-lens/`) and edit the text, theme colors, and OG meta URLs.
2. Add a card for it in root `index.html` (with `.badge` showing the platform, e.g. Chrome/Safari).
3. Add it to the Products list in `README.md`.
4. Update the "Last updated" date in `privacy.html` when policy content changes.

Contact email used across all pages: `itoama@gmail.com`.
