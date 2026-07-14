# Sarah — Maths — Rung 1 "Tables & 2-step"

**Blended grade-equivalent target:** SG MOE Primary Maths P3 / CBSE-NCERT Class 3 / Saxon Math 5/4 core scope (per plan's rung-map table).

## Source check — what was actually readable

**`sources/maths/ncert-class3-maths-magic.pdf`** (NCERT *Maths Mela*, Textbook of Mathematics for Class 3, ISBN 978-93-5292-816-3) — `pdfinfo` confirms this file is **only 16 pages** (front matter: cover, foreword pp.iii-iv, "About the Book" pp.v-vi, NSTC committee, textbook dev team, acknowledgements, national anthem, **Contents p.[vi]**, Constitution preamble). The actual chapter body (pp.1-191) was not captured during Phase‑1 sourcing. So the only thing I can cite from this file is the verified table of contents and the "About the Book" framing text — I cannot cite specific exercises/number ranges from inside any chapter because those pages aren't in the file.

Verified Contents (p.[vi] of the PDF), relevant chapters for rung 1:
- Ch.3 "Double Century" p.16 — numbers to ~200 / place value
- Ch.6 "House of Hundreds–I" p.64 and Ch.9 "House of Hundreds–II" p.117 — 3-digit place value, addition/subtraction with regrouping
- Ch.7 "Raksha Bandhan" p.82 — thematically a multiplication/equal-grouping chapter (rakhi packets)
- **Ch.8 "Fair Share" p.107 — a dedicated division / equal-sharing chapter**
- Ch.12 "Give and Take" p.150 — addition/subtraction word-problem chapter
- Ch.14 "The Surajkund Fair" p.177 — money/word-problem context

Verified "About the Book" text (pp.v-vi): the book is built around "whole numbers and operations," is meant to move learners from "bare problems" to "problem situations," and explicitly uses shopping/sharing/travel real-world contexts for word problems — confirming that money-context and equal-sharing word problems are core Class‑3 material, not an enrichment add-on.

**`sources/maths/moe-primary-maths-syllabus-2013.pdf`** (Singapore MOE Primary Mathematics Teaching and Learning Syllabus, 2013 ed.) — `pdftotext` search + the doc's own cover page confirm: **"This document contains general information about the primary mathematics curriculum and the content for Primary Mathematics at Primary 1 only. The content for Primary 2 to 6 will be updated accordingly."** So there is no P3-specific content table in this file at all — only (a) general Primary-level aims (p.7: "acquire mathematical concepts and skills for everyday use," "develop thinking, reasoning ... through a mathematical approach to problem-solving") and (b) the detailed P1 strand breakdown (pp.33-35): Whole Numbers sub-strand explicitly separates "3.1 concepts of multiplication and division," "3.3 multiplying within 40," "3.4 dividing within 20," "3.5 solving 1-step word problems involving multiplication and division." This establishes the strand *shape* (mult. and div. always taught and word-problemed together as a fact-family, starting 1-step) that P3 (two implementation-years later, per the 2013→2015 rollout table on p.6 of the doc) extrapolates from — but the specific P3 number ranges/2-step language below are **general knowledge, no source file** (this doc doesn't reach P3; the Singapore Primary Mathematics P3 textbooks in `sources/maths/` are the unauthorized copies and are off-limits).

**Saxon Math 5/4 core scope** — **(general knowledge, no source file)** — per task's hard constraint, not opened. Saxon 5/4 drills multiplication/division fact fluency through the ×12 table and mixes arithmetic with two-step "mixed practice" word problems from the first third of the book — used here only to justify pushing the warm-up fact range to ×12 rather than capping at ×10.

## What this rung should test

1. **Multiplication & division fact fluency**, both directions (a×b and its inverse a÷b), factors from 3 up through 12 (Saxon-hardest end of the blend; NCERT/SG P3 baseline is 2-10, already covered by rungs below this range).
2. **Two-step word problems** — the rung's own name is "Tables & 2-step," so both major two-step shapes that Class-3-level books use should appear, not just one:
   - multiply-then-subtract ("buy N packets of Q each, give some away — how many left")
   - **divide-then-multiply / share-then-count ("Fair Share" style — share T items equally among g people, then ask about n of those people's totals)** — this shape existed nowhere in the word-problem generator before my edit (see below), even though NCERT dedicates a whole chapter to it and MOE's strand list always pairs multiplication with division.
   - money change (buy, pay with a note, find change) — already present, matches NCERT's shopping-context framing.
3. **Bar-model intro** (sum-and-difference, "cmp" type) — appropriate first bar-model shape for this stage.
4. **Simple arithmetic sequences** (skip-counting-style patterns) — appropriately gentle for rung 1; geometric/triangular/quadratic patterns are reserved for later rungs.
5. **Spot-the-mistake** on basic fact/2-digit-addition errors — on-theme with "Tables."
6. **Gentle ⭐ challenge** (CHAL tier 1) — counting/multiplication-tables flavoured, not yet olympiad-heavy (that's woven in from higher rungs per the plan).

## Verdict on current knobs/generators

`LEVELS.sarah.maths[0] = {n:'Tables & 2-step', wm:12, s:2, bar:'cmp', patt:'arith', chal:1}` — **mostly well-calibrated already**:
- `m_warm` defaults to ×3-12 fact fluency (factors 3-12) for both multiplication and division when no frac/longmul/mul2 flag is set — correctly matches the blended-hardest target described above. (Note: the `wm:12` knob itself is dead/unused in code — `m_warm` hardcodes the same `R(3,12)` range regardless — so the *value* happens to already match what `wm` implies, but the knob doesn't do anything; left as-is, out of scope for a content-calibration task.)
- `m_bar('cmp')`, `m_pattern('arith')`, `m_mistake` easy bank, `CHAL[1]` are all correctly pitched for rung 1 — no change needed.
- **Gap found:** `m_words(L)` for `L.s<3` (rungs 1-2) only ever generated the multiply-then-subtract "packets" word problem for its second block — no division-based two-step problem existed anywhere in the word-problem set, despite (a) the rung being literally named "Tables & 2-step" (tables = both × and ÷ fact families), (b) NCERT Class 3 devoting a full chapter ("Fair Share," p.107) to equal-sharing/division word problems, and (c) MOE's own strand structure always listing multiplication and division word problems together, never multiplication alone. This is the one real content gap worth fixing at this rung.

## Change made

In `m_words(L)`, the `L.s<3` branch now randomly alternates (50/50) between the existing multiply-then-subtract "packets" problem and a new divide-then-multiply "Fair Share" problem (share T items exactly among g people, then ask how many were used/eaten by n of those people). Numbers are chosen so the division is always exact (no remainders — remainders are not yet a rung‑1 topic). This does not change `m_words`'s return shape (`{items,a}`... contributing to the overall `{items,a}`-per-block shape `ws_maths_sarah` expects) and does not touch any other rung's behavior (the `L.s<3` branch is shared by rungs 1-2 only, and rung 2 already exercises the packets version plenty via repeated worksheet generation — the fix genuinely adds coverage rather than removing any).
