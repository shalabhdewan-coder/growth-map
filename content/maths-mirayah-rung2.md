# Mirayah — Maths — Rung 2 "Bonds to 10"

**Blended grade-equivalent target:** SG MOE Primary Maths P1 upper / Saxon Math 1 upper (per plan's rung-map table).

## Source check — what was actually read

**`sources/maths/moe-primary-maths-syllabus-2013.pdf`**, pp.33-34 (same P1-only content as rung 1, re-checked for this rung's specific angle):
- **1.8** "number bonds for numbers up to 10" — listed as its own numbered content item, confirming "bonds to 10" is a real, named P1 syllabus point, not an invented rung title.
- Learning-experience (b): "use strategies such as 'count on', 'count back', 'make ten' and 'subtract from 10' for addition and subtraction within 20... and thereafter, within 100" — the phrase **"make ten"** is the textbook name for exactly this rung's skill (decomposing/composing to 10), and it's explicitly positioned as a *strategy that enables* addition/subtraction within 20 — i.e., bonds-to-10 mastery is scaffolding for rung 3, not an end in itself. Good confirmation that rung 2 → rung 3 is the right next step.
- Sub-strand Whole Numbers 1.6 "patterns in number sequences" again — supports continuing/deepening the skip-counting block at this rung too (now with `count:[2,5]`).

**`sources/maths/ncert-class2-joyful-maths-ch03.pdf`** ("Fun with Numbers," 9 pages, verified full chapter) — this chapter is centred on a 100-square number line/chart with skip-counting activities: "use skip counting in two... draw on the numbers... now use skip counting in five," and "write down the numbers that are common to the skips in twos and fives" (p.25 region of chapter). This directly supports rung 2's `count:[2,5]` knob (introducing 5s alongside 2s) as a real, sourced next step up from rung 1's `count:[2]` only — not an arbitrary bump.

## What this rung should test

1. **Bonds to 10, with more repetition/fluency than rung 1** (MOE 1.8) — existing block A, `bonds:10` unchanged from rung 1 (correct: MOE scopes bonds work to "up to 10" for the whole of this stage, so it should stay anchored at 10 rather than escalate to bonds-to-20 here — that would blur it with rung 3's territory).
2. **Skip-counting in 5s introduced alongside 2s** (NCERT ch.3's paired 2s/5s activity) — existing block D, `count:[2,5]`.
3. **Slightly larger addition sums** (`addMax:12` vs rung 1's `10`) — modest, appropriate escalation; MOE's "make ten" strategy note implies sums just past 10 are the natural next step before formal "within 20" work at rung 3.
4. **Shapes continue** (block F) — MOE 2D-shapes learning experience (g)/(h) explicitly wants shape *sorting* and *pattern-making* practice repeated, not a one-off.

## Verdict on current knobs/generator

`LEVELS.mirayah.maths[1] = {n:'Bonds to 10', addMax:12, bonds:10, count:[2,5], story:0, shapes:1}` — **well-calibrated, no code changes needed.**

- `bonds:10` staying identical to rung 1 is *correct*, not a missed escalation — confirmed by MOE's own scoping of bonds work to "up to 10" only; the rung name itself signals this is a mastery/fluency rung for the same target, not a range increase. (Escalation instead comes from `addMax` 10→12 and `count` gaining `5`.)
- `story:0` still correctly suppresses story sums (block G) — consistent with rung 1's note; word problems start at rung 3.
- No `subMax` — block C still doesn't fire. Same honest caveat as rung 1: MOE would introduce light subtraction here too, but the ladder's own naming defers it to rung 3 by design; not changed.
- **Dead-flag audit for this rung's knobs:** `addMax`, `bonds`, `count`, `story`, `shapes` are all read in `ws_maths_mirayah` — no unused knob at rung 2.

## Verification

- `node --check` on the extracted `<script>` block: pass, no output.
- Generated 3 sample worksheets via node (`ws_maths_mirayah(1)`): all produced 4 blocks / 4 answer lines (A bonds, B add, D count — now sometimes stepping by 5s — F shapes), no `undefined`/`NaN`/`[object Object]`.
- No `growth-map.html` change required for this rung; committing the content note only.
