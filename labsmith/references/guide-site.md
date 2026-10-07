# Guide site

A static, generated site next to the lab: it ranks for what learners search, answers it well, and sends them into the matching exercise.

## 1. Single-source generator

- Content lives as data (one module per concern): structure (categories, order, beginner path, UI strings), articles (per language), links (scenes per article, glossary, tags), sources (references).
- One command regenerates everything: home, category pages, articles, sitemap, crawler files, redirects, manifest, 404, and the crawlable boot text of the lab's entry pages.
- Generated folders are rebuilt from scratch each run; hand-written assets (stylesheet, script) live in a folder the generator never deletes.
- The generator fails on: articles not placed in a category, unknown scenes, unknown sources, and any broken internal link across all pages (including the lab's entry pages).
- The generator and its data are not deployed; only the build output is.

## 2. Information architecture

```
/guide/                     guide home: hero + clip, browse by topic, beginner path, practice scenes, about the lab
/guide/<category>/          category: intro + article cards + other topics
/guide/<category>/<slug>/   article
/zh/guide/…                 same in the second language
```

- Five or so categories by learner intent (e.g. reading, methods, tolerances and decisions, instrument care, teaching and certification). Split a category beyond ~8 articles.
- Moving or renaming a published page adds a permanent 301 entry; never delete published redirects.
- Every page footer lists all categories and articles (site map and internal links).

## 3. Article template

Breadcrumbs → category label → H1 → reading time and updated date → **key answer** box (the first sentence answers the question) → collapsible contents (narrow screens) → lab clip → numbered sections (lists for steps, tables for comparisons, full worked examples) → references → practice scene cards → FAQ → "try it yourself" band → related guides → previous/next in category; a sticky contents column with an "open in the lab" button on wide screens.

## 4. Styles

- One stylesheet for all pages, colour tokens on `:root`, dark and light following `prefers-color-scheme`.
- Brand colour for links and accents; on light paper use a deeper tone of the same hue so links reach at least 4.5:1 contrast (e.g. cyan #00f0ff on dark, #00808a on white).
- Reading measure ≤ ~720 px; CJK body 17 px / 1.8; numbered section headings in a mono accent; tables in rounded cards that scroll horizontally on phones.
- Respect `prefers-reduced-motion`; print styles hide navigation, clips and cards.

## 5. Internal links

- **Glossary links:** the first mention of a term in an article body links to the article that explains it; not inside headings or existing links, not to the page itself, each target once, at most ~8 per page.
- **Related guides:** chosen by shared tags, a fixed number (3–5), plus the guide home.
- **Scene links:** every article links to its exercises; the main call to action opens the first.
- All of these are rebuilt on every generation, so older articles pick up links to newer ones automatically.

## 6. Entry pages of the lab

- Multi-page build with one entry per language path; full metadata in each; a crawlable boot text inside the app root listing the guides (replaced when the app mounts); `noscript` text for no-JS visitors.
- The lab's header and help panel link to the guide home.
