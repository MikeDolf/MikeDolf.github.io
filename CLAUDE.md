# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A static site served directly by GitHub Pages (`mikedolf.github.io`) with no build step, no framework, and no package manager — every page is a hand-written, self-contained HTML file with its CSS inlined in a `<style>` block. There is no `package.json`, no Jekyll config, no CI.

Two unrelated things currently live in this repo:

1. **TechReview** — the actual site: a Russian-language tech review/comparison blog (`index.html` plus everything under `obzory/`, `sravnenie/`, `top/`, the category hubs, etc.). This is the real content.
2. **`natyazhnye-potolki-ekb/index.html`** — a standalone lead-gen landing-page prototype for an unrelated business (stretch-ceiling installation in Yekaterinburg), built during niche-research exploration in this repo/branch. It is *not* linked from TechReview's nav, homepage, or `sitemap.xml`, does not share TechReview's design tokens, and should be treated as an independent one-off — don't assume changes to one affect the other.

## Working locally

There is no build/lint/test tooling of any kind. To preview changes, just serve the directory statically, e.g.:

```
python3 -m http.server 8000
```

then open `http://localhost:8000/<path>/`.

## TechReview architecture

**Clean-URL convention.** Every content page is `<section>/<slug>/index.html`, so it's reachable as `/<section>/<slug>/` with no `.html` in the link (GitHub Pages resolves `index.html` automatically). New pages must follow this — a page at `obzory/foo.html` would break the URL scheme the rest of the site uses.

**Top-level sections:**
- `obzory/` — individual product reviews (one folder per product)
- `sravnenie/` — head-to-head comparisons (one folder per pairing)
- `top/` — "top N" list pages (one folder per list)
- `gajdy/` — buying guides
- `smartfony/`, `noutbuki/`, `televizory/`, `audio/`, `apple/`, `samsung/`, `gadzhety/`, `bytovaya-tehnika/`, `flagmany/`, `do-10000/` — category/brand hub pages that list and link into `obzory/`, `sravnenie/`, and `top/` content
- `kontakty/`, `o-nas/`, `politika/` — utility pages (contact, about, privacy policy)

**Every page is fully self-contained.** There are no shared `.css`/`.js` files anywhere in the repo. Each page redeclares the same design-token set (`--white`, `--bg`, `--text`, `--accent`, `--radius`, etc.) inside its own `<style>` block, using the same font pairing (Geologica for UI text, Spectral for headings, both loaded from Google Fonts). Because there's no shared stylesheet, a global style change (e.g. a new accent color) has to be applied by editing every page individually — check a couple of existing pages in the target section first to match the exact token names and values already in use there before adding a new page.

**SEO artifacts are hand-maintained, not generated.** `sitemap.xml` and `robots.txt` are plain static files — when adding a new page under `obzory/`, `sravnenie/`, `top/`, or a category hub, add its canonical URL to `sitemap.xml` yourself; nothing does this automatically.

**Structured data.** Content pages carry inline JSON-LD (`<script type="application/ld+json">`): `Article` + `Person` (author) + `Organization` on review/guide pages, `CollectionPage` on hub/listing pages, `WebSite` on the homepage only. Match the existing pattern in a sibling page rather than inventing a new schema shape.
