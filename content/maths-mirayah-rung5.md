# Mirayah — Maths — Rung 5 "Within 100"

**Blended grade-equivalent target:** SG MOE Primary Maths P2 / CBSE-NCERT Class 2 (per plan's rung-map table).

## Source check — what was actually read

**`sources/maths/moe-primary-maths-syllabus-2013.pdf`**, p.34, sub-strand Whole Numbers, topic "2. Addition and Subtraction":
- **2.5** "adding and subtracting **within 100**" — the literal source of this rung's name.
- **2.8** mental calculation "within 20 / of a 2-digit number and ones without renaming / of a 2-digit number and tens" — the syllabus explicitly separates "without renaming" (this rung) from renaming/regrouping (reserved for rung 7).
- Learning experience (c): "use strategies such as 'count on', 'count back', 'make ten' and 'subtract from 10' for addition and subtraction within 20 ... and thereafter, **within 100**."

**`sources/maths/ncert-class2-joyful-maths-ch03.pdf`** ("Fun with Numbers," verified full chapter) — the number-range chapter: a "ginladi" (bead string) exercise asking children to make number cards for **38, 44, 58, 65, 98**; a number-strip/number-line exercise with ants sitting at 20 and 80, plotting an ant at **65** and a cube at **79**; skip-counting patterns filled in up to the 90s ("40, 45, 50, ... ", "50, 60, 70, 80, 90"); and a "Guess my Number" game using comparison language ("more than 50", "less than 20"). This chapter is the direct source for treating "within 100" as genuinely approaching 100 (numbers into the high 80s/90s), not stopping at 50.

## What this rung should test

1. **Addition and subtraction with genuinely 2-digit numbers approaching 100** (MOE 2.5) — existing blocks B/C.
2. **Skip-counting by 5s and 10s through the 90s** (ch.3's "40,45,50..." and "50,60,70,80,90" patterns) — existing block D, `count:[5,10]` unchanged, correct.
3. **Place value (tens & ones) at the wider range** (rung 4's `place:1` block, now exercised up to a bigger cap) — see verdict below.
4. **No forced regrouping yet** (MOE 2.8 explicitly separates "without renaming" from renaming) — this rung should NOT force carrying/borrowing; that is rung 7's job (`regroup:1`, not set here).

## Verdict on current knobs/generator — calibration bug found and fixed

`LEVELS.mirayah.maths[4]` was `{n:'Within 100', addMax:50, subMax:50, count:[5,10], story:1, shapes:1, place:1}`.

**Bug:** a rung literally named "Within 100," sourced against MOE 2.5 ("adding and subtracting within 100"), was capping every addition and subtraction at 50 — i.e. actually testing "within 50." This is the same class of "the rung's own name promises X but the knob doesn't deliver X" issue found at rung 4 (there it was a missing block; here it's a numeric ceiling that undersells the rung's name and its source). Meanwhile rung 7 ("Regrouping") already sits at `addMax:99`/`subMax:99`, so the old 50→60→99 progression left a large, uneven jump between rungs 6 and 7.

**Fix:** `addMax`/`subMax` raised **50 → 80**. Chosen (not simply jumped to 99) to keep a smooth three-rung climb toward the true ceiling: rung 5 = 80 (approaching 100, no forced regrouping), rung 6 = 90 (see that rung's note), rung 7 = 99 (the actual "within 100" ceiling, now *with* forced regrouping). This also matches ch.3's own example numbers, which run up to 98/79/65 rather than stopping at 50.

**Interaction with the rung-4 `place:1` fix, checked as asked:** `ws_maths_mirayah`'s block I computes `cap=Math.max(19,Math.min(99,L.addMax||19))`, deriving its number range from `L.addMax` rather than a hardcoded constant. At `addMax:80` this correctly widens to `n=R(11,80)` (verified by direct generation — samples produced `39 = 3 tens and 9 ones`, `57 = 5 tens and 7 ones`, both arithmetically correct). No hardcoding issue at this rung's range — the rung-4 fix generalises cleanly, exactly as that rung's own note predicted.

**Dead-flag audit for this rung's knobs:** `addMax`, `subMax`, `count`, `story`, `shapes`, `place` all confirmed read in `ws_maths_mirayah`. `regroup` is correctly *absent* from this rung's object (not yet — reserved for rung 7) and the generator correctly falls through to its plain, non-forcing branch for both add (block B) and subtract (block C) when `L.regroup` is unset — verified no unintended forced-carry/borrow leaking into this rung's output.

## Verification

- `node --check` on the extracted `<script>` block: pass, no output.
- Generated 5 sample worksheets via `ws_maths_mirayah(4)`: all produced 7 blocks / 7 answer lines, no `undefined`/`NaN`/`[object Object]`. Sample: `B: 33 + 9 = 42`, `C: 62 − 25 = 37`, `I: 39 = 3 tens and 9 ones` — all within the new 80-ceiling, none forced to regroup (consistent with `L.regroup` being unset), arithmetic spot-checked correct.
- Full-ladder regression: all 10 Mirayah maths rungs (0-9) re-generated 8× each after this rung's edit — no failures, no dead answer/block-count mismatches.
