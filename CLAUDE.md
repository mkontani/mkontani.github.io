# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Static GitHub Pages site at https://mkontani.github.io/ — mkontani's personal profile page plus a public hub for app/extension **landing pages**, **privacy policies**, and **support pages**. Pure HTML/CSS, no build system, no JS framework, no tests. Pushing to `main` deploys automatically via GitHub Pages. `.nojekyll` disables Jekyll processing.

## Local preview

All internal links are root-absolute (`/assets/style.css`, `/jwt-lens/`), so `file://` won't work. Serve from the repo root:

```bash
python3 -m http.server 8000
```

## Structure & page patterns

```
/index.html              profile page: intro hero + #apps section (self-contained)
/favicon.svg             site favicon (">_" mark), used by root index.html
/<product>/index.html    product marketing/landing page (self-contained)
/<product>/privacy.html  privacy policy
/<product>/support.html  support page
/<product>/icon.png      product icon (used as favicon + OG image)
/assets/style.css        shared styles for utility pages only
/assets/apps/<product>.png  app icons for the profile page's tiles
/assets/og.png           1200x630 social preview for root index.html
```

There are **three page styles** — don't mix them:

1. **Utility pages** (`privacy.html`, `support.html`): minimal pages that link `/assets/style.css` and use its classes (`.wrap`, `.site` header, `.card`, `.muted`, `.badge`). Light/dark via `prefers-color-scheme`. Header breadcrumb pattern: `<a href="/">mkontani</a> · <a href="/<product>/">Name</a>`.

2. **Product landing pages** (`<product>/index.html`): self-contained marketing pages with a large inline `<style>` block, Google Fonts (Bricolage Grotesque / Hanken Grotesk / JetBrains Mono), dark themed, and full OG/Twitter meta tags pointing at `https://mkontani.github.io/<product>/`. They do NOT use `/assets/style.css`.

3. **Profile page** (root `index.html`): mkontani's engineer profile, self-contained like the product pages (dark `#0b0d12`, Bricolage Grotesque / Hanken Grotesk / JetBrains Mono, Zen Kaku Gothic New for Japanese text, terminal accents like `$ whoami` and `// apps`). `lang="en"`; Japanese app names and descriptions carry `lang="ja"`. The page is a stack of `<section class="section">` blocks (`.section-head` + body) — only `#apps` exists today; skills / career / GitHub will be added later as more sections in the same pattern (and as links in the top nav).
   - Each app is an `<li class="app" style="--c: <accent>">` tile. `--c` is that product's own theme color and tints the tile's glow. The app name link is stretched over the whole tile (opens the product page); Store / Privacy / Support links sit above it and get `aria-label`s that include the app name.
   - Icons live in `/assets/apps/`, max 192px. Pixel-art icons (`majo-type`, `tekuteku-neko`) are copied as-is, never resampled.
   - Keep the `N products · 0 servers` count in `.section-head` in sync with the tiles. `0 servers` holds because every listed app's privacy policy says it has no server and no analytics; revisit it if that changes.
   - `bonsai/` is intentionally not listed (no release plan yet). Its pages stay online; re-add a tile when it ships.
   - `assets/og.png` was rendered from a throwaway 1200x630 HTML composition (hero + app icons) with Playwright. Regenerate it if the intro text or the app lineup changes.

`lang` attribute matches the product's audience: `sakura-grade` (サクラ診断) and `nokori` (Japanese market) use `lang="ja"`; `jwt-lens` uses `lang="en"`.

**Exception — `nokori/`** (migrated from the former `nokori-site` repo): its `privacy.html` is also fully self-contained with its own inline CSS (Zen Old Mincho / Zen Kaku Gothic New fonts, `--paper`/`--ink`/`--sage` design tokens duplicated in both files — keep both `:root` blocks in sync). Images live in `nokori/assets/` (`app-icon.png`, `screenshots/*.png`) and are referenced with relative paths. Its landing page reveal animation degrades gracefully without JS (`<html class="js">` gate + IntersectionObserver fallback) — preserve that. Only `nokori/support.html` uses the shared hub style.

**Exception — `cert-tool/`**: its landing page uses the light "security paper" look of the Chrome Web Store screenshots (Schibsted Grotesk, `--paper: #eaeff6`, ink `#14213d`, violet `#7c3aed`, a guilloché ring drawn by inline JS) instead of the dark default.
- **Hero**: a real PEM block plus replicas of the extension's on-page badge and details card.
  - The certificate is fictional, from a throwaway PKI whose keys were discarded. It is valid until 2027-05-07.
  - The card text was copied from the extension's own output for that certificate, so a visitor who runs Scan this page sees the same thing.
  - Days left are computed live in the page's JS, mirroring `src/page/validity.ts` in the extension repo.
  - If you change the certificate, regenerate the card text from the extension.
- **`cert-tool/privacy.html`** mirrors `PRIVACY.md` in `mkontani/cert-tool-extension`. Keep the two in sync.
- **`cert-tool/screens/*.webp`** are WebP copies of that repo's `screenshots/store/*.png`.
- **The `/cert-tool-extension/` path** belongs to that repo's own Pages site, a live demo of the popup. Never add a folder with that name here.

## Adding a new product

1. Copy an existing `/<product>/` folder (e.g. `jwt-lens/`) and edit the text, theme colors, and OG meta URLs.
2. Add a tile for it in the `#apps` section of root `index.html` (icon in `/assets/apps/`, `--c` accent, platform `.chip`, store/privacy/support links) and bump the `N products` count.
3. Add it to the Products list in `README.md`.
4. Update the "Last updated" date in `privacy.html` when policy content changes.

Contact email used across all pages: `itoama@gmail.com`.
