# Build path: from an empty folder to a published lab

Follow the stages in order. Each stage names the companion skill that runs it, what to say to the agent, the deliverables, and the **exit gate** — the stage is not done until the gate passes with evidence. Do not start a stage while the previous gate is red.

Start every session by stating the current stage and the companion skills available (see SKILL.md, "Companion skills"). Keep a short status table in the project's planning docs: stage, gate state, evidence link.

The first release is deliberately small: **one instrument, one task, three to five parts, two to four wrong uses, two languages, five articles.** Breadth comes after the loop works end to end.

---

## S0 — Kickoff and planning

**Skill:** `planner` (Labsmith supplies the lab-specific decisions below). **Say:** "Use planner and labsmith to plan a training lab for <trade/instrument> for <learners>. Produce the planning docs and project instructions; no product code yet."

Decide and write down:

1. **Learners and outcomes** — who uses it (students, apprentices, inspectors, teachers), in which scenario (class demo, pre-lab, homework, onboarding, exam prep), and the skills they must demonstrate.
2. **MVP scope** — the instrument, the first task, the parts (include at least one made out of tolerance), the wrong uses (from the trade's real practice, with a source for each), languages.
3. **Concept mapping** — fill the table in `adapting-to-a-trade.md` for this trade.
4. **Runtime contract** — use `templates/runtime-contract.md`: units, coordinate scale, instrument and part definitions, tasks, validity values, tolerance evaluation, commands and rejections, share-state format.
5. **Stack** — locked versions in a stack document; changes only through a decision card (`templates/decision-card.md`). A proven default: Vite + React + TypeScript (strict), three.js with @react-three/fiber, a small store (zustand), i18next, Vitest, ESLint with import boundaries; static hosting. Any equivalent stack works if it keeps the domain framework-free.
6. **Experience spec** — layout, readout view, camera behaviour, demo behaviour, accessibility, phone support.
7. **Project instructions** — create the agent instruction file (`CLAUDE.md` / `AGENTS.md`) from `templates/project-instructions.md`, including the gate command.
8. **Repository scaffold** — empty app that builds; lint, typecheck, test and build wired into one gate command; CI running it.

**Exit gate:** planning docs exist and agree with each other; the gate command passes on the scaffold; layering lint rule rejects an import from UI into the domain (prove it with a deliberate failing import, then remove it).

---

## S1 — Domain core

**Skill:** `tasker`. **Say:** "Use tasker and labsmith: implement the domain layer for <instrument> and the <task> task per the runtime contract, with tests. No UI."

Deliverables: instrument definition (range, resolution, readout type and behaviour such as zero/hold/units); part library (nominal, actual, tolerance, material, draft status); geometry and contact solver for the first task; reading quantised to the instrument's resolution; validity; tolerance evaluation; commands and reducer with rejection reasons; share-state encode/decode with schema version.

**Exit gate:** tests show (a) the correct reading on every MVP part, (b) every invalid condition yields a non-`valid` validity and `not_evaluated` tolerance, (c) the out-of-tolerance part fails, (d) rejected commands leave state unchanged, (e) share state round-trips.

---

## S2 — Instrument model and readout

**Skill:** `tasker`; `bugfixer` for visual defects. **Say:** "Use tasker and labsmith: build the 3D model of <instrument> from real dimensions and render the domain reading on its scale/display, with a readout view toggle."

Deliverables: model built from measured dimensions or reference photos (1 unit = 1 mm or the contract's unit); scales and displays generated from the domain's scale definition; the render reads state only; readout view that turns square to the scale and returns to the previous view.

**Exit gate:** screenshots at three openings where the visible scale/display equals the domain value; readout legible at desktop and phone widths; readout view toggles there and back.

---

## S3 — Interaction and camera

**Skill:** `tasker`; `bugfixer` when a gesture misbehaves. **Say:** "Use tasker and labsmith: make the instrument operable — drag the moving parts, place it on the part, lock it — with smooth camera and touch support."

Deliverables: joint drags limited by the domain (mechanical and contact limits); placing on and lifting off the part; small controls with large grab targets and no drag conflicts; glides for programmatic changes; hover only on the canvas; long-press on touch; camera framing that respects panels.

**Exit gate:** a person (or a scripted browser session) takes a valid reading by hand on desktop and on a phone viewport; recorded screenshots of each step; no control triggers the wrong gesture.

---

## S4 — Interface shell and languages

**Skill:** `tasker`. **Say:** "Use tasker and labsmith: build the navigation, measuring panel, part tray and task list in both languages."

Deliverables: navigation and context panel (status, reading, instrument controls, task, part); part tray and part cards; feedback that never implies success on an invalid setup; i18n for all strings with a parity test; language in the URL path for crawlable versions.

**Exit gate:** parity test passes; every panel screenshot reviewed in both languages, light/dark if supported, phone width; feedback shown for valid, invalid and out-of-tolerance cases matches the domain.

---

## S5 — More tasks and wrong-use demonstrations

**Skill:** `tasker`; `auditor` for a domain review before release. **Say:** "Use tasker and labsmith: add the <task> task and the wrong-use demonstrations <list> with guidance cards and camera focus."

Deliverables: per task, the scenario list in the domain; each demo stages itself, moves the reading in the documented direction, never passes tolerance, and has a guidance card (cause, effect, one correcting action); camera focuses on the fault and returns; closing the card ends the demo.

**Exit gate:** a domain test per scenario (direction and size of the effect, never pass); each demo exercised in the browser with screenshots; a review of the wrong-use list against its sources.

---

## S6 — Scenes, share links and clips

**Skill:** `tasker`. **Say:** "Use tasker and labsmith: add scene presets for every task and demo, their tests, and record the lab clips."

Deliverables: presets file read by app and generator; `?scene=` handling (own keys only, after the opening move, hash wins, parameter removed); tests for each preset; opening-move marker; capture script producing clip, poster and preview per scene and language (see `scenes-and-media.md`).

**Exit gate:** preset tests pass (valid, pass, `-reject` fails, demos never pass); every scene link opens correctly in a browser; clip frame strips checked; sizes within budget.

---

## S7 — Guide site, SEO/GEO and content system

**Skill:** `tasker`; `auditor` (code) and Labsmith's content audit (articles) before launch. **Say:** "Use tasker and labsmith: build the guide site generator, the first five articles with sources, and the project's authoring, review and audit guides."

Deliverables: keyword matrix by audience and scenario (`seo-geo.md`); generator, templates and stylesheet (`guide-site.md`); home, categories and five articles — basics, reading the instrument, the first task's method, wrong uses, teaching/training — each with key answer, clip, scenes, references and FAQ; structured data, sitemap with media, robots, `llms.txt`; the three content guides and an audit log from the templates.

**Exit gate:** generator passes link, scene and source checks; all JSON-LD parses; pages reviewed in dark, light and phone widths; every article self-audited and logged; source URLs opened or logged as browser-verified.

---

## S8 — Release and operations

**Skill:** `auditor` before release; `evolver` after. **Say:** "Use auditor to check release readiness, then labsmith to configure headers and deployment; afterwards use evolver to record what we learned."

Deliverables: security headers and CSP tested locally (`security-and-deploy.md`); deploy only the build output; domain, redirects of other hosts, search-console verification and sitemap submission (owner tasks); review and audit cadence in the calendar; lessons recorded in the evolution ledger.

**Exit gate:** production pages return the expected headers and redirects; scene links work on the production domain; the audit findings marked critical or major are fixed; the ledger has the release entry.

---

## After launch — the feature loop

For every new capability, run the same loop and do not skip steps:

1. domain model and tests (S1 rules) →
2. model, interaction and UI (S2–S4 rules) →
3. demos for its wrong uses (S5) →
4. scene presets, tests and clips (S6) →
5. articles and updates to older articles via the content sync table (S7, `content-lifecycle.md`) →
6. gates, release, review, and `evolver` for lessons.

Periodically: site review (quarterly or on lab release) and content audit (yearly, plus spot checks after each review).
