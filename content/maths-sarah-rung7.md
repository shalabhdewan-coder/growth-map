# Sarah — Maths — Rung 7 "Ratio & proportion"

**Blended grade-equivalent target:** SG MOE Primary Maths P5-P6 / Saxon Math 7/6 scope (per plan's rung-map table).

## Source check — what was actually readable

**`sources/maths/ncert-class6-ganita-prakash.pdf`** — `pdfinfo` confirms 20 pages (front-matter/TOC truncation, same as every prior rung's note). Visible Contents page reaches only as far as **Ch.7 "Fractions" p.151** before the file cuts off — no ratio-titled chapter is visible in the truncated range, and grepping the extractable text for "ratio" returns zero hits. Honest gap: cannot cite a specific NCERT ratio chapter from this file; noted rather than invented.

**`sources/maths/moe-primary-maths-syllabus-2013.pdf`** — as established in every earlier rung's note, this document is P1-content-only; grepped specifically for "ratio" this pass — zero hits (unsurprising, ratio is an upper-primary strand per the rung-map, well past what this P1-only document covers).

**`sources/maths/amc8-compendium-1985-2026.pdf`** (307-page official AJHSME/AMC 8 archive, legitimate source) — searched the full extracted text for "ratio" and "percent" to find genuine calibration points for how a real competition treats ratio as an entry-level skill (not just a curriculum-recall topic):
- **1989 AJHSME, Problem 9** (`https://artofproblemsolving.com/wiki/index.php/1989_AJHSME_Problems`, confirmed via the compendium's own embedded page-source URL): *"There are [x] boys for every [y] girls in Ms. Johnson's math class. If there are [n] students in her class, what percent of them are boys?"* — the specific numeric values were stripped by the PDF's image-rendered math (same limitation already documented in rung 12's note for other AMC 8 years), but the **problem shape** is fully legible and is directly useful: it's problem **9** of the paper (early/easy tier, per rung 12's finding that AMC 8/AJHSME's first third is consistently entry-difficulty), and it explicitly chains ratio → percent in a single problem. This is real, first-hand evidence that ratio-to-percent conversion is treated as an *entry-level* competition skill, not an advanced one — which directly supports the rung-map's own sequencing (ratio at rung 7, immediately followed by percentages at rung 8, is the right order: percentages are naturally taught as "a ratio out of 100").
- **1995 AJHSME, Problem 1**: *"Walter has exactly one penny, one nickel, one dime and one quarter in his pocket. What percent of one dollar is in his pocket?"* — Problem 1 (the easiest slot on the paper), money + percent, no ratio notation needed but confirms money-flavoured percent/ratio-of-a-whole problems are treated as genuinely easy, entry-tier material — useful corroboration for keeping this rung's ratio word problems in a money/sharing context (as the existing `m_words` ratio branch already does) rather than jumping to abstract ratio notation.

**Saxon Math 7/6 scope** — **(general knowledge, no source file, per the hard constraint — not opened)** — used only to corroborate that ratio/proportion as a named, dedicated strand (simplifying ratios, equivalent ratios, sharing a quantity in a given ratio) is squarely Saxon-7/6-territory, appropriate as this rung's headline new skill, building on the long-multiplication fluency Saxon 7/6 also continues to drill (matching the existing `longmul:1` knob carried forward from rung 6).

## What this rung should test

1. **Ratio fact fluency** — simplifying/recognising equivalent ratios, the ratio analogue of the "3-12 times table" fluency that anchors rung 1's warm-up. This did not exist anywhere on the ladder before this task (see gap below).
2. **Ratio-sharing word problems** (share a total in a given ratio) — already present and correctly pitched in `m_words`.
3. **Ratio bar-model** (draw equal boxes to find each share) — already present and correctly pitched in `m_bar`.
4. **A ratio-specific misconception check** — additive vs. multiplicative reasoning ("double the people, add instead of multiply the ingredient") is the single most well-documented ratio misconception at this level; did not exist before this task (see gap below).
5. **Continued long-multiplication fluency** (`longmul:1` carried over from rung 6) — correctly retained; ratio problems at this level still assume solid multi-digit multiplication as a prerequisite, and Saxon 7/6 keeps drilling it alongside the new ratio content, so keeping `longmul` here (rather than dropping it) is the right call.

## Verdict on current knobs/generators — dead-flag / narrow-reach audit

`LEVELS.sarah.maths[6] = {n:'Ratio & proportion', longmul:1, ratio:1, s:3, bar:'ratio', patt:'seq', chal:3}`.

Traced every reference to `L.ratio` across all five generators before making any change:
- **`m_bar`**: `t==='ratio'` branch already exists and is correctly reached (`bar:'ratio'` matches exactly) — a genuine, well-pitched ratio bar model. No change needed.
- **`m_words`**: first-block `if(L.pct){}else if(L.ratio){}else if(L.frac){}else if(L.dec){}else if(L.algebra>=2){}` — rung 7 sets only `ratio` among these, so it fires cleanly with no masking (checked the full chain per the task's instruction to audit if/else priority for this rung's exact combination — `pct` is not set on rung 7, so no earlier branch can steal priority). One ratio-sharing word problem generated correctly.
- **`m_warm`**: the `specials` array only ever checked `frac`/`longmul`/`mul2` (pre-fix) — `L.ratio` was **never checked anywhere in the warm-up generator**, so none of the six warm-up items could ever be ratio-flavoured, despite ratio being the rung's headline new skill (the direct analogue of rung 1's "Tables" fact-fluency warm-up, which this rung has no equivalent of).
- **`m_mistake`**: bank selection `(L.chal>=3)?hard:easy` (rung 7's `chal:3` selects `hard`) has no ratio-specific item at all — same class of gap already found and fixed at rung 4 (fractions) and rung 5 (decimals).
- **`m_pattern`**: `patt:'seq'` (quadratic-flavoured `n²+b` sequence) is topic-orthogonal to ratio, consistent with the ladder's established design where the pattern-hunter block is a separate skill track not required to match each rung's headline flag (already confirmed as the intended design in rung 2's and rung 4's notes — e.g. rung 3's `patt:'geo'` is unrelated to "three-step problems"). No change needed or expected here.

Coverage before this fix: `L.ratio` touched 2 of 6 blocks (`m_words`, `m_bar`) — narrower than ideal for the rung's headline flag, though not as severe as rung 5's pre-fix 1-of-6. After adding warm-up and mistake-spotting coverage, `L.ratio` now reaches 4 of 6 blocks, matching the coverage level established by the rung 4/5 fixes (pattern-hunter and the always-present change/steps problems are deliberately left topic-agnostic, consistent with ladder-wide precedent).

**Bug found and fixed during warm-up implementation (not a dead-flag masking bug, a correctness bug introduced while writing the new content — caught by testing, not by inspection):** the first draft of the new ratio warm-up item picked `x` from `[2,3,4]` and `y` from `[3,5,7]` independently, then displayed `x*k : y*k` and claimed the answer `x:y` was "simplest form." Both pools can produce `3`, so `x=3,y=3` was reachable, producing e.g. `"15 : 15 in simplest form = ____ : ____"` with the claimed answer `"3:3"` — which is **not actually simplest form** (gcd(3,3)=3, true simplest form is `1:1`). Caught via a 200-iteration automated check that verifies `gcd(x,y)===1` for every generated ratio warm-up item. Fixed by making the `y` pool exclude `3` whenever `x===3` (`P(x===3?[5,7]:[3,5,7])`), which guarantees `gcd(x,y)=1` for every possible combination (verified: `x=2` with any `y∈{3,5,7}` → all coprime since `y` is always odd; `x=3` with `y∈{5,7}` → coprime by construction; `x=4` with any `y∈{3,5,7}` → coprime since `y` is always odd and `4=2²` has no odd factors). Re-ran the 200-iteration check after the fix: zero non-simplest-form results.

## Changes made in `growth-map.html`

1. **`m_warm`**: added `ratio` to the `specials` array (`if(L.ratio)specials.push('ratio');`) and a new `kind==='ratio'` case: an equivalent-ratio simplification item (`x*k : y*k` in simplest form, with the coprimality guarantee described above). Additive-only — only rung 7 sets `L.ratio`, so no other rung's warm-up is affected (verified below).
2. **`m_mistake`**: added a `ratioBank` (one item: the "double the people, add instead of multiply the sugar" additive-vs-multiplicative misconception, the single most standard ratio-scaling error at this level) merged in via `if(L.ratio)bank=bank.concat(ratioBank);`, following the same additive pattern already used for `fracEasy`/`decBank`. Only rung 7 currently sets `L.ratio`.

No existing function signatures or return shapes changed; `ws_maths_sarah`'s call signature is untouched. `m_bar`'s existing `ratio` branch and `m_words`' existing ratio word problem were left exactly as-is — both were already correctly calibrated.

## Verification

- `node --check` on the extracted `<script>` block: pass, no output.
- Generated `ws_maths_sarah(6)` (rung 7, 0-indexed) 200 times directly in Node: zero occurrences of `undefined`, `NaN`, or `[object Object]` across all 200 runs; zero non-coprime "simplest form" claims across all 200 runs (see bug-and-fix above).
- Confirmed content matches the rung's own label: warm-up mixes ratio-simplification items with `longmul`-flavoured 2-digit×2-digit facts across trials; word-problem block includes the ratio-sharing problem; bar-model asks for each team's score given a total and a ratio; mistake-spotting block reachably includes the new ratio misconception item (confirmed present across repeated trials).
- Answer-key line count (6 lines: A-E + ★) matches the 6 generated blocks in every trial.
- Re-ran rungs 1-6, 8-12 (sharing `m_warm`/`m_mistake` with rung 7) 20 times each after this change: zero `undefined`/`NaN`/`[object Object]`, and confirmed none of them produce ratio-flavoured warm-up or mistake-spotting content (none set `L.ratio`), confirming the fix is fully scoped to rung 7.
