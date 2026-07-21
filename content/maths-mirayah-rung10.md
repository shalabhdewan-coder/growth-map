# Mirayah — Maths — Rung 10 "Bridge to big-girl"

**Blended grade-equivalent target:** Math Kangaroo Pre-Ecolier (per plan's rung-map table) — the final Mirayah maths rung; after this she is at the ceiling of her ladder (further practice happens via the existing generic level-up-ceiling synthesis, not a new hand-authored rung).

## Source check — what was actually read

**`sources/maths/mathkangaroo-preecolier-sample.pdf`** (International Math Kangaroo Competition, Pre Ecolier — Grade 1, Set A, 24 questions, `pdftotext` text layer confirmed readable — this is the exact source the plan's rung-map names for this rung) — read the full 24-question set (Sections A/B/C, 3/4/5-point tiers). Relevant findings:
- The paper is overwhelmingly **spatial/logic-puzzle** in character (picture-piece assembly, shape counting, pattern completion, cube-counting, flipped-card orientation, taller/shorter arrow-chain reasoning) — genuinely different in *kind* from anything on this ladder so far, not just harder arithmetic. Confirms the rung's own name "Bridge to big-girl" is really about a step up in reasoning style, not only bigger numbers.
- **Q16 (Section B, 4-point):** *"Grandmother has just baked 12 cookies. She wants to give all of the cookies to her 5 grand-children but also wants to give each of the grandchildren the same number of cookies. How many more cookies should she bake?"* — an equal-sharing-with-remainder problem (12÷5 doesn't divide evenly; answer is how many more are needed to reach the next multiple of 5). This is a genuinely new reasoning shape versus anything already on the ladder (division-with-remainder, phrased as "how many more," not "how many each").
- **Q12 (Section B):** two flowers, sum of petal numbers equal, one petal hidden — solve for the missing addend via balancing two totals. This is missing-addend reasoning at a slightly more abstract level than rung 6's missing-addend story sum (numbers presented structurally, not narratively).
- **Section A Q1:** *"The kangaroo goes up 3 steps each time the rabbit goes down 2 steps. On which step do they meet?"* — a step-counting/meeting-point problem, directly related to the number-line skip-counting skill this ladder has built since rung 3 (regular +2/-2 style jumps), just applied as a genuine reasoning puzzle instead of a drill.

**Given this rung is the final one and its own source is squarely a reasoning/logic-puzzle competition paper (not a repeat of the arithmetic-drill format), the honest scope decision is:** keep the existing block structure (A-E, G, H — all now properly sourced/fixed per rungs 8-9's files) as the arithmetic backbone, and use the Kangaroo source specifically to differentiate **block H's rung-10 shape** — turning the plain add/subtract two-step into a multiply-then-subtract chain that previews Sarah's own rung 1 two-step shape (see `content/maths-sarah-rung1.md`, which documents that exact "multiply then subtract" pattern as one of Sarah's two two-step shapes, sourced there to NCERT Class 3). This is the literal meaning of "bridge to big-girl" per the plan: rung 10 should start resembling what rung 1 of the *next* ladder looks like, not just be "rung 9 with bigger numbers."

**Honesty note:** the Q16 "how many more to share exactly" and Q12 "balance two totals" puzzle shapes are **not** implemented as new blocks this pass — adding them would mean inventing a brand-new `m_*`-style generator function for this single rung alone (out of scope for a dead-flag-audit + knob-recalibration task, and risks becoming a bigger surface than can be verified in this pass). They're recorded here as a legitimate follow-up if this ladder gets a second editing pass, backed by a real citation, not fabricated.

## What this rung should test

1. Everything from rung 9, all fixed knobs carried forward (background add/subtract, groups/multiplication, skip-counting, story sums, two-step).
2. **A harder two-step shape** that combines multiplication (the skill this 3-rung block introduced) with subtraction — bridging toward Sarah's rung 1 "multiply then subtract" two-step shape.
3. **A wider `count` array** (`[3,4,6]`, unchanged from before this task) for skip-counting/grouping — `3` is directly modelled in NCERT ch.3 ("count in threes... jump on 24?", p.26, already cited at rung 8); `4` and `6` are **not** explicitly modelled by name in ch.3's own examples (only 2/3/5/7/10 appear there) — noted honestly rather than claiming a citation ch.3 doesn't actually contain. Extending skip-counting to 4s/6s by analogy from the same number-line-jump mechanic ch.3 teaches for 2/3/5/7/10 is standard, uncontroversial pedagogy, not a fabricated source.

## Verdict on current knobs/generator

`LEVELS.mirayah.maths[9] = {n:'Bridge to big-girl', addMax:99, subMax:99, count:[3,4,6], story:2, shapes:0, mul:1, twostep:1}` — **knob change made:** `twostep:1` → `twostep:2`, so this rung's block H hits the new differentiated branch (rung 9 stays at `twostep:1`, plain add/subtract chain). Also added `mulMax:90` (this rung's own group-multiplication ceiling — see rationale below).

**`mulMax` progression across rungs 8-10, stated plainly (general-knowledge extension beyond rung 8's literal MOE citation, honestly flagged as such):** rung 8 = `40` (the literal MOE 3.3 number). Rungs 9-10 are, per the plan's own rung-map, explicitly "bridge" territory beyond P1 scope, so there is no equivalent MOE-P2/P3 document in the whitelist to cite a harder ceiling from (same gap already logged for two-step at rung 9). Chose `60` (rung 9) and `90` (rung 10, approaching but not exceeding the ladder's own `addMax:99` ceiling so a "groups" total never outgrows what block B/C are already producing) as a smooth, defensible progression — not arbitrary per-run randomness, a fixed authored value like every other knob on this ladder.

**Block H, rung-10 branch (`L.twostep>=2 && L.mul`):**
```
const arr=L.count||[2,5,10],per=P(arr),g=R(3,6),total=g*per,give=R(2,Math.max(2,Math.min(total-3,20)));
blocks.push({h:'H. Two-step story',items:[P(NAMES)+' packs '+g+' boxes with '+per+' pencils in each box, then gives away '+give+' pencils. How many pencils are left?  ____'],write:1});
ans.push('H: '+(total-give));
```
`per` drawn from this rung's own `count:[3,4,6]` (not a repeat of rung 8/9's `[2,5,10]`, genuinely different practice), `g=R(3,6)` kept small/concrete, `give` algebraically bounded below `total` so the subtraction is always valid (checked: `total` ranges `3×3=9` to `6×6=36`; `give<=min(total-3,20)`, so `total-give>=3` always, no negative/zero-degenerate results).

## Verification

- `node --check` on the extracted `<script>` block: pass, no output.
- Generated 8 sample worksheets via `ws_maths_mirayah(9)`: all produced 7 blocks (A,B,C,D,E,G,H) / 7 answer lines, no `undefined`/`NaN`/`[object Object]`.
- **Block H now visibly differs from rung 9's shape** across 8 samples — multiplication-then-subtraction chains, e.g. `4 boxes with 6 pencils each = 24, gives away 9, left = 15`; `5 boxes with 4 pencils each = 20, gives away 12, left = 8`; `6 boxes with 3 pencils each = 18, gives away 6, left = 12` — confirming the `twostep:2` gate actually routes to the new branch and rung 9's plain add/subtract branch is not reused here.
- Block E (groups) confirmed capped at this rung's `mulMax:90` with `per` drawn from `[3,4,6]` in every sample (e.g. `8 baskets have 4 apples each = 32`, `6 baskets have 6 apples each = 36`) — no product observed exceeding 90 across 8 samples (theoretical max for this rung's `arr`/`g` combination is well under the cap, as expected from the filter logic).
- **Full-ladder regression (rungs 8-10 together, this task's closing requirement folded in early since all three share edited code paths):** all 3 rungs re-generated 8× each after the block-E/block-H rewrite — no failures, no dead answer/block-count mismatches, no cross-rung contamination (rung 8 never hits the `twostep` branch at all since the flag is unset there; rung 9 never hits the `twostep>=2` branch; rung 10 is the only one that does).

## Addendum — Block A "Number bonds to 100" plateau fixed (later audit pass)

Same plateau bug documented in rung 7's file — this was the last and most conspicuous rung in the flat
run (the ladder's final rung, previously indistinguishable from rung 7's Block A three rungs earlier).
**Fix:** `bondsForm:2` (tens-jump + ones-jump bridging, NCERT ch.6 p.53-54) plus `bondsReps:9` (the
ladder's highest bonds-fluency rep count, up from rung 9's 8) — the terminal rung gets the most practice
at the hardest bonds skill, consistent with every other block's own escalation curve peaking here.
Verified via live generation: Block A produces 9 bridging items at this rung; before this fix, rungs
7-10 were byte-for-byte-identical in shape (`x+____=100`, always 6 items) — now every one of the four
back-half rungs is distinguishable by item count and all four share the qualitatively harder skill
instead of the trivial single-blank complement.
