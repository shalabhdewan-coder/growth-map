# Sarah — Maths — Rung 2 "Bigger numbers"

**Blended grade-equivalent target:** SG MOE Primary Maths P3 upper / CBSE-NCERT Class 3 upper / Saxon Math 5/4 upper scope (per plan's rung-map table) — i.e. one notch harder than rung 1, still squarely Class-3-equivalent, not yet Class-4 material.

## Source check — what was actually readable

**`sources/maths/ncert-class3-maths-magic.pdf`** — same 16-page front-matter-only file already documented in rung 1's note (verified via `pdfinfo`: 16 pages, cover through Constitution preamble, no chapter bodies). Re-confirmed the Contents page (p.[vi]) for this rung specifically:
- **Ch.3 "Double Century" p.16** — the chapter title itself ("double century" = 200) signals Class 3's number-sense chapter is about extending place value/number range beyond 100, not yet into the thousands.
- **Ch.6 "House of Hundreds – I" p.64** and **Ch.9 "House of Hundreds – II" p.117** — two separate chapters on 3-digit place value / addition-subtraction with regrouping, confirming Class 3 spends real time on larger (3-digit) numbers as a dedicated strand, not a one-off topic.
- "About the Book" (pp.v-vi, already quoted in rung 1's note): the book's chapters cover "whole numbers and operations" with a deliberate move "from bare problems to problem situations" — i.e. bigger-number work is meant to show up in word problems, not just isolated arithmetic drills.

**`sources/maths/ncert-class4-math-magic.pdf`** (NCERT *Maths Mela* for Class 4, ISBN 978-93-5729-703-5) — `pdfinfo` confirms this file is also **only 16 pages** (same front-matter-only situation as Class 3: cover, foreword, "About the Book," NSTC committee, Contents page, Constitution preamble — no chapter bodies). Read for this rung as a **forward reference only** (rung 2 targets Class-3-upper, not Class-4, so this file mainly confirms what rung 2 should *not* yet reach):
- Verified Contents page: **Ch.4 "Thousands Around Us" p.39** — this is where 4-digit/thousands place value formally lives, one grade above this rung's target. Confirms rung 2 should stay in the 2–3-digit range (tens/hundreds), not jump to thousands — that escalation belongs at a later rung, not here.
- "About the Book" text (already quoted in rung 12's note) lists "whole numbers and operations... measurement... data handling" as Class 4 scope, continuing (not replacing) Class 3's whole-number strand.

**`sources/maths/moe-primary-maths-syllabus-2013.pdf`** — as already established in rung 1's note, this document is P1-content-only (its own cover page says "content for Primary 2 to 6 will be updated accordingly," never was). The 2013→2018 rollout table (p.6) confirms P3 syllabus content only reached classrooms from 2015 — nothing P3-specific to cite from this file. Re-checked for this rung specifically (grepped for "Primary 3"/"fraction" across the whole doc): no new hits beyond the rollout table already found. Not useful beyond what rung 1 already extracted.

**Saxon Math 5/4 upper scope** — **(general knowledge, no source file, per the hard constraint — not opened)** — used only to corroborate that "bigger numbers" as a discrete step (2-digit multiplier facts, larger word-problem totals, before three-step problems are introduced) is a real staging point in Saxon's own incremental sequence, not an invented rung.

## What this rung should test

1. **Larger multiplication/division facts** — 2-digit × 1-digit (already the `mul2` flag's job), one step up from rung 1's single-digit-factor drill. Matches Class 3's "House of Hundreds" chapters' emphasis on working comfortably with numbers past 100.
2. **The SAME two-step word-problem shapes as rung 1** (multiply-then-give-away, share-then-count) but **with genuinely bigger numbers throughout** — not just in the warm-up block. This is the one real gap found (see below).
3. **Bar-model, pattern-hunting, mistake-spotting** at the same *shape* as rung 1 (still `bar:'cmp'`, still the arithmetic-sequence pattern default) but again should use bigger numbers where the rung's own name promises it.
4. **Same ★ Challenge tier (`chal:1`) as rung 1** — deliberately unchanged; the existing rung-progression already pairs rungs 1–2 at the same challenge tier (chal sequence across the whole ladder is `1,1,2,2,3,3,3,4,4,5,5,6` — clearly an intentional pairing pattern, not an oversight), so tightening `chal` here would be inconsistent with the ladder's own design and out of this task's scope.

## Verdict on current knobs/generators

`LEVELS.sarah.maths[1] = {n:'Bigger numbers', wm:12, mul2:1, s:2, bar:'cmp', patt:'arith', chal:1}` — **the only difference from rung 1's knob object is `mul2:1`.**

**Gap found (not a dead-flag masking bug like rung 6's, but a real content-fit gap of the same family — a flag that's set but doesn't actually reach most of the sheet):** traced every use of `L.mul2` in the codebase before this task — it was referenced **only inside `m_warm`** (as part of the `specials` array added by the rung-6 dead-flag fix). Every other block on the sheet — the word problems (`m_words`), the bar-model (`m_bar`), and the pattern-hunter (`m_pattern`) — ignored `L.mul2` completely and used the exact same fixed number ranges as rung 1. Concretely, before this task's fix, generating rung 1 vs rung 2 side-by-side, the *only* observable difference across the entire six-block worksheet was the six warm-up facts; blocks B (word problems), C (bar model), D (pattern), and E (mistake-spotting) were statistically indistinguishable between the two rungs. For a rung explicitly named "Bigger numbers," and given NCERT Class 3's own "About the Book" text says bigger-number work should show up in "problem situations" (not just bare facts), this under-delivers on the rung's own label.

I checked specifically for the rung-6-style *dead-flag* pattern (a flag masked by an earlier-checked flag in the same if/else chain) across `m_warm`/`m_words`/`m_bar`/`m_pattern`/`m_mistake` for this rung's exact knob combination (`wm, mul2, s:2, bar:'cmp', patt:'arith', chal:1`) — none exists here: `mul2` doesn't collide with any other flag set on this rung (rung 2 sets no `frac`/`longmul`/`dec`/`ratio`/`pct`/`algebra`), so there's no masking. The gap is narrower scope, not masking.

## Change made

Extended `L.mul2`'s effect from "warm-up drill only" to "the whole sheet," gated purely on the same flag (so rung 1, which never sets `mul2`, is completely unaffected — verified below):
- **`m_words`**, block 2 (the `L.s<3` shared branch, packets / fair-share problems): packet count and per-packet count (`p`,`q`) and the sharing group/amount (`g`,`share`) now use a wider `mul2`-gated range when set, otherwise fall back to rung 1's original ranges.
- **`m_words`**, block 3 (the always-present change/division problem): unit price (`c`) and quantity bought (`cnt`) now use a wider `mul2`-gated range when set.
- **`m_bar`**, default (`cmp`) branch: total and difference (`tot`,`d`) now use a wider `mul2`-gated range when set.
- **`m_pattern`**, default (arithmetic-sequence) branch: starting value and common difference (`st`,`d`) now use a wider `mul2`-gated range when set.

All four changes are single-line ternaries of the form `L.mul2?R(bigger):R(original)` — the original (non-`mul2`) branch is byte-for-byte the same arithmetic as before, so rung 1 (no `mul2`) is provably unaffected, and any other current or future rung that doesn't set `mul2` is equally unaffected. Rungs that *do* already set `mul2` besides rung 2 (rung 3 and rung 5, per the current `LEVELS` table) pick up the same wider ranges as a side effect — for rung 3 this is a genuine improvement (rung 3 is "Three-step problems," one notch harder, so bigger numbers are directionally correct there too, and rung 3 is being recalibrated in this same task batch). Rung 5 ("Decimals & money") hasn't been recalibrated yet (later task in the plan's sequence) — the wider ranges are a directionally-reasonable side effect there too (bigger numbers, not smaller), and don't break anything (still produces valid, exact arithmetic), so left as-is per the plan's own Phase 6 note anticipating this kind of shared-function ripple.

## Verification

- `node --check` on the extracted `<script>` block: pass, no output.
- Generated `ws_maths_sarah(1)` (rung 2, 0-indexed) 8 times directly in Node (script extracted, DOM-init tail removed at `document.querySelectorAll('.tab')`, evaluated): zero occurrences of `undefined`, `NaN`, or `[object Object]` across all 8 runs.
- Confirmed rung 2's `level` tag ("Singapore P3+ · larger numbers") and content now genuinely differ from rung 1 across every block, not just the warm-up (sample run: rung 1 bar-model shared 22 stickers with a difference of 4; rung 2 bar-model shared 76 stickers with a difference of 16 — same problem shape, visibly bigger numbers).
- Re-ran rung 1 (`ws_maths_sarah(0)`) 8 times after the edit: numbers stayed in the original small ranges (e.g. bar-model totals in the 16–30 range, not 40–80+), confirming the `mul2`-gated branches don't leak into rung 1.
- Re-ran rung 6 (`ws_maths_sarah(5)`) and rung 12 (`ws_maths_sarah(11)`) 8 times each (sanity check per task instructions, since this task touched shared functions `m_words`/`m_bar`/`m_pattern` that those rungs also use): both clean, no `undefined`/`NaN`/`[object Object]`, and both still read as calibrated to their existing labels (rung 6 still produces long-multiplication/fraction/triangular-number content per its own flags; rung 12 still produces its algebra/geometry/logic/proof blocks). Neither rung sets `mul2`, so neither was expected to change, and neither did.
