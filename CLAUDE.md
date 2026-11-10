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
- **"And the site you're on" card** (third card in the ways section): an HTML replica of the Inspector after *Show this site's certificate*, for the same fictional `shop.example.com` → Example Issuing CA R1 → Example Root CA chain as store screenshot 7. Its summary text and Leaf / Intermediate / Root tags copy the extension's `SiteSummary` and `ChainSelector`; update them if those change.
- **`cert-tool/privacy.html`** mirrors `PRIVACY.md` in `mkontani/cert-tool-extension`. Keep the two in sync.
- **`cert-tool/screens/*.webp`** are WebP copies of that repo's `screenshots/store/*.png`.
- **The `/cert-tool-extension/` path** belongs to that repo's own Pages site, a live demo of the popup. Never add a folder with that name here.

**`ssh-keytool/`** (Chrome extension SSH Keytool, source in `mkontani/ssh-keytool-extension`) follows the dark default, recolored to the extension's own navy / teal (`#2dd4bf`) / blue (`#60a5fa`) with circuit traces from its store promo tile.
- **Hero**: an HTML replica of the popup's Generate tab. Its key pair is a throwaway Ed25519 key whose private half was deleted after generation; only the public key and the first three lines of the private key file are shown (header + public part, no secret bytes). Keep it that way — never paste a full private key into a page.
- **FIDO section**: a small command builder demo. Its inline JS mirrors `buildKeygenCommand`, `buildAuthorizedKeysLine` and `buildSshdConfigLine` in that repo's `src/utils/fidoCommands.ts`; update it if those change. The form is `hidden` until JS runs, and the static output matches its default state. Its defaults follow store screenshot 2 (application `ssh:work`, comment `alice@laptop`, Require PIN on), not the extension's all-off defaults.
- **`ssh-keytool/screens/*.webp`** are WebP copies of that repo's `assets/webstore/0*.png`; `og.jpg` is its `promo-marquee-1400x560.png`.
- **`ssh-keytool/privacy.html`** has no counterpart in the extension repo (it has no `PRIVACY.md`). It was written from the code: no network requests, no storage, only the `clipboardWrite` permission. Re-check it when the extension gains permissions or I/O.
- **The `/ssh-keytool-extension/` path** belongs to that repo's own Pages site (the full app as a web page, linked as "web version"). Never add a folder with that name here.

**Exception — `chrometchi/`** (Chrome extension Chrometchi, a pixel-art virtual pet; source in `mkontani/chrometchi-extension`) uses the light pastel look of its Chrome Web Store images instead of the dark default.
- **Look**: the palette of that repo's `e2e/store/compose.ts`: ink `#2b2233`, cream `#fff4e0`, white cards with a 4px ink border and a hard 6px ink shadow, and a dot pattern. The bands follow the store screenshots' order (pink, mint, yellow, lavender, night). Headings use DotGothic16; body text uses Zen Maru Gothic.
- **Two languages in one page**: every piece of copy exists as a `lang="ja"` / `lang="en"` pair. CSS shows only the pair matching `<html data-lang>`. An inline script in `<head>` picks the language before first paint (`?lang=`, then the visitor's choice saved in `localStorage`, then the browser's first language). Without JS the page is Japanese and the switch is hidden. Add both languages whenever you change copy. `privacy.html` and `support.html` hold both languages one after the other (`#ja`, `#en`).
- **Walking pet**: a pet walks along the bottom of the landing page. It mirrors the extension's "Walk on web pages" feature.
  - Its inline JS mirrors that repo's `src/roam/constants.ts` and `src/roam/gait.ts`: rest and walk times, frame times, gait speeds, hop and bob heights.
  - Clicking it shows the happy frame and a bubble. As in the extension (`src/roam/view.ts`), the pet stands still while the bubble is open.
  - The bubble closes after a few seconds unless it is hovered or focused. Escape or a second click also closes it.
  - "Hide on this page" hides the pet, and the walk section's button brings it back. Focus moves to the button that replaces the one clicked.
  - With reduced motion it sits still.
- **Art**: `chrometchi/art/` holds copies of that repo's `src/assets` at the same paths. They are pixel art, so copy them as-is and show them only at whole multiples (2× / 3×). The one exception is the life-stage strip on screens narrower than 360px: there five 64px sprites do not fit, so it shows them at 48px.
  - `popup-{ja,en}.webp` are the real popup captured at 3× (1080×1386, lossless WebP).
  - `og.jpg` is that repo's `screenshots/ja/promo_marquee.png`.
- **`chrometchi/privacy.html`** mirrors `docs/store/privacy-policy.md` in the extension repo. Keep the two in sync.

**Exception — `status-cat/`** (Chrome extension Status Cat: pixel cats at the bottom of every page act out its HTTP status; source in the private `mkontani/status-cat-extension`) uses the light look of its store promo tiles instead of the dark default.
- **Look**: the tiles' palette from that repo's `scripts/generate_promo.py`: a cream wall `#faf2e4` with faint pixel paw prints (the inline SVG in `--paws`), a floor `#eedcc0` / `#e2ccaa`, ink `#2b2b2b`, and white cards with a 4px ink border and a hard 6px shadow. Headings use DotGothic16; body text uses Zen Maru Gothic.
- **Status chips** (`.code`) take the toolbar badge colours from that repo's `lib/status.js` (2xx `#1e9e4a`, 3xx `#2f6fde`, 4xx `#e07a00`, 5xx and ERR `#d93025`) as a border and a light tint, with ink text, because white on those colours is too faint to read.
- **Two languages in one page**, built as in `chrometchi/` (the `localStorage` key is `status-cat-lang`). Unlike Chrometchi, the page is English without JS, because the extension's default locale is English.
- **Images**: only the store assets, never the raw sprites. The extension's `sprites/` are all rights reserved, which is also why its repo is private.
  - `screens/*.webp` are lossless WebP copies of that repo's `promo/store/screenshot-*.png`.
  - `screens/err-box.png` is a 300x330 crop of `promo/store/marquee-promo-tile.png` at x 22, y 130 (the cat in the box). It is shown at half size, on a CSS floor whose top edge lines up with the tile's.
  - `og.jpg` is the marquee tile.
  - `icon.png` is that repo's `icons/icon128.png`. `/assets/apps/status-cat.png` is the same icon on the cream wall, made opaque because the profile tiles are dark.
- **`status-cat/privacy.html`** mirrors `promo/store/privacy-policy.md` in the extension repo. Keep the two in sync, and in step with what the extension stores (for example `HISTORY_LIMIT` in its `lib/tab-record.js`). The store's Privacy practices tab links to this page.

## Adding a new product

1. Copy an existing `/<product>/` folder (e.g. `jwt-lens/`) and edit the text, theme colors, and OG meta URLs.
2. Add a tile for it in the `#apps` section of root `index.html` (icon in `/assets/apps/`, `--c` accent, platform `.chip`, store/privacy/support links) and bump the `N products` count.
3. Add it to the Products list in `README.md`.
4. Update the "Last updated" date in `privacy.html` when policy content changes.

Contact email used across all pages: `itoama@gmail.com`.
