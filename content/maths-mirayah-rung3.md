# Mirayah — Maths — Rung 3 "Add & take to 20"

**Blended grade-equivalent target:** SG MOE Primary Maths P1-P2 / CBSE-NCERT Class 1 upper (per plan's rung-map table).

## Source check — what was actually read

**`sources/maths/moe-primary-maths-syllabus-2013.pdf`**, p.34, sub-strand Whole Numbers, topic "2. Addition and Subtraction":
- **2.5** "adding and subtracting within 100" (2.5a: "within 20" listed as the sub-step before "within 100")
- **2.7** "solving 1-step word problems involving addition and subtraction within 20" — this is the exact skill-and-range this rung's name promises, and it's the first place the syllabus calls for a *word problem* rather than a bare number sentence, confirming this rung is the right place for the ladder's first story-sum block.
- Learning experience (b): "compare two numbers within 20 to tell how much one number is greater (or smaller) than the other by subtraction" — subtraction-as-comparison, which is what the existing take-away block already drills.
- Learning experience (e): "achieve mastery of basic addition and subtraction facts within 20 through playing a wide range of games" — supports both add (B) and subtract (C) firing together at this rung, unlike rungs 1-2 where subtraction was absent.

**`sources/maths/ncert-class2-joyful-maths-ch10.pdf`** ("Fun at the Fair," 10 pages, verified full chapter) — money/word-problem chapter: "Her mother gave her ₹50... Money given by Rupal's mother − Money spent = Money left," and a worked running-subtraction chain ("₹8 is left... spent ₹20 on rides = ₹28... spent ₹20 more = ₹48... spent ₹2 more = ₹50... Total spent = ₹42"). This chapter's number range (up to ₹50) is a stage *above* this rung's `addMax:20`/`subMax:20` — appropriate, since Ch.10 is Class-2-level and this rung blends toward the lower P1-P2 end — but its "start with a total, take away one step at a time, ask what's left" structure is exactly the shape of the existing story-sum block's subtract-branch (`had X, ate Y, how many left`), confirming that shape is correctly sourced even though the exact ₹-range differs.

## What this rung should test

1. **Addition AND subtraction, both firing together, capped at 20** (MOE 2.5/2.7) — existing blocks B (`addMax:20`) and C (`subMax:20`, first time `subMax` is set anywhere in the ladder).
2. **The first 1-step word problem ("story sum")** (MOE 2.7 is literally titled this) — existing block G, `story:1` (first non-zero `story` value in the ladder), matching NCERT ch.10's "how many left" framing.
3. **Skip-counting continues at 2s/5s** (`count:[2,5]`, unchanged from rung 2) — reasonable hold-steady; 10s are deliberately introduced next at rung 4 alongside teen-number/place-value content (see that rung's note), not here.
4. **Shapes continue** (block F).

## Verdict on current knobs/generator

`LEVELS.mirayah.maths[2] = {n:'Add & take to 20', addMax:20, subMax:20, count:[2,5], story:1, shapes:1}` — **well-calibrated, no code changes needed.**

- `subMax:20` turning on block C for the first time, at exactly the rung named "...& take to 20," is correct and source-matched (MOE 2.5a: within-20 first).
- `story:1` turning on block G for the first time is correct and matches MOE 2.7's explicit "1-step word problems... within 20" framing — the block's own number ranges (`a=R(3,9)+b=R(2,6)` for the add-story, capping the sum at 15, and `a=R(6,12)−b=R(1,5)` for the subtract-story) stay safely inside 20, consistent with the rung's cap.
- **Dead-flag audit for this rung's knobs:** `addMax`, `subMax`, `count`, `story`, `shapes` all read in `ws_maths_mirayah` — no unused knob at rung 3.
- `bonds:10` is **absent** from this rung's object for the first time (rungs 1-2 both set it). Confirmed this is not a regression: block A's own code (`const b = L.bonds||10; ...`) already defaults to 10 whenever `bonds` is unset, so the bonds-to-10 warm-up still runs every time — the knob's omission is cosmetic (bonds work continues via the same default it would use anyway), not a dropped skill.

## Verification

- `node --check` on the extracted `<script>` block: pass, no output.
- Generated 3 sample worksheets via node (`ws_maths_mirayah(2)`): all produced 6 blocks / 6 answer lines (A bonds-default, B add, C subtract, D count, F shapes, G story), no `undefined`/`NaN`/`[object Object]`.
- No `growth-map.html` change required for this rung; committing the content note only.
