---
name: labsmith
description: >
  Build and maintain browser-based industry training labs (interactive 3D simulations of real
  instruments, tools or procedures, e.g. calipers, micrometers, gauges, torque tools, lab
  equipment) together with their guide/content site: domain-truth modelling, believable
  interaction and camera, wrong-use demonstrations, scene deep links, recorded lab clips,
  a generated guide site with SEO/GEO, and the content lifecycle (authoring, periodic review,
  evidence-based content audit with sources). Use for 实训室, 虚拟仿真实训, 仿真教学, 量具/仪器
  仿真, 职业教育实训, 培训模拟器, 实训站点, 专题文章, 指南站, 内容审计, SEO/GEO for a training
  lab, or keeping guide articles in step with lab releases. Not a replacement for general
  planning, execution, debugging, code audit or lesson capture — hand those to planner,
  tasker, bugfixer, auditor and evolver.
argument-hint: "Describe the trade or instrument, learners, current stack and repo state, and which part you need: lab, scenes/media, guide site, SEO/GEO, or content authoring/review/audit."
user-invocable: true
disable-model-invocation: false
version: 1.0.0
---

# Labsmith

Build training labs that teach a real skill correctly, and a guide site that brings learners to them and stays true as the lab grows. A lab that looks right but measures wrong, or an article that reads well but states something unverifiable, is a failure.

## When to use, and what to hand off

Labsmith owns the domain playbook for this product type. It does not repeat the general skills:

| Need | Use |
| --- | --- |
| Early planning, stack and doc system for a new lab | `planner` (then Labsmith's references for lab-specific decisions) |
| Executing a change as a checkable contract | `tasker`, with Labsmith's gates below |
| A concrete regression or "still wrong" | `bugfixer` |
| Code, security or deployment audit | `auditor` (Labsmith's content audit covers articles, not code) |
| Persisting a verified lesson as a rule or guard | `evolver` |

## References

Load only what the task needs:

- [references/lab-engineering.md](references/lab-engineering.md) — domain truth layer, units, validity, wrong-use demos, interaction, camera and transitions, layout, i18n, verification of 3D UI.
- [references/scenes-and-media.md](references/scenes-and-media.md) — scene deep links, their tests, and recording short lab clips with posters.
- [references/guide-site.md](references/guide-site.md) — generated guide site: information architecture, templates, styles, internal links, redirects, link check.
- [references/seo-geo.md](references/seo-geo.md) — audiences and keyword matrix, page-per-intent, metadata, structured data, sitemap, crawler files, post-launch work.
- [references/content-lifecycle.md](references/content-lifecycle.md) — authoring, periodic review and content audit; sources and terminology; syncing articles with lab releases.
- [references/security-and-deploy.md](references/security-and-deploy.md) — static deploy scope, headers and CSP, framing, parameter handling, testing headers locally.
- [templates/](templates/) — starting points for the project's content guides and audit log.

## Non-negotiable rules

1. **One source of measurement truth.** A framework-free domain layer computes contact, readings, validity and pass/fail; UI and render only dispatch commands and draw state. Never compute a reading in the render layer.
2. **An invalid setup never passes.** Tilt, unchecked zero, debris, excessive force, off-centre or unseated contact produce a validity other than `valid`, and tolerance is `not_evaluated`. Wrong-use demonstrations are never judged against tolerance.
3. **Real units and real dimensions.** Pick one internal unit (e.g. mm, rad) and one scene scale (1 unit = 1 mm). Instrument and part data stay `draft` until a domain expert reviews them; say so in the UI.
4. **Teach the trade's actual practice.** Every task gets the wrong uses that practitioners actually make, each with cause, effect on the reading and the correction. Source them from standards, verification procedures and shop practice, not imagination.
5. **No claim without evidence.** In articles, every standard, value, rule of thumb, person, product or historical statement has an A–D level source (see content lifecycle) or is narrowed until it does. Lab capability claims are checked against code and tests.
6. **Stack discipline.** Respect the project's locked stack; tooling used only to build content (browsers for capture, ffmpeg) stays outside project dependencies and outside the deployed bundle.
7. **Both languages, always.** User-facing strings go through i18n with a parity test; articles exist in every site language with the same facts, written idiomatically rather than translated line by line.

## Workflow

1. **Ground.** Read the project instructions, the domain model, the scene presets and the content plan. Identify the instrument family, learners and the skills the lab must teach.
2. **Decide the slice.** Lab feature, scene/media, guide page, SEO/GEO, or content review/audit. Name the observable outcome (e.g. "depth task reads 20.05 mm on the blind part and the not-seated demo reads high").
3. **Domain first.** Model geometry, constraints and validity in the domain layer with tests before wiring UI. Include the wrong-use cases of the task.
4. **Interaction and camera.** Make every motion continuous (glide, not jump), make toggles reversible to the previous view, keep tooltips quiet over readouts and demos, and resolve drag conflicts explicitly.
5. **Link the content.** Add or update a scene preset for the new capability, its test, its clip, and the articles that teach it (content lifecycle §sync).
6. **Verify like a learner.** Run the project gates, then open the real app: take the reading, run each demo, check light/dark and phone layouts. Screenshots or recorded frames are evidence; "should work" is not.
7. **Close.** Commit with the gates passing, republish any preview, and report what was verified and what was not. Route durable lessons to `evolver`.

## Gates

Before calling lab work done:

- domain tests cover the new task, including every wrong-use scenario and the "invalid never passes" rule;
- each scene preset opens a valid setup; `-reject` presets fail tolerance; demo presets never pass;
- i18n parity holds; typecheck, lint, tests and build pass;
- the feature was exercised in a browser (WebGL), with screenshots for visual changes.

Before calling content work done:

- the generator passes its internal-link and scene checks;
- every new source URL was opened (or, when the publisher blocks automated access, confirmed and logged);
- examples recomputed; both languages agree on every number;
- capability statements match the current lab version.

## Reporting

State the slice, what changed, how it was verified (commands, screenshots, frames), and what remains unverified or manual (e.g. DNS binding, search-console submission, browser-only source checks). Keep the user's decisions with the user: ask only when a choice changes the outcome and cannot be settled from the code or conventions.
