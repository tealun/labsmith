# <Project> — runtime contract

| Version | Date | Notes |
| --- | --- | --- |
| 0.1.0 | YYYY-MM-DD | Draft for S0 |

The domain layer is the only source of measurement truth. UI and render dispatch commands and draw state.

## 1. Units and scale
- Internal units: (e.g. length mm, angle rad, torque N·m)
- Scene scale: (e.g. 1 unit = 1 mm)
- Display conversions: (e.g. mm/inch) — only at the display edge

## 2. Instrument definition
| Field | Meaning | Example |
| --- | --- | --- |
| id, revision, status | identity; `draft` until expert review | |
| range | measuring range | |
| resolution | smallest display step | |
| readout | vernier / dial / digital / … and its behaviour (zero, hold, units, modes) | |
| geometry | dimensions needed by contact and rendering | |
| joints | moving parts, limits | |

## 3. Subject (part) definition
| Field | Meaning |
| --- | --- |
| id, revision, status | |
| features | each with kind (task), nominal, actual, tolerance (lower, upper) |
| geometry | what the contact solver needs |
| material | for rendering and articles |
Include at least one subject outside its specification.

## 4. Tasks
For each task: inputs (pose, joint positions, settings), contact rule, opening/positioning limits, reading formula, snapping rule.

## 5. Validity
Enumerate: `valid`, `not_in_contact`, `misaligned`, `out_of_range`, and task-specific values. Rule: tolerance is evaluated only when `valid`; otherwise `not_evaluated`.

## 6. Tolerance
Result values: `pass`, `fail_low`, `fail_high`, `indeterminate` (optional uncertainty band), `not_evaluated`.

## 7. Wrong-use scenarios
| Task | Scenario | Staging | Effect on reading | Validity | Source |
| --- | --- | --- | --- | --- | --- |
Demonstrations never pass tolerance.

## 8. Commands and rejections
List commands (select instrument/subject/task, drive joint, move/rotate, seat/lift, set mode, set scenario, restore shared state) and rejection reasons. A rejected command leaves state unchanged.

## 9. Shared state
Format, schema version, size limit, what is included, how invalid input is rejected.
