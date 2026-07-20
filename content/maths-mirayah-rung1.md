# Mirayah — Maths — Rung 1 "Counting play"

**Blended grade-equivalent target:** SG MOE Primary Maths P1 / CBSE-NCERT Class 1 / Saxon Math 1 (per plan's rung-map table).

## Source check — what was actually read

**`sources/maths/moe-primary-maths-syllabus-2013.pdf`** (Singapore MOE Primary Mathematics syllabus, 2013 ed.) — as Sarah's rung-1 note already established, this file only contains detailed content for **Primary 1** (the cover page states P2-P6 "will be updated accordingly" but never was in this download). That's actually ideal for Mirayah, since her whole ladder targets P1-P2. Verified content, pp.33-35, sub-strand "Whole Numbers," topic "1. Numbers up to 100":
- **1.1** "counting to tell the number of objects in a given set"
- **1.6** "patterns in number sequences"
- **1.7** "ordinal numbers (first, second, up to tenth) and symbols"
- Learning-experience note (a): "use number-bond posters and make number stories to build and consolidate number bonds for numbers up to 10"
- Learning-experience note (b): "make a group of ten and count on from 10 to tell the number (less than 20)... make groups of ten and count tens and ones to tell the number (more than 20)... estimate the number of objects in a set before counting."
- Sub-strand Geometry, 1.1: "recognise, name and describe the 4 basic 2D shapes (rectangle, square, circle and triangle)."

**`sources/maths/ncert-class2-joyful-maths-ch01.pdf`** ("A Day at the Beach," 15 pages, verified full chapter body, not a stub) — this Class-2 chapter is actually pitched at exactly this counting-in-groups skill: "Look at the above picture. Count and write the number of objects... How did you count them? Did you count them one by one or in bunches or groups?", then moves into block-sticks/ten-strips ("1 block stick = ___ blocks", "1 ten strip = ___ units") for counting in tens. Confirms counting-in-groups (not just one-by-one) is core early-numbers content, and that the "count in steps" skill and "tens & ones" skill are taught side by side from the very first counting chapter — relevant context for rung 4 below, not just this rung.

**`sources/maths/ncert-class1-joyful-maths.pdf`** — per the task's warning, this specific file is the **old truncated stub** (confirmed: `pdfinfo` reports only 14 pages, which is front-matter/Contents only — no chapter bodies). The verified **Contents page** (real, not fabricated) is still useful as topic-naming evidence: Ch.3 "Mango Treat (Numbers 1 to 9)," Ch.4 "Making 10 (Numbers 10 to 20)," Ch.5 "How Many? (Addition and Subtraction of Single Digit Numbers)," Ch.6 "Vegetable Farm (Addition and Subtraction up to 20)" — this maps almost one-to-one onto Mirayah's own rung 1→2→3→4 naming, which is a strong independent confirmation that the existing rung sequence is well-ordered, even though I cannot cite exercise content from inside this specific PDF.

## What this rung should test

1. **Counting objects in bunches/groups, not just one-by-one** (MOE 1.1; NCERT ch.1's framing question) — the existing skip-counting block (D) already does this via `count:[2]`.
2. **Simple number-sequence patterns** (MOE 1.6) — same block D.
3. **The 4 basic 2D shapes** (MOE Geometry 1.1) — existing block F, and the item bank (`triangle/square/circle/rectangle` with the exact defining features MOE lists: sides, equal sides, no corners, 2 long+2 short sides) already matches this precisely.
4. **Number bonds to 10 as an early, playful introduction** (MOE note (a)) — existing block A. Since MOE explicitly scopes early bonds work to "up to 10," having `bonds:10` already anchored at rung 1 is correctly calibrated, not premature.
5. **Simple addition within 10** (existing block B, `addMax:10`) — MOE's "concepts of addition and subtraction" (2.1) is introduced from the very start of P1 alongside counting, so this is not out of scope for a "Counting play" rung — see honest caveat below on subtraction.

## Verdict on current knobs/generator

`LEVELS.mirayah.maths[0] = {n:'Counting play', addMax:10, bonds:10, count:[2], story:0, shapes:1}` — **well-calibrated, no code changes needed at this rung specifically.**

- Block A (bonds), B (add), D (count-in-steps), F (shapes) all fire and are correctly bounded by the P1-only MOE content above.
- `story:0` correctly suppresses the story-sum block (G) — word-problem-in-a-sentence is a step up appropriately deferred to rung 3 ("Add & take to 20"), matching the ladder's own naming, not MOE's exact pairing.
- No `subMax` set, so block C (subtract) never fires at rung 1 or rung 2. **Honest gap, deliberately not fixed here:** MOE 2.1 technically introduces addition and subtraction concepts together from the start, and 1.8's learning experience explicitly asks children to "write two addition facts and two subtraction facts for a given number bond." A stricter MOE-only reading would add light subtraction at rung 1. I did not make this change because the ladder's own naming scheme deliberately reserves "take away" for rung 3 ("Add & take to 20") as a distinct escalation step, and rungs 1-2 already carry real content (bonds + addition + skip-counting + shapes) — adding subtraction here would blur rung 3's distinguishing feature rather than sharpen rung 1's. Flagging as a considered-and-rejected change, not a silent omission.
- **Dead-flag bug audit for this rung's own knobs:** every knob on this rung (`addMax`, `bonds`, `count`, `story`, `shapes`) is read somewhere in `ws_maths_mirayah` — no unused knob at rung 1 itself. (The real dead flag found this session, `place`, first appears at rung 4 — see that rung's note.)

## Verification

- `node --check` on the extracted `<script>` block: pass, no output.
- Generated 3 sample worksheets directly via node (`ws_maths_mirayah(0)`): all produced exactly 4 blocks / 4 answer lines (A bonds, B add, D count, F shapes — C/G/H/I correctly absent), no `undefined`/`NaN`/`[object Object]` in any sample.
- No `growth-map.html` change required for this rung; committing the content note only.
