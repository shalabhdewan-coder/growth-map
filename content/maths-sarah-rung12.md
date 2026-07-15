# Sarah — Maths — Rung 12 "The quantitative spike" (top rung)

**Blended grade-equivalent target:** AMC 8 entry-level / proof-lite reasoning (per plan's rung-map table). This is the top of Sarah's 12-rung maths ladder — reached after climbing, realistically closer to age 10–11 in practice, not "age 8."

## Source check — what was actually read

**`sources/maths/amc8-compendium-1985-2026.pdf`** (307 pages, official AJHSME/AMC 8 archive, clean source per task constraints) — extracted with `pdftotext -layout` and read problems 1–6 (occasionally through 7–9 where numbers weren't image-rendered) from **five years spread across the archive**:

- **1999 AMC 8**, Problems 1–9 (pp. 109 in the compendium's own TOC). Fully text-readable. Key calibration points:
  - **P2**: "What is the degree measure of the smaller angle formed by the hands of a clock at 10 o'clock?" — pure spatial/arithmetic reasoning, no algebra.
  - **P3**: "Which triplet of numbers has a sum NOT equal to 1?" — arithmetic checking.
  - **P5**: rectangular garden 60×20 ft re-fenced into a square of the same perimeter — "how much bigger is the new area" — perimeter/area reasoning, no formal equation-solving needed.
  - **P6** (the model for the new `m_logic` generator below): "Bo, Coe, Flo, Joe, and Moe have different amounts of money. Neither Joe nor Bo has as much money as Flo. Both Bo and Coe have more than Moe. Joe has more than Moe, but less than Bo. Who has the least amount of money?" — a pure deduction/ordering puzzle. Solving it: Flo > Bo > Joe > Moe, and Coe > Moe (but Coe's exact rank vs. the others is never pinned down) — yet Moe is still provably the unique minimum. This is genuine "proof-lite" reasoning: you don't need to fully solve the puzzle to answer it rigorously, you need to justify why the answer holds despite incomplete information.
  - **P7**: highway milepost interpolation ("three-fourths of the way from the third exit to the tenth exit") — proportional reasoning.

- **2008 AMC 8**, Problems 1–7 (compendium p. 173). Fully text-readable.
  - **P1**: "Susan had 50 dollars... spent 12 on food and twice as much on rides. How many dollars did she have left?" — this is literally rung-1-level arithmetic (multiply, subtract). Important calibration point: **AMC 8 Problem 1 is not hard.** It is deliberately as easy as a Class-3/P3 two-step word problem.
  - **P3**: "If February is a month that contains Friday the [13th], what day of the week is February 1?" — modular/day-of-week reasoning (no formal algebra).
  - **P5**: odometer palindrome/average-speed problem — rate reasoning.
  - **P2, P4, P6, P7**: increasingly lean on a supplied figure (cipher table, nested triangles, grid of squares, a numeric substitution) — from Problem ~4 onward, diagrams start doing real work, which is a first calibration point about what a printable, figure-free worksheet generator *can't* faithfully reproduce (see "Honest limits" below).

- **2013 AMC 8**, Problems 1–7 (compendium p. 207). Fully text-readable, and the single most useful year for this rung.
  - **P1**: Danica has 23 model cars, wants rows of exactly 6 — smallest number of additional cars needed. Pure "next multiple of 6" reasoning.
  - **P2**: "50% off, half-pound packages for $3... what is the regular full-pound price?" — percent + unit reasoning, no algebra notation required.
  - **P4**: 8 friends split a bill; 7 of them each pay an extra $2.50 to cover the 8th's share — find the total bill. This is *algebra-flavored* (it's really "7 × 2.50 = one person's share, and total = 8 shares") but is solved by reasoning about shares, not by writing `8x = ...` — a good example of why AMC 8 rewards arithmetic cleverness over symbol manipulation.
  - **P5** (the model for the new mean/median word problem below): "Hammie is in [6th] grade and weighs 106 pounds. Her quadruplet sisters are tiny babies and weigh 5, 5, 6, and 8 pounds. Which is greater, the average (mean) weight of these five children, or the median weight, and by how many pounds?" — mean = 26 lb, median = 6 lb, mean greater by 20. Clean, no-algebra, entirely arithmetic + comparison — and (checked against the rest of the file, see below) this exact skill — mean vs. median — did not exist anywhere else in the whole ladder.
  - **P6**: a multiplication-pyramid puzzle (each box = product of the two above it) — depends on a supplied figure.

- **2019 AMC 8**, Problems 1–6 (compendium p. 246). Partially readable — several problems (P1, P2, P4) had their numeric values stripped by the PDF's math rendering (LaTeX-as-image), leaving only sentence skeletons ("Ike and Mike go into a sandwich shop with a total of ___ to spend..."). Still useful for calibration of *problem shape*: P3 (order three fractions least to greatest, no calculator) and P5 (interpret a distance-time graph matching a tortoise-and-hare story) are both readable in full and confirm the same pattern — reasoning about relationships, not equation-solving.

- **2024 AMC 8**, Problems 1–4 (compendium p. 280). Similarly, numeric literals were image-rendered and stripped by text extraction (P1 "ones digit of [some expression]", P2 "value of this expression in decimal form" — expression itself missing). P3 (four squares of increasing size, alternating white/gray, area of visible gray region) and P4 (Yunji's sum of consecutive integers with one number missing, incorrect sum is a perfect square) are readable in full — both are multi-step reasoning puzzles, not formal-algebra problems.

**Overall calibration takeaway (this is the single most important finding for this rung):** across all five years, **Problems 1–3 are consistently easy, arithmetic-only, and no harder than a good rung-8/9 problem on this ladder** (2008 P1 literally *is* rung-1-level). The genuine step-up that defines "AMC 8 entry-level" difficulty doesn't come from harder algebra — it comes from problems that need **you to reason about relationships (ordering, mean vs. median, proportional shares, modular/day-of-week cycling) rather than execute a memorized procedure**, and from problems where you must **justify an answer under incomplete information** (1999 P6 is the clearest example: you can answer confidently without fully solving the puzzle). That is the "proof-lite" flavor the plan calls for, and it is a different skill from solving `3x + 1 = 22`.

**`sources/maths/moe-primary-maths-syllabus-2013.pdf`** — checked per task instructions; as rung 1's note already established, this file only contains P1-specific content scope, nothing that reaches this rung. Not useful here beyond confirming (again) that it has no P4–P6 tables to cite from.

**`sources/maths/ncert-class6-ganita-prakash.pdf`** — checked the TOC; confirmed (per rung-1's finding) this file is front-matter/TOC only, no chapter body. Not useful for this rung's content (Class 6 pace is well below AMC 8 entry difficulty anyway — this rung is explicitly *above* NCERT/MOE's own scope, so the gap not mattering here is expected, not a blocker).

**Saxon 8/7-with-Prealgebra and Beast Academy 5D** — per the hard constraint, not opened; used only from general knowledge to sanity-check that formal single-variable linear-equation solving (`mx + c = value`) is squarely Beast-Academy-5D/Saxon-8/7 territory — i.e. appropriate as *one ingredient* of this rung, but not sufficient on its own to earn the "quantitative spike" label, because it's the same skill already present at rungs 9–11.

## What this rung should actually contain — honest assessment

Given the above, the honest shape of "AMC 8 entry-level / proof-lite" for an advanced ~10–11-year-old is:

1. **Keep the existing fact-fluency, multi-step word problems, bar models, patterns, spot-the-mistake, and linear-equation algebra** — these are correctly pitched (Beast Academy 5D / lower-secondary territory) and match roughly AMC 8 Problems 1–3 difficulty (confirmed against 2008 P1, 2013 P1–P2 above). They should stay; removing them would make the rung *narrower*, not harder.
2. **Add genuine deduction/ordering reasoning** — this was completely absent from the ladder before this task. Modeled directly on 1999 AMC 8 Problem 6.
3. **Add mean-vs-median reasoning** — also completely absent before this task (confirmed by grepping the whole file for "average"/"mean"/"median" — zero hits). Modeled directly on 2013 AMC 8 Problem 5.
4. **Keep the ★ Challenge tier (`CHAL[6]`) and ★★ Prove-it block as-is** — on inspection, `CHAL[6]`'s three existing entries (smallest number leaving remainder 1 mod 2–6; number of ways to make ₹10 from ₹1/₹2/₹5 coins; triangle angle ratio) are already genuinely well-pitched to AMC 8 entry style (a CRT-flavored remainder puzzle, a combinatorics/casework count, and a ratio-geometry problem) — no change needed there.

**What would be unfair/pointless to add at this stage, and was deliberately left out:**
- **Figure-dependent problems** (cipher-table decoding, nested-triangle area-difference, multiplication pyramids) — a meaningful fraction of real AMC 8 problems (even in the "easy" first third: 2008 P2/P4/P6, 2013 P6) depend on a supplied diagram. This worksheet generator is plain text with no image/diagram rendering capability, so faithfully reproducing that category isn't honestly possible without either (a) hand-drawing ASCII diagrams that would themselves be confusing for an 8–11-year-old to parse, or (b) silently dropping the visual complexity and calling it equivalent, which would be dishonest. Flagging this rather than faking it.
- **Actual AMC 8 multiple-choice format** — real AMC 8 is 25 questions, 40 minutes, 5-choice multiple-choice, no-penalty-for-wrong scoring. Converting this rung to that exact format would be a mismatch with how every other rung on this ladder works (open-response, "show your working," small worksheet) and would test test-taking speed/strategy rather than the underlying reasoning skill the ladder is trying to build. Kept as open-response by design.
- **Combinatorics/probability beyond casework counting** — true AMC 8 upper-half problems (roughly P15+) reach into conditional probability, modular arithmetic proofs, and number-theory arguments that assume a formal vocabulary (case-by-case enumeration with justified exhaustiveness, etc.) genuinely beyond what's fair to expect without dedicated instruction first. This rung deliberately stays anchored to the *entry* quarter of AMC 8 (problems 1–8ish), as instructed — going further would be exactly the kind of "included to look impressive" inflation the task explicitly warned against.

## Changes made in `growth-map.html`

1. **`LEVELS.sarah.maths[11]`** — added `logic:1` flag (only to this rung; rungs 9–11 untouched to stay within this task's scope).
2. **New `m_logic(L)`** (after `m_geometry`) — a deduction/ordering puzzle isomorphic to 1999 AMC 8 P6: 5 randomly-named people, 3 comparison clues built from a fixed, pre-verified logical skeleton (`F>B, F>J, B>M, C>M, B>J, J>M`), asks who has the least — answerable rigorously without fully resolving the order (mirrors the real problem's "proof-lite" character). Wired into `ws_maths_sarah` as new block **H**, gated on `L.logic` — does not affect any other rung.
3. **`m_words(L)`** — added one new `else if(L.algebra>=2)` branch (mean vs. median word problem, modeled on 2013 AMC 8 P5, values constructed so the mean is always a clean integer). This affects rungs 10–12 (all three already have `algebra:2` and previously fell through to no special problem-1 case at all — `m_words`' first block only fires for `pct`/`ratio`/`frac`/`dec`, none of which are set at rungs 9–12, so this branch was pure *added* coverage, not a removal, same justification pattern as rung 1's fix). Flagging this side-effect honestly since rungs 10–11 aren't formally "done" yet in the plan sequence — the plan's own Phase 6 note anticipates and accepts this kind of shared-function ripple.
4. **`mLevelTag`** — rung 12's label text updated to mention logic + "(AMC 8 entry)" so the on-screen level tag matches what's actually being tested.

No existing function signatures or return shapes changed; `ws_maths_sarah`'s call signature is untouched.

## Verification

- `node --check` on the extracted `<script>` block: pass, no output.
- Playwright: navigated to the file (served over `http://localhost` — `file://` is blocked in this environment), ran `LEVEL['sarah']['maths']=11; renderSub('sarah','maths'); makeWS('sarah','maths')`, inspected `#wsprev-sarahmaths` text, then regenerated 6 more times in a loop. All 7 runs: no `undefined`, `NaN`, or `[object Object]`. Found and fixed one real bug during this pass: the new logic-puzzle item pool used "as much [item]" for countable nouns (marbles/stickers/house points), which is a grammar error — fixed by pairing each item with the correct much/many form.

## Honest bottom line on whether this reaches genuine competition rigor

Partially, and it's worth being direct about where it falls short. The new logic-deduction and mean/median blocks are faithful, correctly-calibrated analogues of real AMC 8 problems 5–6 (the easiest quarter, as instructed) — a strong ~10–11-year-old who can solve these has genuinely touched AMC 8 entry-level reasoning. But the rung as a whole is still a **worksheet of separate, labeled, one-answer-per-block exercises** ("A. Warm-up," "B. Word problems," ... "H. Logic puzzle"), not a single unified, unlabeled problem set with no signposting of which skill is being tested — which is a real part of what makes AMC 8 (and any timed multiple-choice competition) hard: you don't get told "this one's a logic puzzle." Removing the labels would make the sheet harder in a way that's true to the source but would also break the ladder's existing pedagogical pattern (every other rung on this ladder is labeled/scaffolded the same way) and wasn't something this task's scope covers changing. Flagging it rather than quietly calling the rung "competition-grade" without qualification.
