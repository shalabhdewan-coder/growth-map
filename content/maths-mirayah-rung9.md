# Mirayah — Maths — Rung 9 "Two-step (little)"

**Blended grade-equivalent target:** "Bridge into Sarah rung 1 territory" (per plan's rung-map table — this is the one Mirayah rung explicitly defined *relative to Sarah's ladder* rather than a curriculum grade, so its sourcing job is different from every other rung: confirm what's genuinely coverable from the whitelisted Mirayah sources, and be explicit about the rest).

## Source check — what was actually read / re-checked

**`sources/maths/moe-primary-maths-syllabus-2013.pdf`** — re-checked with `pdftotext` across the entire "PRIMARY ONE" section (the document's only section, confirmed again per rung 8's file) for any two-step / multi-step word-problem language: **none found.** The only word-problem learning experience in the whole document is 2.7 "solving **1-step** word problems involving addition and subtraction within 20" (p.33) and 3.5 "solving **1-step** word problems involving multiplication and division" (p.34) — both explicitly capped at one step. This confirms, rather than assumes, that genuine two-step reasoning is a P1-syllabus gap, not something this document quietly covers.

**`sources/maths/ncert-class2-joyful-maths-ch01.pdf` through `ch11.pdf`** — none of the 11 NCERT Class 2 chapters (already read across rungs 1-8 of this ladder) contain a two-step word problem either; NCERT Cl.2's word problems are consistently single-operation (ch.6's carrying/borrowing story sums, already the basis for this rung's `story:2` block, are each one operation).

**Cross-reference (not a new primary source, an honesty-preserving pointer):** `content/maths-sarah-rung1.md` — already-committed rung, quotes NCERT Class 3 *Maths Mela* ch.7 "Raksha Bandhan" (multiplication/equal-grouping) and ch.8 "Fair Share" (division/equal-sharing) as the source for Sarah's own two-step word problems, and explicitly logs that its own SG-P3-specific 2-step number ranges are "general knowledge, no source file" (Singapore's P3 textbooks are off-limits under the same hard constraint that applies here). So even Sarah's own rung 1 — the destination this Mirayah rung is bridging toward — isn't fully curriculum-sourced for its two-step shape; it's sourced for *what topic* the two-step draws on (multiply/divide fact families) but not for the specific two-step sentence structure.

**Verdict, stated plainly (per this task's explicit instruction not to fabricate a citation that isn't real):** there is no NCERT Cl.2 / MOE-P1 / Math-Kangaroo-Pre-Ecolier source in the whitelist for this task that models a *two-step add/subtract word problem* at this exact age. The existing block H sentence shape ("has A, buys B more, gives C away") is **general-knowledge scaffolding** — a standard early-years two-step pattern (chain of concrete actions on a single quantity), not attributable to any specific page in the sourced material. What *is* sourced is: (a) the individual operations it chains together (addition/subtraction within 99, per rungs 5-7's own citations) and (b) the naming convention "little" for this rung matching the plan's explicit "bridge into Sarah rung 1, but the little/gentle version" framing — i.e. add/subtract-only, no multiplication yet (that's saved for rung 10, see that rung's file).

## What this rung should test

1. **Two-step add-then-subtract chains**, scaled to the rung's actual `addMax:99`/`subMax:99` ceiling (not a fixed small range regardless of level — see bug below).
2. Everything from rung 8 stays active and unchanged: groups/multiplication (`mul:1`, now within `mulMax`), skip-counting, story sums tier 2, background add/subtract at full range.

## Verdict on current knobs/generator — dead-flag bug found and fixed (the flag named in this task's brief)

`LEVELS.mirayah.maths[8] = {n:'Two-step (little)', addMax:99, subMax:99, count:[2,5,10], story:2, shapes:0, mul:1, twostep:1}` — knobs otherwise fine, **generator bug found and fixed in block H** (`twostep:1`, this rung's own new flag).

**Bug found:** `L.twostep` block H existed and *was* read — not fully dead — but its magnitude was completely disconnected from the rung's own `addMax:99`/`subMax:99` knobs:
```
if(L.twostep){const a=R(3,8),b=R(2,6),c=R(1,5);...}
```
`a`, `b`, `c` were hardcoded to single-digit ranges (max possible result `8+6-1=13`) regardless of the rung declaring a ceiling of 99. This is **exactly the same bug class already found on `story` at rung 6 and `place`-adjacent issues earlier in this ladder**: a flag that is read (not silently ignored) but whose generated numbers don't actually track the rung's declared difficulty knob, so the worksheet a child sees at "Two-step (little), within 99" looks numerically identical to whatever a hypothetical much-earlier rung with the same flag would have produced. Confirmed via direct generation before the fix: 8 samples, max total value seen was 13, nowhere near the 99 ceiling every other block on this same worksheet (A/B/C/D/E) is actually using.

**Fix:** magnitude now derives from `cap=Math.max(L.addMax||30,L.subMax||30)` — `a=R(20,~cap*0.5)`, `b=R(5,~cap*0.25)`, `c=R(1,min(a+b-5,~cap*0.3))`, guaranteeing `c<a+b` (positive result) algebraically for every value `cap` can take on this ladder, not by luck. This also future-proofs the block for the ladder's level-up-ceiling synthesis logic (rungs beyond 10 clone+intensify `addMax`/`subMax`, and this block now actually respects whatever those synthesized values are).

```
if(L.twostep){
 const cap=Math.max(L.addMax||30,L.subMax||30);
 if(L.twostep>=2 && L.mul){ /* rung 10 only — see that rung's file */ }
 else{
  const a=R(20,Math.max(21,Math.floor(cap*0.5))),b=R(5,Math.max(6,Math.floor(cap*0.25))),sum=a+b,c=R(1,Math.max(1,Math.min(sum-5,Math.floor(cap*0.3))));
  blocks.push({h:'H. Two-step story',items:[P(NAMES)+' has '+a+' stickers, buys '+b+' more, then gives '+c+' away. How many now?  ____'],write:1});
  ans.push('H: '+(a+b-c));
 }
}
```
The `L.twostep>=2` branch is rung 10's differentiated shape (this rung stays on the plain `else` branch since `twostep:1<2`) — see `content/maths-mirayah-rung10.md` for that half.

## Verification

- `node --check` on the extracted `<script>` block: pass, no output.
- Generated 8 sample worksheets via `ws_maths_mirayah(8)`: all produced 7 blocks (A,B,C,D,E,G,H — F/I still skipped, `shapes:0`/`place` unset) / 7 answer lines, no `undefined`/`NaN`/`[object Object]`.
- **Block H magnitude now tracks the 99 ceiling** across 8 samples (e.g. `29 stickers + 18 more − 8 away = 39`, `44 + 22 − 25 = 41`, `24 + 6 − 9 = 21`) — a visible, order-of-magnitude jump from the pre-fix single-digit range, and every sample's `c<a+b` (no negative results).
- Block E (groups) verified still capped correctly at this rung's own `mulMax:60` (see rung 10's file for why 60, not 40) — e.g. `5 baskets have 10 apples each = 50`, `6 baskets have 5 apples each = 30`, both ≤60.
