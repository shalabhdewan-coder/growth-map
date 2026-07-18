# Sarah — Maths — Rung 4 "Fractions begin"

**Blended grade-equivalent target:** SG MOE Primary Maths P4 fractions / Beast Academy 4A (per plan's rung-map table).

## Source check — what was actually readable

**`sources/maths/ncert-class4-math-magic.pdf`** — same 16-page front-matter-only file documented in rung 3's note (`pdfinfo`: 16 pages, no chapter bodies). Two citable findings specific to fractions:
- The Contents page (already reproduced in full in rung 3's note) has **no chapter literally titled "Fractions."** The closest candidate is **Ch.5 "Sharing and Measuring" p.62** — thematically, "sharing" is exactly the informal-fraction entry point (splitting a quantity into equal parts), consistent with how fractions are usually introduced at this stage (via division/sharing contexts) before formal `a/b` notation is drilled in isolation.
- "About the Book" (pp.v–vi, already quoted in full in rung 12's note) explicitly lists the book's scope as **"whole numbers and operations, fractions, shapes and spatial relationships, measurement..., and data handling"** — fractions are named as a first-class, explicit Class-4 topic (not folded silently into another strand), confirming this is the right grade to introduce formal fraction work on the ladder.

**`sources/maths/ncert-class3-maths-magic.pdf`** — re-checked (16-page front-matter file, per rungs 1–3's notes) specifically for its own "About the Book" wording on fractions (not fully quoted in earlier notes): **"The chapters of the book cover the foundational ideas of Mathematics: whole numbers and operations, introduction to fractions, shapes and spatial relationships, measurement..., and introduction to data handling."** This is an important calibration point: Class 3 already has an **"introduction to fractions"** (matching **Ch.8 "Fair Share" p.107**, already cited in rung 1's note as an equal-sharing/division chapter) — i.e. informal, sharing-based fraction exposure starts at Class 3, and Class 4 is where it becomes an explicitly named, formal strand (`a/b` notation, fraction-of-a-quantity). This directly justifies the current ladder shape: rungs 1–3 use *no* `frac` flag at all (informal sharing only, via the "fair share"/"mangoes" word problems already in `m_words`), and rung 4 is correctly the first rung to set `frac:1` and introduce `num/d` notation — the grade-equivalence citation supports the plan's placement exactly.

**`sources/maths/moe-primary-maths-syllabus-2013.pdf`** — re-checked; as established in every earlier rung's note, this file is P1-content-only and has no P4 fraction scope to cite. Not useful beyond what's already documented.

**Beast Academy 4A** — **(general knowledge, no source file, per the hard constraint — not opened)** — used only to corroborate that "fractions begin" at this level means fraction-of-a-quantity (forward and inverse), simple same-denominator/simple-denominator comparison, and complement reasoning ("part + remaining part = whole") — not yet equivalent fractions, unlike-denominator addition, or mixed numbers, which are harder-rung territory later in the ladder.

## What this rung should test

1. **Fraction-of-a-quantity, forward direction** (`num/d of whole = ?`) — already in `m_warm` via the `frac` special.
2. **Fraction-of-a-quantity, inverse direction** (given the part, find the whole) — already in `m_bar`'s `frac` branch.
3. **A fraction word problem with complement reasoning** ("X/d of the class walk — how many walk, and how many do not?") — already in `m_words`' first block (`L.frac` branch).
4. **Simple fraction comparison** (which is bigger, and why) — this was the one real gap found (see below): the skill exists in the codebase's `m_mistake` hard bank ("3/4 is smaller than 2/4... wait") but was **structurally unreachable at this rung**.

## Verdict on current knobs/generators

`LEVELS.sarah.maths[3] = {n:'Fractions begin', wm:12, mul2:1, frac:1, s:3, bar:'frac', patt:'geo', chal:2}` — mostly well-calibrated (warm-up, word-problem, and bar-model fraction content all correctly gated on `L.frac` and correctly pitched per the sourced Class-3→Class-4 fraction-introduction progression above), **but one genuine content-fit gap found**, close cousin to the rung-6 dead-flag class of bug this task was specifically asked to hunt for:

`m_mistake(L)`'s bank selection was `const bank=(L.chal>=3)?hard:easy;`. The `hard` bank is the *only* place in the whole codebase a fraction-comparison mistake-spotting item existed (`"3/4 is smaller than 2/4 because 3 comes after 2… wait.” Which is bigger and why?`). But `hard` only becomes reachable at `chal:3+` — and rung 4 sets `chal:2` (correctly, per the ladder's own tier-pairing with rung 3, established in rung 3's note). The result: **the one existing fraction-comparison exercise in the whole app was pinned to rungs 5+ ("Decimals & money" onward), and was completely unreachable at rung 4 — the exact rung named "Fractions begin," whose entire point is fraction reasoning.** This isn't the identical *if/else-priority-masking* shape as the rung-6 bug (there's no second flag stealing priority inside the same branch), but it's the same underlying failure mode the task asked to hunt for: a rung's headline flag (`L.frac`) is silently ignored by a generator (`m_mistake`) because an unrelated gate (`L.chal` threshold) was checked instead, with no path for `frac` to ever influence the outcome. Confirmed by tracing every reference to `L.frac` in the file before this fix — it was checked in `m_warm`, `m_words`, and `m_bar`, but never in `m_mistake`.

**Dead-flag check for this rung's exact knob combination** (`wm, mul2, frac, s:3, bar:'frac', patt:'geo', chal:2`) across the other four generators: `m_warm`'s `specials` array (`frac` + `mul2` both set here) already handles multiple simultaneous special flags correctly post the rung-6 fix (`P(specials)` picks randomly between them each time a "special" warm-up slot is rolled) — confirmed both `frac`-style and `mul2`-style warm-up items actually appear across repeated trials (see verification below), so no masking there. `m_words`'s first-block `if/else if` chain (`pct`/`ratio`/`frac`/`dec`/`algebra>=2`) is correctly ordered so `L.frac` fires (none of the earlier-checked flags are set on this rung). `m_bar`'s `if(t==='frac')` correctly fires since `bar:'frac'` is this rung's exact value. No masking in any of these three.

## Change made

In `m_mistake(L)`, added a small `fracEasy` pool (one item: "1/4 is bigger than 1/2, because 4 is bigger than 2" — a comparison-of-fractions misconception, complementary to the existing hard-bank item rather than a duplicate of it) and changed the bank selection to:
```js
let bank=(L.chal>=3)?hard:easy;
if(L.frac&&L.chal<3)bank=bank.concat(fracEasy);
```
This is additive-only and scoped precisely to `L.frac&&L.chal<3` — the only rung currently in that state is rung 4 (`frac:1, chal:2`). Rungs 1–3 (no `frac`) are unaffected. Rung 6 (`frac:1, chal:3`) still takes the `chal>=3` branch exactly as before (unaffected — the existing hard-bank fraction item stays reachable there too, unchanged). No other rung currently sets `frac` with `chal<3`, so this fix has zero side effects outside rung 4 as of today's `LEVELS` table.

## Verification

- `node --check` on the extracted `<script>` block: pass, no output.
- Generated `ws_maths_sarah(3)` (rung 4, 0-indexed) 8 times directly in Node: zero occurrences of `undefined`, `NaN`, or `[object Object]` across all 8 runs.
- Confirmed the generated content matches the rung's own label: warm-up block mixes fraction-of-a-quantity items (`2/3 of 21 = ____`) with `mul2` facts across repeated trials; word-problem block includes the fraction-of-a-class-with-complement problem ("A class has 15 children. 4/5 of them walk to school. How many walk, and how many do not?") plus the inherited 3-step problem (rung 4 keeps `s:3`); bar-model block correctly asks for the whole given a fraction of it ("1/4 of a number is 3. Draw a bar in 4 equal parts to find the whole number."); level tag reads "P4 · fractions of a quantity."
- Re-ran with a small script forcing many repeats of block E specifically to confirm the new `fracEasy` item is reachable (not just theoretically wired) at rung 4 — appeared within a handful of trials, alongside the pre-existing `easy` bank items, as expected from `P(bank)`'s uniform random pick.
- Re-ran rung 6 (`ws_maths_sarah(5)`, which also sets `L.frac` but at `chal:3`) 8 times: still selects from the unmodified `hard` bank exactly as before this change (spot-checked that the fraction-comparison hard-bank item and the other three hard-bank items still all appear across repeated trials) — confirms the fix didn't regress rung 6's existing calibration.
- Answer-key line count (6 lines: A–E + ★) matches the 6 generated blocks in every trial.
