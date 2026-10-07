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
