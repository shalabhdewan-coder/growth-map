# Sarah — Maths — Rung 8 "Percentages"

**Blended grade-equivalent target:** SG MOE Primary Maths P6 / CBSE-NCERT Class 6 / Beast Academy 5B scope (per plan's rung-map table).

## Source check — what was actually readable

**`sources/maths/ncert-class6-ganita-prakash.pdf`** — `pdfinfo` confirms 20 pages (front-matter/TOC truncation, same as rung 7's note). Visible Contents page reaches only **Ch.7 "Fractions" p.151** before the truncation — no percentage-titled chapter visible, and grepping the extractable text for "percent" returns zero hits. Same honest gap already documented for rungs 5 and 7: the revised NEP-2023 NCERT sequence very plausibly covers percentages in a chapter beyond what this 20-page truncated file captures, but I cannot cite page numbers or specific problems that aren't in the file.

**`sources/maths/moe-primary-maths-syllabus-2013.pdf`** — grepped for "percent" per the same method as rungs 5 and 7; zero hits, consistent with this document only containing detailed P1 content (percentages are an upper-primary strand per the rung-map, well past P1).

**`sources/maths/amc8-compendium-1985-2026.pdf`** (307-page official AJHSME/AMC 8 archive, legitimate source) — already searched for "percent" while sourcing rung 7 (both rungs share the same source file); the two most useful calibration points, re-examined here specifically for percentage difficulty pitch:
- **1995 AJHSME, Problem 1**: *"Walter has exactly one penny, one nickel, one dime and one quarter in his pocket. What percent of one dollar is in his pocket?"* — Problem 1 (the easiest slot on any AJHSME/AMC 8 paper, per rung 12's finding that problems 1-3 are consistently entry-difficulty). This directly confirms that **money-context percent-of-a-whole is treated as genuinely easy, entry-tier material** by a real competition — supporting the existing `m_words` discount problem's framing (percent-off a shop item) and this task's addition of money-flavoured percent-of-a-quantity warm-up items.
- **1989 AJHSME, Problem 9**: the boys/girls ratio-to-percent problem already cited in rung 7's note — direct evidence that ratio (rung 7) and percent (rung 8) are treated as a connected pair of skills in real competition material, one naturally following the other (percent = "a ratio out of 100"), which corroborates the rung-map's own sequencing decision to place them back-to-back.
- A general scan of "percent" hits across the compendium (candy-sale discounts, exam-grade percentages, bar/pie-chart-percent-reading, successive percentage changes) confirms that **discount/sale-price framing** (already the app's existing `m_words` percent problem) and **percent-of-a-quantity** (the gap this task fills in `m_warm`) are both genuinely representative percent problem shapes at this level — successive/compound percentage change (e.g. "50% off then another 20% off") appears in later papers but reads as a harder, pre-algebra-adjacent skill, appropriately reserved for a later rung rather than added here.

**Beast Academy 5B / CBSE Class 6 scope** — **(general knowledge, no source file, per the hard constraint — not opened)** — used only to corroborate that percent-of-a-quantity (forward direction: "25% of 60 = ?") is the standard entry point for formal percentage work at this grade band, taught immediately alongside (not after) percent-discount word problems — i.e. both skills belong at the same rung, which is exactly what this task's `m_warm` fix now delivers alongside the pre-existing `m_words` discount problem.

## What this rung should test

1. **Percent-of-a-quantity, forward direction** (`p% of whole = ?`) — the percentage analogue of rung 4's fraction-of-a-quantity warm-up and rung 8's own missing warm-up content (see gap below).
2. **Percent discount word problems** (sale price + amount saved) — already present and correctly pitched in `m_words`.
3. **Reverse-percentage bar model** (find the original price given a discounted price and the percent) — already present and correctly pitched in `m_bar`.
4. **A percent-specific misconception check** — confusing "take X% off" with "subtract X" is the single most common entry-level percentage error; did not exist before this task (see gap below).
5. **Continued long-multiplication fluency** (`longmul:1` carried over from rungs 6-7) — correctly retained, same rationale as rung 7's note.

## Verdict on current knobs/generators — dead-flag / narrow-reach audit

`LEVELS.sarah.maths[7] = {n:'Percentages', longmul:1, pct:1, s:3, bar:'pct', patt:'seq', chal:4}`.

Traced every reference to `L.pct` across all five generators before making any change:
- **`m_bar`**: `t==='pct'` branch already exists and is correctly reached (`bar:'pct'` matches exactly) — a genuine, well-pitched reverse-percentage bar model (find the original price from a discounted price). No change needed.
- **`m_words`**: first-block chain is `if(L.pct){...}else if(L.ratio){...}else if(L.frac){...}else if(L.dec){...}else if(L.algebra>=2){...}` — `L.pct` is checked **first** in the chain, so it always fires cleanly regardless of what else might be set (audited the full chain per the task's instruction; rung 8 sets no other competing flag from this list anyway, so there is no masking risk here even in principle for this rung's exact combination). One percent-discount word problem generated correctly. Verified the underlying arithmetic is always exact: `O=R(2,9)*10` (multiple of 10) and `pc∈{10,20,50}` (all divisors of 100 that keep `O*(100-pc)` a multiple of 100 for any multiple-of-10 `O`) — no rounding/fraction leakage.
- **`m_warm`**: the `specials` array only ever checked `frac`/`longmul`/`mul2` (pre-fix) — `L.pct` was **never checked anywhere in the warm-up generator**, so none of the six warm-up items could ever be a percent-of-a-quantity fact, despite percentages being the rung's headline new skill (the direct analogue of rung 4's fraction warm-up, which this rung had no equivalent of).
- **`m_mistake`**: bank selection `(L.chal>=3)?hard:easy` — rung 8's `chal:4` selects `hard` (same bank as rungs 5-7's `chal:3`, since the gate is `>=3` not an exact match). The `hard` bank has no percentage-specific item at all — same class of gap already found and fixed at rungs 4 (fractions), 5 (decimals), and 7 (ratio).
- **`m_pattern`**: `patt:'seq'` — topic-orthogonal to percentages, consistent with the ladder's established design (same reasoning as rung 7's note; pattern-hunter is a deliberately separate skill track ladder-wide).

Coverage before this fix: `L.pct` touched 2 of 6 blocks (`m_words`, `m_bar`) — identical shape to rung 7's pre-fix gap. After adding warm-up and mistake-spotting coverage, `L.pct` now reaches 4 of 6 blocks, matching the coverage level established by the rungs 4/5/7 fixes.

## Changes made in `growth-map.html`

1. **`m_warm`**: added `pct` to the `specials` array (`if(L.pct)specials.push('pct');`) and a new `kind==='pct'` case: percent-of-a-quantity, built from a fixed table of `[percent, divisor]` pairs (`[10,10],[20,5],[25,4],[50,2],[5,20],[75,4]`) where `whole = k*divisor` for a random integer `k`, guaranteeing `percent% of whole` is always an exact integer — no rounding, no decimals leaking into a rung that hasn't formally covered decimal×percent interaction. Additive-only — only rung 8 sets `L.pct`, so no other rung's warm-up is affected (verified below).
2. **`m_mistake`**: added a `pctBank` (one item: "25% of 60 = 60 − 25 = 35" — the classic novice error of treating "take X%" as an instruction to subtract the number X itself rather than compute a fraction of the whole) merged in via `if(L.pct)bank=bank.concat(pctBank);`, following the same additive pattern already used for `fracEasy`/`decBank`/`ratioBank`. Only rung 8 currently sets `L.pct`.

No existing function signatures or return shapes changed; `ws_maths_sarah`'s call signature is untouched. `m_bar`'s existing `pct` branch and `m_words`' existing percent word problem were left exactly as-is — both were already correctly calibrated and confirmed arithmetically exact.

## Verification

- `node --check` on the extracted `<script>` block: pass, no output.
- Generated `ws_maths_sarah(7)` (rung 8, 0-indexed) 200 times directly in Node: zero occurrences of `undefined`, `NaN`, or `[object Object]` across all 200 runs; spot-checked that every percent-of-a-quantity warm-up answer is an exact integer (no `.5` or other partial values) across all 200 runs.
- Confirmed content matches the rung's own label: warm-up mixes percent-of-a-quantity items with `longmul`-flavoured 2-digit×2-digit facts across trials; word-problem block includes the sale-discount problem; bar-model asks for the original price given a discounted price and percent; mistake-spotting block reachably includes the new percentage misconception item (confirmed present across repeated trials, alongside the pre-existing hard-bank items).
- Answer-key line count (6 lines: A-E + ★) matches the 6 generated blocks in every trial.
- Re-ran rungs 1-7, 9-12 (sharing `m_warm`/`m_mistake` with rung 8) 20 times each after this change: zero `undefined`/`NaN`/`[object Object]`, and confirmed none of them produce percent-flavoured warm-up or mistake-spotting content (none set `L.pct`), confirming the fix is fully scoped to rung 8.
- Ran the full 12-rung Sarah-maths sweep (20 samples each) after all three rungs' edits (5, 7, 8) were complete: all 12 rungs clean, plus a separate 10-rung Mirayah-maths sweep (10 samples each, unaffected by this task's edits since Mirayah's generators are entirely separate functions): all 10 rungs clean.
