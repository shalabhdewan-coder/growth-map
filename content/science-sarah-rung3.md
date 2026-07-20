# Sarah — Science — Rung 3 "Animals & classification"

**Blended grade-equivalent target:** SG MOE Primary Science "broad groups of living things" outcome + general CBSE Class 6 Science vertebrate/invertebrate framing (widely standard, general knowledge — no PDF captures actual CBSE Class 6 chapter body, see gap note below) + SOF NSO sample-paper clue-based question style.

## Source check

**`sources/science/moe-primary-science-syllabus-2008.pdf`** (p.18) — verified outcome: *"Recognise some broad groups of living things — plants (flowering, non-flowering), animals (birds, fish, insects, mammals), fungi (e.g. mushroom, yeast), bacteria."* Note this is a **different, higher-level grouping** than vertebrate/invertebrate + 5 vertebrate classes (mammal/bird/fish/reptile/amphibian) — MOE's primary syllabus doesn't use "vertebrate/invertebrate" terminology at all. The existing rung content uses the vertebrate/invertebrate framing, which is standard CBSE/general-knowledge scope for this age but wasn't literally in either sourced PDF (NCERT Class 6/7 "Curiosity" PDFs in `sources/science/` are confirmed truncated to ~18-20pp front matter only via `pdfinfo` — no chapter body available, same issue as the NCERT Class 3/5 EVS files noted in the maths reference doc).
**Decision:** kept the existing vertebrate/invertebrate framework (it's genuinely standard scope, not fabricated-as-sourced), and **added** the MOE-sourced plants/animals/fungi/bacteria broad-group fact as a distinct, honestly-cited addition rather than conflating the two systems.

**`sources/science/sof-nso-iso-sample-class5.pdf`** (2pp, confirmed real sample questions, not just syllabus pattern) — Q4 verified verbatim: *"Read the given statements regarding an animal X and select the option that correctly identifies it. • It has flippers. • It has lungs for breathing. • It has webbed feet."* This clue-based multi-statement identification format is the direct model for the two new `sc` entries below.

## What was added

- **1 new fact:** "Besides plants and animals, name two other broad groups of living things?" → fungi, bacteria (MOE p.18, cited above).
- **2 new `sc` entries** in the SOF NSO clue-statement style (3 clues → identify the group), replacing "just recall the definition" with genuine multi-fact reasoning:
  - flippers/lungs/webbed feet → water-adapted mammal or bird, explicitly contrasted against "why not a fish" (gills vs lungs) — same reasoning shape as the sourced SOF Q4.
  - scales/lungs/leathery eggs on land → reptile, contrasted against a fish (which also has scales) — tests that a single shared feature (scales) isn't enough to classify, must combine clues.
- **2 new `method` entries** (was 1): a whale mammal-vs-fish feature checklist (extends the existing bat-vs-bird "fair feature" logic to a second classic misconception), and a "why isn't habitat a fair classification clue" investigation (explicit reasoning about *why* body-covering, not habitat, is the valid classification variable — ties fair-test thinking into classification, matching the rung's Olympiad-reasoning brief).

## Code change

Same mechanism as rung 1 — `method` now an array of pairs, `P()`-picked.
