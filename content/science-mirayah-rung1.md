# Mirayah — Science — Rung 1 "Is it alive?"

**Blended grade-equivalent target:** pre-formal-science (age 6) foundation for the same concept MOE later formalises at Lower Block P3-4 ("Diversity of Living and Non-Living Things").

## Source check

**`sources/science/moe-primary-science-syllabus-2008.pdf`** (58pp, confirmed real via `pdftotext`) — Singapore's Primary Science syllabus is explicit that Science as a discrete subject starts at **Primary 3** (p.9-10 overview table: "Lower Block (Primary 3 and 4)" / "Upper Block (Primary 5 and 6)" — there is no P1/P2 tier). So there is no age-6-specific MOE content for this rung. The nearest real content is the Lower Block (P3-4, i.e. one age-band above Mirayah) "Diversity of Living and Non-Living Things" learning outcome (p.17): *"Describe the characteristics of living things — need water, food and air to survive; grow, respond and reproduce."* This is the same core idea `SCI_MIRAYAH[0]`'s facts already test, simplified further for age 6.

**`sources/science/sof-nso-iso-sample-class1.pdf`** — Section 2 syllabus line lists "Living and Non-living Things" as a Class-1 topic, and Q4 (odd-one-out, natural vs man-made) is the only text-based question on this theme, but it's image-based (can't be lifted as text) and tests natural-vs-man-made, not quite living-vs-non-living. Not directly citable as a question; confirms the topic is age-appropriate for Class 1 (age 6) but the sample paper doesn't give usable text content beyond the syllabus line itself.

## Verdict

Existing vocab/facts/lab already match the MOE Lower Block "characteristics of living things" learning outcome at simplified age-6 level — no change needed there.

## What was added

- **`method[0]`→`P(L.method)` bug fixed** (see code-change note below — applies to all 10 Mirayah rungs).
- **2 new `sc` entries** (was 1, now 3): a toy car (batteries, not food) and a tree (alive but doesn't move) — both test the same "needs food/water/air, grows" definition from two new misconception angles a 6-year-old actually encounters (toys that move; plants that don't).
- **2 new `method` entries** (was 1, now 3): a "plant makes its own food" guess, and a "point to one living + one non-living thing around you" observation task.

## Code change made (applies to all 10 Mirayah science rungs)

`ws_science_mirayah` previously hardcoded `L.method[0]`/`L.method[1]` because `method` was stored as a flat `[question, answer]` pair — the exact same bug the Sarah-ladder pass found and fixed in `ws_science_sarah`, but living in an independent code path (`ws_science_mirayah` is a separate function from `ws_science_sarah`, so it had to be checked and fixed independently). Fixed: `method` is now stored as an **array of `[question, answer]` pairs** on every rung (rungs 6-10 got a mechanical bracket-wrap of their existing single entry — no content change, since `P()` on a 1-item array always returns that item, a no-op for untouched rungs). Generator now does `const me=P(L.method); items:[me[0]]; ans: me[1]`.
