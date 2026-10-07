# SEO and GEO for a training lab

## 1. Audiences, scenarios, intents

Build the keyword plan from people and situations, not from a word list:

- **People:** school students (physics practicals), vocational and technical college students, university labs, teachers and lab managers, company trainers, front-line roles (inspectors, machinists, fitters, metrology staff), certification and skills-competition candidates, makers.
- **Scenarios:** classroom projection, pre-lab practice, remote and blended lessons, homework via share links, mistake-focused lessons, pass/fail training, onboarding, exam preparation.
- **Intents:** how to read X, how to use X, X vs Y, why my reading is wrong, how to calibrate, how to choose, tolerances and decisions, course and lab-building terms.

Write a matrix per language: category → head terms → long-tail questions → landing page. Write each language in its own search habits; do not translate keywords.

## 2. One page per intent

- One article answers one intent; two pages never compete for the same query (merge or re-scope, then redirect).
- Title (core term first), description (what the page gives), H1, key-answer box, H2s that mirror real sub-questions, FAQ with 2–4 real questions.
- Natural keyword use in title, description, headings and body; meta keywords only for terms the page truly covers.

## 3. Generative engines (GEO)

- Put the direct answer first (definition, formula, value), then tables, worked examples and short Q&A — the parts answer engines quote.
- Cite standards and authoritative sources with links; state ranges and conditions instead of absolutes.
- Publish `llms.txt` (site summary, guides by category, scene links) and `llms-full.txt` (full article text with references).
- Allow AI crawlers in `robots.txt` unless the owner decides otherwise.

## 4. Technical checklist

- `canonical`; `hreflang` for each language plus `x-default`; `og:*` and Twitter cards with a real preview image (JPEG/PNG, not only WebP).
- Structured data: articles as `TechArticle` (with `articleSection`, `wordCount`, `timeRequired`, `citation`, `video`), `FAQPage`, `BreadcrumbList`, `HowTo` where steps exist; home and categories as `CollectionPage` + `ItemList`; the lab as `WebApplication` (educational, free) with `Organization` and `WebSite`.
- Sitemap with `xhtml:link` alternates and image/video extensions.
- Baidu: `applicable-device`, `Cache-Control: no-transform`; submit to Baidu as well as Google and Bing.
- Fast static pages; explicit media dimensions; lazy images below the fold.
- Validate every JSON-LD block parses; spot-check with the search engines' rich-result tools after launch.

## 5. Honesty about data

Keyword choices made without search-volume or ranking tools are hypotheses. Say so, then replace them with real queries from Search Console and Baidu's platform after launch: adjust titles, descriptions and key answers to what people actually search, and add articles for uncovered queries.

## 6. Post-launch manual work (owner)

Bind the domain, redirect `www` and any old host to the canonical domain, verify the site in the search consoles and submit the sitemap, then build citations: course platforms, teacher communities, video tutorials, repository READMEs.
