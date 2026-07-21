# Mirayah — Maths — Rung 7 "Regrouping"

**Blended grade-equivalent target:** SG MOE Primary Maths P2 upper / Saxon Math 2 upper (per plan's rung-map table — Saxon itself off-limits per the hard constraint, so this rung is sourced entirely from NCERT + MOE).

## Source check — what was actually read

**`sources/maths/ncert-class2-joyful-maths-ch06.pdf`** ("Decoration for Festival," verified full chapter) — this chapter's second half is *specifically* a carrying/borrowing walkthrough, using a "garland" concrete model:
- Carrying (addition): `36 + 47`, worked in a T/O column with an explicit "1" carried above the tens column — "Adding ones (6+7=13) ... Adding tens (3+4+1=8) ... shifting the tens" — landing on 83. Practice set below it: `46+39, 56+19, 36+8, 30+58, 45+35` — every one of these has ones digits summing ≥10.
- Borrowing (subtraction): `46 − 18`, narrated as "I think I should open a [garland, i.e. a ten] ... 46 − 18 → 3 tens 16 ones (crossed-out 4, small 3, crossed-out 6, small 16) → 16−8=8 → 28." Practice set: `45−18, 84−37, 65−45, 27−27, 93−48` — every one has a ones digit of the subtrahend larger than the ones digit of the minuend, i.e. every single practice item **forces** a borrow. That "every item forces it" detail is the direct evidence that a rung named "Regrouping" should force the skill, not just permit it to occur by chance.

**`sources/maths/moe-primary-maths-syllabus-2013.pdf`**, p.34, 2.6 "adding and subtracting using algorithms," 2.8 distinguishing "of a 2-digit number and ones **without renaming**" from the implied renaming case, and learning experience (f) "use the base-ten set to illustrate the standard algorithms for addition and subtraction of 2-digit numbers" — "renaming" is MOE's own term for what NCERT ch.6 calls "opening a garland"; both point at the same skill this rung is named for.

## What this rung should test

1. **Addition that always requires carrying** (ch.6's own practice set — every item forces it).
2. **Subtraction that always requires borrowing** (same — every item in ch.6's borrow set forces it).
3. **At the full within-100 range** (`addMax:99`/`subMax:99`, already correct/unchanged — this is the rung where the "within 100" ceiling from rungs 5-6 is finally reached in full, appropriately paired with the hardest skill).
4. **Place value still active** (`place:1`, unchanged) and **story sums still at tier 2** (`story:2`, unchanged, benefiting from rung 6's fix — see that rung's note).

## Verdict on current knobs/generator — dead-flag bug found and fixed (the flag named in this task's brief)

`LEVELS.mirayah.maths[6] = {n:'Regrouping', addMax:99, subMax:99, count:[10], story:2, shapes:1, place:1, regroup:1}` — knobs unchanged, **generator bug found and fixed**.

**Bug:** `regroup:1` is set on exactly one rung (this one) and, per this task's brief, had never actually been exercised end-to-end before. Grepping the pre-fix file showed `L.regroup` *was* read (in block B, addition) — so it is not a fully-dead flag like `place` was at rung 4 — but two real problems remained:

1. **Block C (subtraction) never read `L.regroup` at all.** A rung named "Regrouping" — which in arithmetic means both carrying *and* borrowing — only forced the addition half. The subtraction block used the same plain "random 2-digit minus random smaller number" logic as every other rung, meaning most generated subtraction items at this rung would **not** require a borrow at all (e.g. `87 − 3`). This is a partial dead-flag: the flag is read, but only half of what its name promises is implemented.
2. **Even in block B, `L.regroup` didn't reliably force a carry.** The old logic (`x=R(15,cap-5); y=R(6,Math.min(9,cap-x))`) picked a large-ish `y` (6-9) but left `x`'s ones digit fully random — so `x`'s ones digit could easily be 0-3, and a 6-9 ones digit doesn't force a carry against a 0-3 ones digit (e.g. `41+8=49`, no carry). It also had a real edge-case: at `x` near `cap-5`, `Math.min(9,cap-x)` could shrink below 6, making the `R(a,b)` call degenerate (when `b<a`, this codebase's `R()` silently returns `a` rather than crashing — not a NaN/undefined bug, but a silent range violation that could push a generated sum slightly past `addMax`).

**Fix (both blocks rewritten with the same ones-digit-forcing technique, modelled directly on ch.6's own worked examples):**
- **Block B (add):** ones digits `xo=R(1,9)`, `yo=R(10-xo,9)` — guarantees `xo+yo>=10` every single time (carry forced, matching ch.6's `36+47` where `6+7=13`). Tens digits are then picked (`R(1,tmax)`) with `tmax` derived from `cap` so the total never exceeds `addMax`.
- **Block C (subtract), now reads `L.regroup` for the first time:** ones digits `xo=R(0,8)`, `yo=R(xo+1,9)` — guarantees `xo<yo` every time (borrow forced, matching ch.6's `46−18` where `6<8`). Tens digits picked so `x`'s tens digit is always strictly greater than `y`'s tens digit, guaranteeing `x>y` (positive answer) even before the borrow is applied.
- Both branches are cap-aware (derived from `L.addMax`/`L.subMax`, not hardcoded), matching the pattern already used for the rung-4 place-value fix and the rung-6 story-sum fix — no magic numbers tied to this specific rung's `99`.

**Place-value interaction, checked as asked:** block I's `cap=Math.max(19,Math.min(99,L.addMax||19))` hits its own ceiling (`99`) at this rung and behaves correctly — verified by direct generation (`94 = 9 tens and 4 ones`, `77 = 7 tens and 7 ones`), no interaction problem with the now-forced-regrouping add/subtract blocks (they're independent code paths).

## Verification

- `node --check` on the extracted `<script>` block: pass, no output.
- Generated 5 sample worksheets via `ws_maths_mirayah(6)`: all produced 7 blocks / 7 answer lines, no `undefined`/`NaN`/`[object Object]`.
- **Every single generated block-B item across all 5 samples (30 items) required carrying** (ones digits verified by hand to sum ≥10 in each, e.g. `33+19` (3+9=12), `47+47` (7+7=14), `18+14` (8+4=12)) — matches ch.6's "every practice item forces it" standard.
- **Every single generated block-C item across all 5 samples (30 items) required borrowing** (ones digit of subtrahend verified greater than minuend's in each, e.g. `36−29` (6<9), `92−67` (2<7), `77−69` (7<9)) — same standard, subtraction side now actually fixed.
- No sum exceeded `addMax:99` and no subtraction went negative in any sample — the `R(a,b)` degenerate-range edge case from the old code no longer applies (both branches now guarantee valid ranges algebraically, not by luck).
- Full-ladder regression: all 10 Mirayah maths rungs re-generated 8× each after this rung's edit — no failures, no dead answer/block-count mismatches.

## Addendum — Block A "Number bonds to 100" plateau fixed (later audit pass)

A later independent audit found `bonds:100` set identically on this rung and the three that follow it
(rungs 8-10) with no format/reps escalation, making Block A statistically indistinguishable across
40% of the ladder while every other block here was escalating hardest — an overlooked plateau, unlike
this file's own deliberately-flat spots (e.g. `story`/`place` carried forward as "background maintenance").
**Fix:** `bondsForm:2` on this rung switches Block A to a genuine new skill sourced from the *same* ch.6
chapter already cited above (p.53-54, "fill the boxes" number-line exercises, e.g. `22 + __ + __ = 38`) —
bridging the gap between a start number and a target by splitting it into a tens-jump plus a ones-jump,
instead of the plain single-blank `x + __ = 100` used through rung 6. `bondsReps` stays at the default 6
here (this rung is the skill's debut); rungs 8-10 step it up (7/8/9) for fluency on top of the same skill,
mirroring the rung-1→2 reps precedent already used for Block A elsewhere in this file. Verified: `node --check`
pass; live-generated samples confirm every Block A item at this rung now reads `s + ___ + ___ = target` with
`target<=100` always and `tensJump>=10` always (never the old flat `x+____=100` form).
