# Sarah — Maths — Rung 6 "Multiplication mastery"

**Blended grade-equivalent target:** SG MOE Primary Maths P5 / Beast Academy 4C (per plan's rung-map table).

**Why this note is being written now, out of order:** every other Sarah-maths rung (1-5, 7-12) got a dedicated `content/maths-sarah-rungN.md` audit note and a `feat(maths/sarah): rung N ...` calibration commit during Phase 2. Rung 6 never did — it was only ever touched incidentally by `fb42749` ("fix(maths): m_warm longmul dead when frac also set on same rung... hit rung 6 specifically") and the original placement commit `cfe46cb` (`chore(sarah): re-place at maths rung 6...`). This gap surfaced while cross-checking the whole ladder against Khan Academy's real NCERT course structure (see `content/khan-academy-crosscheck.md`) — this note closes it using the same audit method every other rung already went through.

## Source check

Real-world placement evidence for this rung already exists and doesn't need re-litigating: `content/sarah-placement-assessment.md` places Sarah's maths floor at rung 6 from 14 real Pathways School Gurgaon PYP3 homework PDFs (Nov 2025-March 2026) — her routine graded work (multiplication, fractions) paces rung 4-6.

New for this note: cross-checked `longmul` (this rung's headline new flag) against Khan Academy's real NCERT Class 4 and Class 5 Math course structures (browsed live, 2026-07-20 — see `content/khan-academy-crosscheck.md` for the full class-by-class findings). Class 4's "Multiplication" unit runs area-model → distributive-property → multiply-by-1-digit-numbers → "multi-digit multiplication" (light 2-digit intro). Class 5's own "Multiplication" unit (not skill-by-skill deep-dived, but confirmed to exist as Unit 2 of an 11-unit, 66-skill course) sits one step past that. This rung's `longmul` flag genuinely tests a step beyond Class 4's area-model introduction — consistent with "Multiplication mastery" sitting at the SG P5/Beast Academy 4C blended target, one notch above rung 4's Class-4-equivalent multiplication scope.

## What this rung's knobs actually generate — dead-flag / narrow-reach audit

`LEVELS.sarah.maths[5] = {n:'Multiplication mastery', longmul:1, frac:1, s:3, bar:'frac', patt:'tri', chal:3}`.

Traced `longmul` and `frac` through all five generators for this exact knob combination:

- **`m_warm`**: `specials` array includes both `frac` and `longmul` (`if(L.frac)specials.push('frac');if(L.longmul)specials.push('longmul');`); `kind==='longmul'` produces genuine 2-digit×2-digit multiplication (`x=R(12,29+3*bo), y=R(11,19+2*bo)`). Confirmed both special types actually appear across repeated trials (`P(specials)` picks uniformly) — this is exactly the bug `fb42749` already fixed (before that fix, `frac` silently masked `longmul` in an if/else chain; now both compete fairly for each "special" warm-up slot).
- **`m_words` block 1** (headline special problem): chain is `pct → ratio → frac → dec → algebra`. Rung 6 sets none of `pct`/`ratio`/`dec`/`algebra`, so `L.frac` fires cleanly (fraction-of-a-class-with-complement problem, same shape as rung 4's calibrated content) — no masking.
- **`m_words` block 2** (`L.s>=3` steps problem): `per=L.longmul?R(6,15):R(3,7)` and `m=L.longmul?R(10,40):R(4,15), n=L.longmul?R(10,40):R(4,15)` — `longmul` correctly scales this block's number range up.
- **`m_words` block 3** (change problem): `big=L.mul2||L.longmul` — `longmul` correctly triggers the bigger price range (`R(15,45)` vs `R(6,19)`) even though `mul2` isn't set on this rung.
- **`m_bar`**: `t='frac'` (this rung's `bar` value) — fires the same fraction bar-model as rung 4/5's frac cases. No `longmul`-specific bar-model type exists, and none is needed — the bar-model block's job on this rung is fraction practice, not multiplication practice, consistent with the rung's design (one new headline skill at a time gets the warm-up/word-problem escalation; the bar-model block continues reinforcing the prior rung's skill).
- **`m_mistake`**: `bank=(L.chal>=3)?hard:easy` — rung 6's `chal:3` selects `hard`, which already contains the fraction-comparison item (`"3/4 is smaller than 2/4..."`) from rung 4's audit. `L.frac&&L.chal<3` (the `fracEasy` addition) doesn't apply here since `chal:3`, which is correct — the harder fraction item, not the easier one, is the right pairing at this rung's difficulty tier. No dedicated `longmul`-mistake bank exists, but the generic `easy`/`hard` banks already include a multiplication-adjacent item (`"9 × 6 = 54, then 54 − 7 = 48"` in `hard`), so multiplication mistake-spotting isn't entirely absent either.

**No masking, no narrow-reach bug found.** `longmul` reaches all three places it should (warm-up, and both scaled word-problem blocks); `frac` correctly continues reaching the same blocks it reached at rung 4, unescalated (appropriate — this rung's job is to advance multiplication, not fractions).

## Verdict

No code change needed. This rung was already correctly calibrated by the time `fb42749` fixed the `longmul`/`frac` warm-up masking bug — it just never got the standalone audit note documenting that. This note is that documentation, written after independently re-deriving the same conclusion via a full generator trace and cross-checking `longmul`'s difficulty jump against Khan Academy's real Class 4→5 multiplication progression.

## Verification

- `node --check` on the extracted `<script>` block: pass, no output.
- Generated `ws_maths_sarah(5)` (rung 6, 0-indexed) 6 times directly in Node: zero occurrences of `undefined`, `NaN`, or `[object Object]` across all runs.
- No `growth-map.html` changes made by this note — verification confirms the existing (already-shipped) rung 6 behaviour, it does not test a new change.
