# Sarah — Science — Rung 2 "Plants"

**Blended grade-equivalent target:** SG MOE Primary Science "Plant System" (Systems theme, pp.23-24) + "Energy Forms and Uses" photosynthesis outcome (p.45).

## Source check

**`sources/science/moe-primary-science-syllabus-2008.pdf`** — two directly relevant, verified outcomes:
- Plant System (p.23-24): *"Identify the different parts of plants and state their functions — leaf, stem, root"*; *"Identify the parts of the plant transport system... leaf, stem, root"*; transport is explicitly framed as "tubes that transport food and water" (p.25, Human System comparison table).
- Energy theme (p.45): *"Investigate the requirements (water, light energy and carbon dioxide) for photosynthesis (production of sugar and oxygen)"*; *"Recognise that food produced by plants becomes the source of energy for animals."*

**NCERT Class 4 EVS ch01-27** — checked again for plant-biology content; none of the 27 (narrative/social-studies EVS, see rung 1 note) contain a dedicated photosynthesis/plant-parts chapter, so not cited here either.

## Verdict on current content

Existing vocab (photosynthesis, nutrients, oxygen) and facts (roots take in water, leaves make food, sunlight+water+CO2, oxygen given out) match the MOE outcomes closely — **already well-calibrated**, kept as-is.

## What was added

- **2 new `sc` entries**, both grounded in the MOE "plant transport system" outcome rather than repeating the existing phototropism item:
  - Transpiration: a sealed plastic bag over a leaf fogs up with water — tests understanding that leaves release water drawn up by the roots.
  - Cactus stem-photosynthesis: tests that photosynthesis happens "in green parts," not leaves specifically — a real adaptation, and a gentle stretch beyond the base fact.
- **2 new `method` entries** (was 1, alongside the existing celery-transport experiment): a dark-cupboard-vs-windowsill fair test (parallels rung 1's method item but here specifically measuring plant food-making/paling, not "is it alive"), and a roots-vs-leaves watering investigation — both are direct fair-test extensions of the MOE plant-transport-system outcome.

## Code change

Same as rung 1: `method` field converted from flat `[Q,A]` to array-of-pairs so `P(L.method)` gives variety; see rung 1 note for the full mechanism (single code change covers all 12 rungs).
