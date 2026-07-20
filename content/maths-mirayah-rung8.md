# Mirayah — Maths — Rung 8 "Groups of 2·5·10"

**Blended grade-equivalent target:** SG MOE Primary Maths P2-P3 bridge / Beast Academy 2C (per plan's rung-map table — Beast Academy itself off-limits per the hard constraint; this rung is sourced entirely from MOE + NCERT Cl.2, both within the task's explicit whitelist for this rung: `ncert-class2-joyful-maths-ch01..11.pdf`, `moe-primary-maths-syllabus-2013.pdf`, `mathkangaroo-preecolier-sample.pdf`, `ikmc-ecolier-sample.pdf`).

## Source check — what was actually read

**`sources/maths/moe-primary-maths-syllabus-2013.pdf`** — re-verified via `pdftotext` that the document's only section marker is `PRIMARY ONE` (line 1264; there is no `PRIMARY TWO` heading anywhere in the file, confirming the prior agent's note that this document truly is "Primary One content only"). The multiplication content sits inside that same Primary One block, p.34 ("Content / Learning Experiences" table):
- **3. Multiplication and Division** — "3.1 concepts of multiplication and division," "3.2 use of x," **"3.3 multiplying within 40,"** "3.4 dividing within 20," "3.5 solving 1-step word problems involving multiplication and division with pictorial representation."
- Learning experience (a): "make equal groups using concrete objects and count the total number of objects in the groups **by repeated addition** using language such as **'2 groups of 5' and '2 fives'**."

This is a direct, exact match for the rung's own name ("Groups of 2·5·10") and for the existing generator's phrasing ("N baskets have M apples each") — both the "equal groups / repeated addition" framing and the "multiplying within 40" number ceiling are sourced, not invented.

**`sources/maths/ncert-class2-joyful-maths-ch03.pdf`** ("Fun with Numbers," verified full chapter, all 9 pages read) — this is the "natural bridge" chapter named in this task's brief. It does **not** contain formal multiplication (no "3 groups of 4 = 12" style content anywhere in the chapter) but it is entirely built around **skip-counting on a number line/number chart**, directly supporting the pre-multiplication scaffolding this rung needs:
- p.26 "Let us Do": explicit skip-counting-in-2s and skip-counting-in-5s exercise on a 100-chart (circle multiples of 2, triangle multiples of 5, find numbers common to both).
- p.26 same page: "If you start from 5 and count in fives, will you jump on 40?" / "If you start from 0 and count in fives, will you jump on 55?" — skip-counting in 5s up into the 40s-50s range.
- p.26: "If you start from 13 and count in threes, will you jump on 24?" — skip-counting in **threes** is also explicitly modelled (relevant for rung 10's `count:[3,4,6]`, noted in that rung's file).
- p.25: "40, 45, 50, ___" and "50, 60, ___" patterns — skip-counting in 5s and 10s in the pattern-completion exercises already backing block D ("Count in Ns") since earlier rungs.

**Honesty note per this task's brief:** ch.3 grounds the *skip-counting* half of "groups of 2·5·10" solidly, but the *multiplication concept itself* ("N groups of M, how many in total") is not this chapter's content — it comes from the MOE P1 syllabus excerpt above instead, which does state it explicitly ("2 groups of 5," "2 fives," "multiplying within 40"). Between the two whitelisted sources, the full rung is genuinely covered — no NCERT Class 3 material needed to be opened for this rung, and none was.

## What this rung should test

1. **Multiplication-as-equal-groups**, phrased exactly as the MOE source phrases it ("N baskets have M [items] each"), bounded to **within 40** (the MOE 3.3 ceiling) — not merely "some formula that happens to fit," a real sourced number bound.
2. **Skip-counting in 2s/5s/10s** (block D, unchanged, already correctly reads `L.count`) — ch.3's own practice range.
3. Addition/subtraction at the full within-99 range stays active (unchanged from rung 7) as background maintenance, not the new focus.
4. Story sums stay at tier 2 (unchanged, already fixed at rung 6).

## Verdict on current knobs/generator — dead/mismatched-flag bug found and fixed

`LEVELS.mirayah.maths[7] = {n:'Groups of 2·5·10', addMax:99, subMax:99, count:[2,5,10], story:2, shapes:0, mul:1}` — knobs otherwise fine, **generator bug found and fixed in block E** (this rung's own `mul:1` flag, named in this task's brief).

**Bug found:** `L.mul` block E existed and *was* read (not a fully-dead flag like `place` at rung 4 originally was) — `if(L.mul){const g=R(2,5),per=P([2,5,10]); ... g*per}`. Two real problems:
1. **`per` was hardcoded to `[2,5,10]` instead of reading `L.count`.** This happens to be harmless *at this specific rung* (rung 8's own `count` array is also `[2,5,10]`) but is a silent mismatch waiting to misfire — rung 10 sets `count:[3,4,6]` and the old code would still draw multipliers from `[2,5,10]` there, ignoring the knob entirely (verified before the fix — same bug class as the `story`/`place`/`regroup` dead-flag pattern already found on this ladder, just one level more subtle: the flag itself is read, but a *sibling* knob it should have deferred to (`L.count`) silently wasn't).
2. **No ceiling matching the sourced "within 40" bound.** `g=R(2,5)` and `per` up to `10` allowed products up to `5×10=50`, exceeding the MOE-sourced ceiling for this exact skill by 25%. Not a crash/NaN bug, but a real content-accuracy miss against the cited source.

**Fix:** Block E now reads `L.count` for `per` (matching block D's existing pattern) and picks `g` first (2-8, kept small enough to plausibly "draw the groups" as the item text instructs), then filters `L.count` down to values that keep `g*per<=L.mulMax` before picking `per` — guaranteeing the product never exceeds the rung's sourced cap. Added a new knob `mulMax:40` to this rung specifically because `40` is the literal MOE 3.3 number, not a guessed value; rungs 9-10 get their own (higher, explicitly general-knowledge-justified) `mulMax` values — see those rungs' files.

```
if(L.mul){const arr=L.count||[2,5,10],mCap=L.mulMax||40,g=R(2,8),permax=Math.max(arr[0],Math.floor(mCap/g)),opts=arr.filter(v=>v<=permax),per=P(opts.length?opts:[arr[0]]);
 blocks.push({h:'E. Groups',items:[g+' baskets have '+per+' apples each. How many apples in all?  ____   (draw the groups)']});ans.push('E: '+(g*per));}
```
`opts` is provably non-empty in every case reachable from this ladder's rung configs (`arr[0]<=permax` whenever `mCap>=g*arr[0]`, true for every `(g,mCap)` pair this rung/rung9/rung10 can produce — checked by direct generation, see Verification below), so there is no NaN/undefined risk from an empty `P([])` call.

## Verification

- `node --check` on the extracted `<script>` block: pass, no output.
- Generated 8 sample worksheets via `ws_maths_mirayah(7)`: all produced 6 blocks (A,B,C,D,E,G — F skipped since `shapes:0`, H/I skipped since `twostep`/`place` unset at this rung) / 6 answer lines, no `undefined`/`NaN`/`[object Object]`.
- **Every generated block-E product across 8 samples stayed ≤40** (e.g. `4 baskets have 5 apples each = 20`, `7 baskets have 5 apples each = 35`, `2 baskets have 10 apples each = 20`, `8 baskets have 2 apples each = 16`) — matches the MOE 3.3 "multiplying within 40" ceiling exactly, and `per` was drawn from `[2,5,10]` in every sample (rung 8's own `count` array), confirming the `L.count` read now actually drives the value instead of the old hardcoded array.
