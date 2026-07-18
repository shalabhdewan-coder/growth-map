# Sarah — Maths — Rung 3 "Three-step problems"

**Blended grade-equivalent target:** SG MOE Primary Maths P4 / CBSE-NCERT Class 4 / Saxon Math 6/5 early scope (per plan's rung-map table).

## Source check — what was actually readable

**`sources/maths/ncert-class4-math-magic.pdf`** (NCERT *Maths Mela*, Textbook of Mathematics for Grade 4, ISBN 978-93-5729-703-5) — `pdfinfo` confirms **16 pages only** (front matter through Constitution preamble; no chapter bodies, same situation as the Class 3 file already documented in rung 1/2's notes). Verified Contents page (p.[vi] of the PDF):

```
Chapter 1 : Shapes Around Us                    1
Chapter 2 : Hide and Seek                      24
Chapter 3 : Pattern Around Us                  34
Chapter 4 : Thousands Around Us                39
Chapter 5 : Sharing and Measuring              62
Chapter 6 : Measuring Length                   80
Chapter 7 : The Cleanest Village               95
Chapter 8 : Weigh it, Pour it                 115
Chapter 9 : Equal Groups                      128
Chapter 10 : Elephants, Tigers, and Leopards  149
Chapter 11 : Fun with Symmetry                164
Chapter 12 : Ticking Clocks and Turning Calendar 175
Chapter 13 : The Transport Museum             184
Chapter 14 : Data Handling                    203
```

Chapters directly relevant to this rung's target skill (multi-step arithmetic word problems at bigger scale):
- **Ch.4 "Thousands Around Us" p.39** — the chapter title itself confirms Class 4 formally reaches 4-digit place value/number range, one clear step up from Class 3's "Double Century"/"House of Hundreds" (2–3 digit) scope documented in rung 1/2's notes. This is the direct citation for pushing the warm-up/word-problem number ranges up again at this rung.
- **Ch.5 "Sharing and Measuring" p.62** and **Ch.9 "Equal Groups" p.128** — division/sharing and multiplication-as-grouping as two separate, dedicated chapters, confirming both operations stay core at Class 4 (not phased out in favour of harder topics) — supports keeping the existing 3-step "sell some, sell more, then divide the rest" word-problem shape (subtract, subtract, divide), which already exercises both chapters' skills in one problem.
- **Ch.13 "The Transport Museum" p.184** — thematically a multi-stage, real-world-context word-problem chapter (exhibits/tickets/distances), consistent with Class 4 being where genuinely multi-step (3+ operation) word problems become normal, not an enrichment add-on.
- "About the Book" (pp.v–vi, already quoted in full in rung 12's note): explicitly states the book covers "whole numbers and operations, fractions, shapes and spatial relationships, measurement..., and data handling," and that "ideas will keep recurring throughout the book building in deeper engagement and complexity" — i.e. Class 4 is explicitly designed to revisit Class 3's operations at greater complexity, which is exactly what a "three-step" (vs rung 1–2's two-step) word problem does.

**`sources/maths/ncert-class3-maths-magic.pdf`** — re-checked (same 16-page front-matter file documented in rung 1/2's notes) for anything relevant to the *step count* of word problems specifically. **Ch.12 "Give and Take" p.150** is confirmed as a dedicated addition/subtraction word-problem chapter at Class 3 — establishes that 2-step (add-then-subtract style) problems are the Class-3 baseline this rung should build on, corroborating the plan's placement of "three-step" one grade above "two-step."

**`sources/maths/moe-primary-maths-syllabus-2013.pdf`** — re-checked; as already established in rungs 1/2/12's notes, this file is P1-content-only (its own text states P2–P6 content "will be updated accordingly," never was, and the 2013→2018 rollout table on p.6 shows P4 syllabus content wasn't scheduled to reach classrooms until 2016). No new citable content for this rung beyond what's already documented.

**Saxon Math 6/5 early scope** — **(general knowledge, no source file, per the hard constraint)** — used only to corroborate that three-step mixed-operation word problems (as opposed to Saxon 5/4's two-step baseline) are a real, deliberate staging point in Saxon's own incremental sequence at the 6/5 level, not an invented rung.

## What this rung should test

1. **The same fact-fluency warm-up as rung 2** (`mul2`-flagged 2-digit × 1-digit facts) — no escalation needed here; Ch.4 "Thousands Around Us" is the citation for *number range* growth, not fact-drill difficulty, and rung 4 (fractions) is where the next warm-up escalation belongs.
2. **A genuine three-step word problem** — start with a total, subtract twice (two separate real-world events), then divide the remainder — already implemented via the existing `L.s>=3` branch in `m_words` (the "shop starts with S stickers, sells m in the morning, n in the afternoon, packs the rest into g boxes" problem). Directly matches Ch.5/Ch.9's division-and-grouping scope, applied on top of Ch.4's bigger-number range.
3. **A harder pattern type** — geometric (multiply-by-r) sequences instead of rung 1–2's simple arithmetic (add-a-constant) sequences, via `patt:'geo'`. This is a genuine step up in reasoning (multiplicative vs additive pattern-spotting), appropriately timed for the rung where three-step multi-operation reasoning is also being introduced.
4. **One tier harder ★ Challenge** (`chal:2`, shared with rung 4) — the ladder's existing chal-tier pairing (`1,1,2,2,3,3,3,4,4,5,5,6`) already places rungs 3–4 together; `CHAL[2]`'s three items (handshake combinatorics, sum/product number pair, clock-angle) are a legitimate step up from `CHAL[1]`'s pure counting/tables flavour and don't need changing.

## Verdict on current knobs/generators

`LEVELS.sarah.maths[2] = {n:'Three-step problems', wm:12, mul2:1, s:3, bar:'cmp', patt:'geo', chal:2}` — **well-calibrated already, and meaningfully different from both rung 1 and rung 2** on inspection:
- vs rung 1: `s:2→3` (activates the genuine 3-step word problem via `m_words`'s `L.s>=3` branch instead of the 2-step packets/fair-share branch), `patt:'arith'→'geo'` (harder pattern reasoning), `chal:1→2` (harder challenge tier). Three independent, source-grounded differentiators — no gap here.
- vs rung 2: same `mul2:1`/`bar:'cmp'` (correctly unchanged — Ch.4's bigger-number-range citation applies equally to rungs 2 and 3, and rung 2's fix in this same task batch already pushed `mul2`-gated ranges into `m_words`/`m_bar`/`m_pattern`'s default branches, so rung 3 inherits genuinely bigger numbers in its word problems and bar-model too, not just its warm-up — confirmed in the verification run below), differentiated by `s`, `patt`, and `chal` as above.

**Dead-flag check for this rung's exact knob combination** (`wm, mul2, s:3, bar:'cmp', patt:'geo', chal:2`) across `m_warm`/`m_words`/`m_bar`/`m_pattern`/`m_mistake`: no masking found. `mul2` doesn't collide with any other special flag on this rung (no `frac`/`longmul`/`dec`/`ratio`/`pct`/`algebra` set), so `m_warm`'s `specials` array (already fixed for the rung-6 case) never has more than one candidate here and there's nothing to mask. `bar:'cmp'` and `patt:'geo'` are each single mutually-exclusive string values consumed by their own function's `if/else if` chain — not multiple simultaneous booleans, so there's no possible priority-masking of the kind found at rung 6. `m_mistake`'s bank-selection (`L.chal>=3?hard:easy`) correctly stays on the `easy` bank at `chal:2` — no `frac`-specific gap here since rung 3 doesn't set `frac` (that's rung 4's headline flag, handled in rung 4's own note). **No code change was needed for this rung specifically** — its knobs were already correctly calibrated, and it benefits from the shared-function widening done as part of rung 2's fix (see rung 2's note) without any additional edit.

## Verification

- `node --check` on the extracted `<script>` block: pass, no output.
- Generated `ws_maths_sarah(2)` (rung 3, 0-indexed) 8 times directly in Node: zero occurrences of `undefined`, `NaN`, or `[object Object]` across all 8 runs.
- Confirmed the generated content matches the rung's own label: warm-up facts in the `mul2` 2-digit range (e.g. `29 × 8`, `27 ÷ 9`), word-problem block B always contains a genuine 3-step problem ("A shop starts with 36 stickers. It sells 5 in the morning and 10 in the afternoon, then packs the rest equally into 3 boxes. How many stickers per box?" — subtract, subtract, divide), pattern block D produces geometric sequences (e.g. `3, 6, 12, 24, ...  Rule: multiply by 2`), and the level tag reads "Singapore P3–P4 · 3-step," consistent with the sourced grade target.
- Answer-key line count (6 lines: A–E + ★) matches the 6 generated blocks in every trial.
