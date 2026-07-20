# Sarah — Science — Rung 1 "Living & non-living"

**Blended grade-equivalent target:** SG MOE Primary Science lower block ("Diversity of Living and Non-Living Things" theme) / general CBSE Class 3-4 EVS framing.

## Source check — what was actually readable

**`sources/science/moe-primary-science-syllabus-2008.pdf`** (58pp, confirmed real full syllabus content via `pdftotext`) — the "Diversity" theme (pp.17-19) is directly on-topic:
- Learning outcome: *"Describe the characteristics of living things — need water, food and air to survive; grow, respond and reproduce."*
- Learning outcome: *"Recognise some broad groups of living things — plants (flowering, non-flowering), animals (birds, fish, insects, mammals), fungi (e.g. mushroom, yeast), bacteria."*

**`sources/science/ncert-class4-evs-looking-around-ch01..27.pdf`** (confirmed real, full 27 chapters) — checked: this is NCERT's *integrated* EVS ("Looking Around"), organised by narrative/social-studies themes (family journeys, food, water, occupations), **not** a discrete "living vs non-living" chapter. Grepped all 27 chapters for "living thing", "non-living" etc. — zero hits. So this source does not directly apply to rung 1; noted honestly rather than force-fitting a citation.

## Verdict on current content

`SCI_SARAH[0]` facts (*"food/energy, water, air"*, *"grow, reproduce or respond"*) already match the MOE Diversity learning outcome almost verbatim — **well-calibrated, no change needed** to vocab/facts/lab.

## What was added

- **2 new `sc` (think-it-through) entries**, testing the same MOE learning outcome from two different misconception angles instead of just one:
  - A mushroom (no eyes/legs/movement) is still alive — tests that movement isn't required for life, and links forward to MOE's "fungi" broad-group fact used at rung 3.
  - A responsive, "growing-battery" toy robot dog — tests that movement/response alone (without real reproduction/growth) isn't enough; a common misconception at this age.
- **2 new `method` entries** (was 1) — fair-test/observation framing consistent with the rung's existing celery-in-dark-cupboard item: a seed-needs-water fair test, and a snail-response observation. `ws_science_sarah` now picks one at random per worksheet (see code-change note below) instead of always the same one.

## Code change made (applies to all 12 Sarah science rungs, not just 1-4)

`ws_science_sarah` previously hardcoded `method[0]`/`method[1]` because `method` was stored as a flat `[question, answer]` pair. To let `P()` pick randomly among several method entries (matching how `sc` already works), `method` is now stored as an **array of `[question, answer]` pairs** on every rung (rungs 5-12 got a mechanical bracket-wrap of their existing single entry — no content change there, `P()` on a 1-item array always returns that item, so this is a no-op for untouched rungs). The generator now does `const me=P(L.method); items:[me[0]]; ans: me[1]`.
