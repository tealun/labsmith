# Adapting the pattern to another trade or product

The caliper lab is one instance of a general pattern: **an instrument acts on a subject to produce a reading, under conditions that make the reading valid or not, judged against a specification.** Map your trade onto that pattern before writing any code (stage S0).

## 1. Concept map

| Concept | Caliper lab | Torque wrench | Digital multimeter | Piston pipette |
| --- | --- | --- | --- | --- |
| Instrument | vernier / dial / digital caliper | click / dial / electronic torque wrench | handheld DMM | adjustable-volume pipette |
| Subject | shaft, bushing, blind part | bolted joint | circuit on a training board | liquid and receiving vessel |
| Task (feature) | outside diameter, bore, depth | tighten to a specified torque | measure voltage, current, resistance | dispense a set volume |
| Reading | length in mm | torque in N·m | V, A, Ω | volume in µL (gravimetric check in mg) |
| Readout types | vernier, dial, digits | click, dial, digits | digits, bar graph | dial counter, digits |
| Validity conditions | contact, squareness, zero, clean faces, force | handle position, angle, extension, speed, preload | range, probe placement, function setting, lead condition | pre-wetting, angle, immersion depth, tip seating, speed |
| Specification | drawing limits | torque specification ± tolerance | expected value ± tolerance | nominal volume ± tolerance |
| Typical wrong uses | tilt, unchecked zero, chip, force, off-centre, not seated | holding the head, extension without correction, jerking, over-clicking | wrong function or range, probes in the wrong jacks, measuring current in parallel | no pre-wetting, tilted, too deep, pressing past the first stop |
| Standards to look up (verify edition) | ISO 13385-1, ISO 286-1, ISO 14253-1, ISO 1 | ISO 6789 series | IEC 61010 series (safety), maker specifications | ISO 8655 series |

Fill the same table for your trade with a domain expert. Every wrong use needs a source (standard, verification procedure, training material or documented shop practice) and a known direction of error.

## 2. Steps

1. **Find the expert and the sources.** Identify the standards, verification procedures and training curricula of the trade; list them in the planning docs with editions. Treat every number as `draft` until the expert reviews it.
2. **Define the reading physics.** How does the instrument turn the subject's property into a reading? Write it as a domain function with its inputs (geometry, settings, pose) and outputs (value, validity).
3. **List the validity conditions.** Each condition becomes a validity value or a scenario; decide which ones the learner can cause by hand and which ones are demonstrations.
4. **Choose the MVP slice.** One instrument, one task, three to five subjects (one out of specification), two to four wrong uses.
5. **Check safety.** In trades where a wrong action can injure (electrical, pressure, chemicals, machinery), the lab must never present an unsafe action as acceptable, demonstrations of unsafe acts must say so plainly, and the guide must point to the governing safety rules. Get expert sign-off before publishing.
6. **Map the content.** Draft the categories and the first five articles for this trade (S7) from the concept map: basics, reading the instrument, the first task, wrong uses, teaching and assessment.

## 3. What changes and what does not

- **Changes:** geometry and contact (or the equivalent physics), the instrument's readout behaviour, the scenario list, units and specifications, the keyword matrix, the articles.
- **Does not change:** the layering, the "invalid never passes" rule, commands and rejections, reversible camera and smooth transitions, scene presets and their tests, the clip pipeline, the generator and its checks, the content lifecycle, the security baseline.
