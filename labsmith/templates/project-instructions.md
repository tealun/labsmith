# <PROJECT> — agent instructions

<One line: what the lab is, stack.> Planning docs: `docs/`. Code, identifiers and the i18n source locale are <language>.

## Companion skills
This project expects the Labsmith skill and the Tealun Skills (planner, tasker, bugfixer, auditor, evolver): https://github.com/tealun/Tealun-Skills. If any is missing, say so at the start of the session and follow Labsmith's fallback protocols.

## Rules that must not be broken
- **Stack is locked.** Only the libraries and versions in `<stack doc>`. Changes need an approved decision card.
- **Layering.** `src/domain` is framework-free and the only source of measurement truth. UI/render dispatch commands through the store; never call the reducer directly. Lint enforces this.
- **Units.** <internal units and scene scale>.
- **i18n.** No hard-coded user-facing strings; every key in all locales; a test enforces parity.
- **Metrology.** An invalid setup never passes. Instrument and subject data stay `draft` until expert review.
- **Content.** Articles follow `<authoring guide>`; claims need sources; lab changes follow the content sync table.

## Content and media pipeline — start here
Follow `<pipeline runbook>`. Every session: pull, `<doctor command>`, `<status command>`; finish with the board free of errors, the gate green, committed and pushed.

## Reenactments and videos (when the lab has them)
- Scripts: `<scripts dir>`; generated ones from `<generator command>` (a changed script gets a new version). Every beat has `expect`; the script test passes before any render.
- Pipeline: build → preview → sparse draft render and look at the frames → full render → publish (versioned keys, captions on the site, manifest with script hash) → regenerate the guide → commit manifest and pages.
- The guide generator fails on a stale video and lists articles waiting for one. Article → script mapping: `<where>`.
- Media credentials only in the ignored `.env` / environment; never printed or committed.

## Before every commit
```bash
<gate command, e.g. npm run typecheck && npm run lint && npm run test && npm run build>
```

## Current stage
<S0–S8 from Labsmith's build path, gate state, evidence link>

## Where things are
- Product scope: 
- Runtime contract: 
- Experience spec: 
- Stack: 
- Guide generator and content guides: 
