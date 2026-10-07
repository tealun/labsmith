# Lab engineering

Lessons from building a caliper lab (React + three.js/R3F, framework-free domain, zustand store). They generalise to any instrument or procedure lab.

## 1. Layers

| Layer | Owns | Must not |
| --- | --- | --- |
| `domain` (framework-free) | instrument and part definitions, geometry, contact solving, joint limits, readings, validity, tolerance, error scenarios, commands/reducer, share-state codec | import UI, render or browser APIs |
| `app` (store, URL state) | dispatching commands, view state (camera mode, panels, drag state), persistence of conveniences | compute measurements |
| `render` | meshes, materials, camera moves, pointer picking, anchors | decide validity or values |
| `ui` | panels, cards, guidance, i18n | call the reducer directly |

- Commands in, state out: `dispatch(command) → { state, rejection? }`. Rejections carry a reason code (e.g. `scenario_not_for_task`) shown through i18n.
- Enforce the layering with lint rules (no imports from `render`/`ui` into `domain`), not with comments.
- Keep a derived frame in state (where the part sits, its axis, the instrument offset, tilt) so render and camera read the same placement the domain used.

## 2. Measurement truth

- One internal unit (mm, rad). Scene scale 1 unit = 1 mm. Convert only at the display edge (mm/inch, display resolution).
- Model the instrument's real resolution and display behaviour (vernier coincidence, dial revolutions, digital ORIGIN/ZERO/ABS/HOLD) in the domain, with tests that read known values.
- Validity is an explicit enum (`valid`, `not_in_contact`, `misaligned`, `out_of_range`, plus task-specific ones such as `off_center`, `not_seated`). Tolerance is evaluated only on `valid`; otherwise `not_evaluated`. Near the limits consider an `indeterminate` band.
- Each task (outside, bore, depth, …) has its own contact solver and opening limits; snapping happens only when geometrically seated.
- Parts: nominal and actual sizes, tolerance limits, material; include parts made out of tolerance on purpose (high and low) so learners practise rejection.
- Mark instrument and part data `draft` until reviewed by a metrology or trade expert.

## 3. Wrong-use demonstrations

- Per task, list the mistakes practitioners actually make (from verification procedures, standards, training material and shop experience). Caliper example: outside — tilt, zero not checked, chip on a face, excessive force; bore — off centre (reads small), tilt along the axis (reads large), zero, chip; depth — beam end not seated (reads large), tilt, zero, chips at the bottom (reads small).
- A demo stages itself so the effect is visible at once (close the jaws, open the blades, extend the rod) and never passes tolerance.
- Pair each demo with a guidance card: what is wrong, why the reading moves, one action that corrects it. Closing the card ends the demo; the demo button reopens it.
- Fly the camera to the fault after a short settle delay, and back to the previous view when the demo ends.
- Keep the scenario list per task in the domain (`SCENARIOS_FOR[task]`) and reject scenarios that do not belong to the current task.

## 4. Interaction

- **Continuous motion.** Pose, opening and rotation glide (~0.6 s) on programmatic jumps; user drags follow the pointer exactly. Parts grow or swing in when the task changes.
- **Drag conflicts.** Small controls on a draggable body (lock screws, bezels, buttons) need generous invisible grab targets and `stopPropagation` on pointer over/out/down so the body does not start dragging.
- **Hover only on the canvas.** Raycast hover only when the pointer target is the canvas element; HTML panels over the canvas must not trigger part tooltips.
- **Quiet modes.** In the readout view and during demos, suppress hover tooltips, part cards and quick actions; they cover what the learner must read.
- **Touch.** Long-press with a movement slop replaces hover; a tap elsewhere closes cards.
- **Quick actions on the active part.** Translucent buttons near the pointer: seat/lift, task buttons for the features the part offers, wrong-use entry, part info.
- **Parts tray.** Real parts laid out on the floor with flat name labels; click to swap; the tray recentres for tasks that move the stage.
- **React Compiler / hooks lint.** Do not mutate refs or three objects during render; keep cross-component controllers in module scope with setter functions; prefer JSX materials over mutating `material` props.

## 5. Camera

- Every view toggle (e.g. "face the readout") is reversible: remember the view before, animate there and back, and show the toggle as active.
- No automatic camera jumps the learner did not ask for, except demo focus, which returns afterwards.
- Frame by the space panels actually leave (insets from navigation, panel, toolbar). While a panel is being dragged, freeze the insets and glide once on drop so the scene does not swim.
- Stage changes (an upright depth task) rotate a stage group, not the camera rules; the floor and tray follow the stage.

## 6. Layout

- Fixed navigation; a draggable control panel that docks by drop position (beside navigation, top, bottom — folding to the bottom edge — side edges, or floating). Test the corner cases: bottom-left under the navigation must dock, not snap back.
- Persist only viewer conveniences (panel position) in local storage, wrapped in try/catch.
- Phones: fixed docking, bottom bar navigation, collapsible panel.

## 7. Readability of instruments

- Model scales so the reading is legible from the readout view; bevel vernier plates to a near-zero edge to reduce parallax, and keep digits clear of bevel creases.
- Textures for scales are generated (canvas) from the domain's scale definition, so a scale change cannot drift from the reading logic.

## 8. i18n

- Source locale (e.g. en-US) plus every target locale, with a parity test that fails on missing keys.
- Locale from the URL path for crawlable language versions (`/` and `/zh/`); switching language updates path, `lang`, title and description. Share links point at the root so the receiver's language wins.

## 9. Verifying 3D UI

- Use Playwright with a software GL (`--use-gl=swiftshader --enable-unsafe-swiftshader`); it is slow, so wait on explicit markers rather than fixed sleeps.
- Expose a harmless marker when the opening camera move ends (e.g. `html[data-intro="done"]`) for scripts and end-to-end checks.
- Check: the reading on a known part, each demo's reading and guidance, view toggles there and back, panel docking, quick actions, language switch, light/dark and phone widths.
- A preview server may be stopped by the environment; restart it rather than trusting stale results.
