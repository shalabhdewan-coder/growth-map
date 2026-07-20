# Mirayah — Maths — Rung 4 "Teen numbers"

**Blended grade-equivalent target:** SG MOE Primary Maths P2 / Saxon Math 2 early (per plan's rung-map table).

## Source check — what was actually read

**`sources/maths/moe-primary-maths-syllabus-2013.pdf`**, p.33, sub-strand Whole Numbers, "1. Numbers up to 100":
- **1.2** "number notation, representations and place values (tens, ones)"
- Learning experience (b): "work in groups using concrete objects to (i) **make a group of ten and count on from 10 to tell the number (less than 20)**, (ii) make groups of ten and count tens and ones to tell the number (more than 20), (iii) estimate the number of objects in a set before counting, (iv) make sense of the size of 100."
- Learning experience (c): "use concrete objects and the base-ten set to represent and compare numbers in terms of tens and ones."

Point (b)(i) is, word for word, the definition of a "teen number": one full ten plus some more ones, counted on from 10. This is the one syllabus line that most precisely matches this rung's name — and it's a **place-value** skill, not an addition-range skill.

**`sources/maths/ncert-class2-joyful-maths-ch01.pdf`** ("A Day at the Beach," verified full chapter) — "Fun with Blocks and Strips": "Can you tell how many blocks are there in this block stick? 1 block stick = ___ blocks," "1 ten strip = ___ units," then a table to complete: "___ block sticks and ___ blocks... ___ total blocks," with a worked table (Total blocks/units → Ten strips → Units, e.g. "24 → 2 → 4"). This is a direct concrete model for "split a number into tens and ones," including numbers in the teens (the chapter's own worked examples run from small totals up through 24, 36, 46).

**`sources/maths/ncert-class2-joyful-maths-ch06.pdf`** ("Decoration for Festival," verified full chapter) — a garland-counting activity that builds a table of "Number of [10s] / Number of [extra] / Total" (e.g. "Rohan: 1 ten + 2 ones = 10 + 2 = 12," "Suwali: 5 tens + 3 ones = 50 + 3 = 53") followed by an explicit **Tens (T) / Ones (O) column** worked example ("12 → T:1 O:2; 53 → T:5 O:3; sum 65"). This is Class-2-level (numbers into the 50s-60s, column addition of two 2-digit numbers), a stage above this specific rung's scope — but it confirms the tens/ones column format is the standard concrete representation this skill builds toward, which is why the new block below is framed the same way ("N = ___ tens and ___ ones") so it can scale cleanly into later rungs (5-7) without needing a second, different place-value block.

## What this rung should test

1. **Splitting a teen number (11-20ish) into 1 ten + some ones** (MOE 1.2 + learning experience (b)(i); NCERT ch.1's block-stick/ten-strip model) — this was **completely absent from the generated worksheet before this task**, see bug below.
2. Everything already correct from rung 3, held or slightly widened: `addMax:20`/`subMax:20` unchanged (correct — MOE's "more than 20" tens-and-ones counting, and the wider addMax/subMax ranges, are reserved for rung 5 "Within 100"), `count:[2,5,10]` (10s added — first skip-counting-by-10s in the ladder, which is itself a scaffold for "ten" being a countable unit, tying directly into the new place-value block), `story:1`, `shapes:1`.

## Verdict on current knobs/generator — dead-flag bug found and fixed

`LEVELS.mirayah.maths[3] = {n:'Teen numbers', addMax:20, subMax:20, count:[2,5,10], story:1, shapes:1, place:1}`.

**Bug:** `place:1` is set on this rung (and rungs 5, 6, 7 — `{n:'Within 100',...,place:1}`, `{n:'Story sums',...,place:1}`, `{n:'Regrouping',...,place:1}`), but **`ws_maths_mirayah(li)` never read `L.place` anywhere** — confirmed by grepping the whole file for `.place`/`L.place` before this fix: the only hits were the four knob-object literals themselves. This is exactly the dead-flag pattern this session has been finding across Sarah's rungs: a knob that's set but never checked. The practical effect: before this fix, rung 4 "Teen numbers" — the rung whose entire name promises place-value/teen-number content — generated a worksheet with **no place-value question at all**. Its only difference from rung 3 was one extra skip-counting step (10s); `addMax`/`subMax` are identical to rung 3 (both 20/20). That's a materially weak escalation for a rung with its own distinct name and target skill.

**Fix (in `ws_maths_mirayah`, `growth-map.html`):** added a new block, gated on `if(L.place)`, immediately after the existing two-step-story block:

```js
if(L.place){const cap=Math.max(19,Math.min(99,L.addMax||19)),n=R(11,cap),t=Math.floor(n/10),o=n%10;
 blocks.push({h:'I. Tens & ones',items:[n+' = ____ tens and ____ ones']});ans.push('I: '+t+' tens and '+o+' ones');}
```

Modelled directly on NCERT ch.1's block-stick/ten-strip table and ch.6's T/O column format above. The number range is derived from `L.addMax` (clamped to 19-99) rather than hardcoded, so it automatically stays "teen-ish" at this rung (`addMax:20` → range 11-20) and widens correctly at rungs 5-7 as their own `addMax` grows (matching the pattern already used for Sarah's `m_algebra`/`m_geometry`, which derive their ranges from rung knobs rather than hardcoding). This only affects rungs 4-7 (the only rungs with `place:1` set); no other rung's blocks or letters were touched, and no existing block/letter was renumbered — `I` was chosen as the next free letter after the existing `A`-`H`.

**Side-effect honestly flagged:** this fix also activates a (previously silent) block at rungs 5-7, which are not yet formally "done" in this task's own sequence (only rungs 1-4 are in scope this pass). Per the plan's own Phase 6 note, this kind of shared-function ripple onto not-yet-visited rungs is expected and accepted — those rungs' place-value range (up to their own `addMax`, e.g. rung 7's 99) will get their own calibration notes when their turn comes, but the block itself is already correctly wired for them.

## Verification

- `node --check` on the extracted `<script>` block: pass, no output.
- Generated 3 sample worksheets via node (`ws_maths_mirayah(3)`): all produced **7** blocks / 7 answer lines (A bonds-default, B add, C subtract, D count, F shapes, G story, **I tens & ones — new**), no `undefined`/`NaN`/`[object Object]`. Spot-checked the new block's answers by hand against its `n`: e.g. `17 = ____ tens and ____ ones` → `1 tens and 7 ones`, arithmetic correct (`Math.floor(n/10)`/`n%10`).
- Also re-ran `ws_maths_mirayah` for two synthetic post-ceiling rungs produced by `intensifyRung` (tiers 1 and 2 cloned from rung 10 "Bridge to big-girl") to confirm the new block doesn't break ladder-extension: both tiers rendered cleanly, `addMax` scaled 99→114→149 as expected, `place` (not set on rung 10, so N/A here) confirmed absent/no-op as expected — see rung-agnostic boost note in the final report.
