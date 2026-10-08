---
name: labsmith
description: >
  Build and maintain browser-based industry training labs (interactive 3D simulations of real
  instruments, tools or procedures, e.g. calipers, micrometers, gauges, torque wrenches,
  multimeters, pipettes) together with their guide/content site, from an empty folder to a
  published product: staged build path, domain-truth modelling, believable interaction and camera,
  wrong-use demonstrations, scene deep links, recorded lab clips, a generated guide site with
  SEO/GEO, and the content lifecycle (authoring, periodic review, evidence-based content audit
  with sources). Use for 实训室, 虚拟仿真实训, 仿真教学, 量具/仪器/工具仿真, 职业教育实训, 培训模拟器,
  从零开发实训室, 实训站点, 专题文章, 指南站, 内容审计, SEO/GEO for a training lab, or keeping guide
  articles in step with lab releases. Works best with the Tealun Skills (planner, tasker,
  bugfixer, auditor, evolver), which run the planning, execution, debugging, audit and
  lesson-capture stages.
argument-hint: "Describe the trade or instrument, learners, current stage (or 'from zero'), stack and repo state, and what you need: plan, lab feature, scenes/media, guide site, SEO/GEO, or content authoring/review/audit."
user-invocable: true
disable-model-invocation: false
version: 1.2.0
---

# Labsmith

Build training labs that teach a real skill correctly, and a guide site that brings learners to them and stays true as the lab grows. A lab that looks right but measures wrong, or an article that reads well but states something unverifiable, is a failure.

## Start here

1. **Check the companion skills** (next section) and tell the user which are available. If any is missing, say plainly that stage results cannot be guaranteed without it, recommend installing it, and use the fallback protocol for that role until it is installed.
2. **Find the stage.** Starting from an empty folder → S0 in [references/build-path.md](references/build-path.md). An existing project → read its instructions file and planning docs, and identify the current stage and its exit gate.
3. **Work one stage (or one feature loop) at a time**, using the companion skill the stage names, until its exit gate passes with evidence.

## Companion skills — strongly recommended

Labsmith supplies the domain playbook for training labs. The general working disciplines that make each stage reliable come from the **Tealun Skills**: <https://github.com/tealun/Tealun-Skills>. Install them before starting; without them, an agent cannot be relied on to plan coherently, to prove each change, to fix defects from their cause, to audit with evidence, or to keep lessons across sessions.

```bash
git clone https://github.com/tealun/Tealun-Skills.git
cd Tealun-Skills
bash ./sync-skills.sh          # Windows: .\sync-skills.ps1
```

Then start a new agent session so the skills load.

| Role | Skill | Used in stages |
| --- | --- | --- |
| Planning, architecture, docs system, readiness gates | `planner` | S0, and before any new instrument or major feature |
| Executing each change as a checkable contract with evidence | `tasker` | S1–S7 and every feature loop |
| Reproducing and fixing a concrete defect from its cause | `bugfixer` | whenever a check fails or the user says it is still wrong |
| Evidence-backed code, security and release audit | `auditor` | end of S5, S7, S8, and before releases |
| Turning verified lessons into rules, guards and a ledger | `evolver` | after S8, and after any repeated correction |

**Detecting them:** look for the skills in the agent's skill list (or the skills directory, e.g. `~/.claude/skills/<name>/SKILL.md`). Record the result in the session's first message.

**Fallback protocols** — the minimum to apply while a companion skill is missing (they do not replace it):

- *planner missing:* write every S0 decision into the planning docs before code; list assumptions and open questions; get the user's confirmation on scope, stack and runtime contract.
- *tasker missing:* before each change state scope, files and verification; after it, show the command output or screenshots; never report done without evidence; give each requirement a verdict.
- *bugfixer missing:* reproduce or observe the symptom first; test competing causes with a check that tells them apart; change only the proven cause; verify the original symptom is gone.
- *auditor missing:* report only findings with file/line or reproducible evidence, with severity and confidence; keep auditing read-only until fixes are requested.
- *evolver missing:* after a verified lesson, add a rule to the project instructions or a check to the gates, and append a dated line to `docs/98_evolution/evolution-ledger.md`.

## References

Load only what the task needs:

- [references/build-path.md](references/build-path.md) — **from zero to release**: stages S0–S8, what to say to the agent, deliverables, exit gates; then the feature loop.
- [references/adapting-to-a-trade.md](references/adapting-to-a-trade.md) — mapping a new trade or product onto the instrument–subject–reading–validity–specification pattern, with worked maps, safety rules and what stays the same.
- [references/lab-engineering.md](references/lab-engineering.md) — domain truth layer, units, validity, wrong-use demos, interaction, camera and transitions, layout, i18n, verification of 3D UI.
- [references/scenes-and-media.md](references/scenes-and-media.md) — scene deep links, their tests, and recording short lab clips with posters.
- [references/reenactments.md](references/reenactments.md) — scripted lab films: script format, generated scripts, tests, film/animation clocks, director, canvas captions, follow mode, deterministic offline render, publishing.
- [references/guide-site.md](references/guide-site.md) — generated guide site: information architecture, templates, styles, internal links, redirects, link check.
- [references/seo-geo.md](references/seo-geo.md) — audiences and keyword matrix, page-per-intent, metadata, structured data, sitemap, crawler files, post-launch work.
- [references/content-lifecycle.md](references/content-lifecycle.md) — authoring, periodic review and content audit; sources and terminology; syncing articles with lab releases.
- [references/security-and-deploy.md](references/security-and-deploy.md) — static deploy scope, headers and CSP, framing, parameter handling, testing headers locally.
- [templates/](templates/) — project instructions, runtime contract, decision card, and the three content guides with an audit log.

## Non-negotiable rules

1. **One source of measurement truth.** A framework-free domain layer computes contact, readings, validity and pass/fail; UI and render only dispatch commands and draw state. Never compute a reading in the render layer.
2. **An invalid setup never passes.** Any condition that makes a reading invalid produces a validity other than `valid`, and tolerance is `not_evaluated`. Wrong-use demonstrations are never judged against tolerance.
3. **Real units and real dimensions.** One internal unit system and one scene scale. Instrument and subject data stay `draft` until a domain expert reviews them; say so in the UI.
4. **Teach the trade's actual practice.** Every task gets the wrong uses practitioners actually make, each with cause, effect on the reading and correction, sourced from standards, procedures and shop practice. In safety-relevant trades, never present an unsafe act as acceptable.
5. **No claim without evidence.** In articles, every standard, value, rule of thumb, person, product or historical statement has an A–D level source (content lifecycle) or is narrowed until it does. Lab capability claims are checked against code and tests.
6. **Stack discipline.** Respect the project's locked stack; tooling used only to build content (browsers for capture, ffmpeg) stays outside project dependencies and outside the deployed bundle.
7. **Both languages, always.** User-facing strings go through i18n with a parity test; articles exist in every site language with the same facts, written idiomatically.

## Gates

A stage or feature is done only when its exit gate in the build path passes. In addition:

- **Lab work:** domain tests cover the task, every wrong-use scenario and the "invalid never passes" rule; scene presets open valid setups, `-reject` presets fail, demos never pass; i18n parity holds; typecheck, lint, tests and build pass; the feature was exercised in a browser (WebGL) with screenshots.
- **Content work:** generator link, scene and source checks pass; every new source URL was opened (or confirmed and logged when the publisher blocks automated access); examples recomputed; languages agree on every number; capability statements match the current lab.

## Reporting

At the start: current stage, companion skills available, and the gate you are working towards. At the end: what changed, how it was verified (commands, screenshots, frames), the gate's state, and what remains unverified or manual (domain binding, search-console submission, browser-only source checks, expert review). Ask the user only when a choice changes the outcome and cannot be settled from the code or conventions.
