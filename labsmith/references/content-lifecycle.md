# Content lifecycle: authoring, review, audit

Each lab project keeps three content guides in its repository (templates in `../templates/`): an **authoring guide**, a **site review guide** and a **content audit guide**, plus dated review and audit logs. Agents maintaining the content follow those project guides; this reference states what they must contain and the rules behind them.

## 1. Authoring

- **When:** every new lab capability (instrument, part, task, wrong-use demo) gets at least one article covering how to do it and how it goes wrong, linked to its scene; planned topics from the keyword plan; frequent learner or teacher questions.
- **Steps:** intent and reader → facts (sources, recomputed examples, lab capability checked in code) → both languages written idiomatically → scenes → sources → register (structure, tags, scenes, glossary) → generate → delivery checks.
- **Writing:** conclusion first; one idea per paragraph; steps as ordered lists, comparisons as tables, full worked examples with units; no "planned" features presented as available; no brand disparagement; ranges and conditions for rules of thumb.
- **Terminology table:** the project's canonical terms, the variants to avoid, and when an exception is allowed (e.g. school physics says 精度 for least count — write 精度（分度值）on first use). Units and symbols: value and unit separated by a space, `−` for minus, `×`, `°C`, `µm`.
- **Definition of done:** generator checks pass; project gates pass; languages agree on every number; key answer answers; every claim that needs a source has one; examples recomputed; capability statements match the current lab; page viewed in dark, light and phone widths; keyword plan updated.

## 2. Syncing with lab releases

| Lab change | Content change |
| --- | --- |
| New instrument | New reading/usage article; scenes; glossary and tags; update articles that called it "planned" |
| New part or task | Extend method articles or write one; scenes; update the lab's feature list in llms and FAQ |
| New wrong-use demo | Add to the article's mistakes table; `error-*` scene linked from the errors article |
| UI rename or flow change | Review every article describing that control; align wording with the i18n files |
| Preset change | Run the scene tests and the generator; re-record affected clips |

## 3. Periodic review

- **Triggers:** lab release; terminology change; a cited standard revised or withdrawn; search data showing a mismatch between queries and content; generator or schema changes; quarterly.
- **Procedure:** open a dated review log (trigger, scope, lab commit) → inventory the articles → check terminology, capability statements, scenes and clips, technical practice, structure and search performance → fix → regenerate and verify (open the scenes in the lab) → change the updated date only for substantive edits → record results and carry-overs.
- Replace terms by context, not blindly; write every terminology decision back into the authoring guide.

## 4. Content audit

**Dimensions:** truthfulness (things exist as stated), accuracy (values, formulas, units, examples, both languages), objectivity (no hype or bias), controversy (alternative practices presented fairly), authority (first-hand sources), currency (current editions), verifiability (sources linked and reachable).

**Evidence levels:** A — publisher pages or texts of standards, verification regulations, national metrology institutes; B — peer-reviewed papers, authoritative textbooks, industry bodies; C — manufacturer documentation (that product only); D — reproducible calculation or lab demonstration with the steps shown. Encyclopedias, forums and marketing pages are leads only.

**Source and link rules:**

- A claim about a standard, a rule of thumb, a person, an organisation, history or a product parameter needs a source; link people, technologies and products to an authoritative page where one exists.
- Links go to the publisher's own page (standards bodies' catalogue pages, DOI pages, institute pages). No paid-download mirrors or unknown PDFs. No stable public page → cite by number without a link.
- Verify by opening the page and matching title, number and year. When a publisher blocks automated requests (ISO returns 403), confirm in a browser or through the search index's title for that exact URL, and write the method in the audit log.
- Keep sources in data (title, publisher, URL or none) attached per article; render a references section and `citation` structured data from it, so an audit fix is one edit.

**Procedure:** dated audit log (scope, auditor, lab and site commits) → list every verifiable statement per article → verify each (source key, calculation, code location) → verdict (true / wrong / narrow / contested / unverified) → severity (critical: leads to a wrong measurement or decision, invented standard, non-existent feature; major: missing necessary source, one-sided contested practice, outdated edition that matters; minor: terminology, tone, language mismatch without effect; suggestion) → fix critical and major now → re-verify → conclusion and carry-overs.

**Contested topics:** state the common practice, then the alternatives and their conditions, with sources; do not decide for the reader without stating when the advice applies.

## 5. Cadence

New article: self-audit before delivery. After each full review: audit at least three articles. At least once a year: full-site audit.
