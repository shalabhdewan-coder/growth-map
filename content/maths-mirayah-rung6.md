# Mirayah — Maths — Rung 6 "Story sums"

**Blended grade-equivalent target:** SG MOE Primary Maths P2 / Beast Academy 2A (per plan's rung-map table — Beast Academy itself off-limits per the hard constraint, so this rung is sourced entirely from NCERT + MOE).

## Source check — what was actually read

**`sources/maths/ncert-class2-joyful-maths-ch06.pdf`** ("Decoration for Festival," verified full chapter — already partially used for rung 4's T/O columns, re-read in full for its word-problem section this time) — the chapter's "Let us Do" word-problem sets, e.g.:
- "Rahim made 43 runs in one match and 58 runs in another. How many runs he made in the two matches together?" (add, 2-digit + 2-digit)
- "Anushka collected 63 shells. She gave 26 shells to her brother. How many shells are left with her?" (subtract)
- "Kanika made 72 bangles. She sold 36 bangles. How many bangles are left with Kanika now?" (subtract)
- "**56 birds were sitting on the tree. A few more birds joined them. Now there are 87 birds. Find out the number of birds that came later.**" (missing-addend / part-part-whole — the *quantity added* is unknown, not the total)
- "Sarika spent ₹56 in the fair, while Manish spent ₹35 ... What is the total money they both spent?"

These are meaningfully different from the small within-20 stories at rungs 3-5: bigger 2-digit numbers throughout the 20s-90s, and — critically — the birds example is a **different reasoning structure**, not just a bigger version of "has X, gets Y more."

**`sources/maths/moe-primary-maths-syllabus-2013.pdf`**, p.34, 2.7 "solving 1-step word problems involving addition and subtraction within 20" sets the *floor* skill (already used at rung 3); this rung's job, per the rung-map, is to escalate that into P2-level word problems with bigger ranges and a wider variety of problem structure, which MOE's own MP3 process strand ("thinking skills... comparing, sequencing... use heuristics") supports as an explicit P1-P2 target independent of number size.

## What this rung should test

1. **Word problems using the rung's own addMax/subMax range** (not small fixed numbers regardless of rung) — this was the dead-flag bug, see verdict below.
2. **A third problem structure — missing addend** (ch.6's bird example) — genuinely new reasoning (find what was added, not just "add" or "subtract"), appropriate for a rung whose entire name is "Story sums" and whose whole point is variety/complexity of story, not just arithmetic range.
3. **Numbers scaled to this rung's own within-100 progression** — `addMax`/`subMax` raised alongside rung 5 (see that rung's note for the full 80→90→99 reasoning); this rung sits at 90.

## Verdict on current knobs/generator — dead-flag bug found and fixed

`LEVELS.mirayah.maths[5] = {n:'Story sums', addMax:60→90, subMax:60→90, count:[2,5,10], story:2, shapes:1, place:1}` (addMax/subMax raised as part of this task — see below).

**Bug (the main one):** `L.story` is set to `2` at this rung (and rung 7) versus `1` at rungs 3-5, clearly intended by the ladder's own authors as an escalation flag. But `ws_maths_mirayah`'s story block only ever checked `if(L.story)` — **truthy**, not the actual value — so rung 6 "Story sums," the rung whose entire *name* is about story-sum sophistication, generated **byte-for-byte the same** tiny within-15 balloon/sweet stories as rung 3. This is the exact dead-flag pattern named in this task's brief: a knob that varies across rungs but drives zero different behaviour. Confirmed by grepping the pre-fix file for every `L.story` read — there was exactly one, and it never branched on the numeric value.

**Fix:** the story block now branches on `L.story>=2`. Tier-1 rungs (3-5) keep the **exact original code path, unchanged** (no regression risk to already-audited rungs). Tier-2 rungs (6-7) get:
- `kind===0`: add, numbers scaled off `L.addMax` (`shells... found more`, ch.6-sourced context)
- `kind===1`: subtract, numbers scaled off `L.subMax` (`bangles... gave away`, ch.6-sourced)
- `kind===2`: **new** missing-addend structure (`N birds on a tree... a few more joined... now there are M... how many joined?`), answer computed as `total-start` — directly modelled on ch.6's bird word problem, word-for-word structure match.

All three kinds derive their number ranges from `L.addMax`/`L.subMax` rather than hardcoding, matching the pattern already established by rung 4's place-value fix — so this rung's story block will keep scaling correctly if `addMax`/`subMax` change again later without needing another code edit.

**Regroup interaction check (explicitly asked for in this task):** `L.regroup` is correctly *absent* from this rung's object — regrouping is reserved for rung 7 only, and confirmed by direct generation that blocks B/C at this rung never force carry/borrow (they use the plain branch), consistent with rung 7 being the sole place that skill is drilled.

## Verification

- `node --check` on the extracted `<script>` block: pass, no output.
- Generated 5 sample worksheets via `ws_maths_mirayah(5)`: all produced 7 blocks / 7 answer lines, no `undefined`/`NaN`/`[object Object]`. All three story kinds observed across samples: `"Kabir collected 57 shells and found 30 more... "` → 87 (add), `"Kabir had 67 bangles and gave away 53..."` → 14 (subtract), `"35 birds were sitting on a tree... now there are 89 birds. How many birds joined?"` → 54 (missing addend) — all hand-verified arithmetically correct, all within the rung's 90-cap.
- Full-ladder regression: all 10 Mirayah maths rungs re-generated 8× each after this rung's edit — no failures.
