# Mirayah — English — Rung 3 "First sentences"

**Knob:** `{n:'First sentences', wtier:'a', mini:1, write:'sent'}` — `LEVELS.mirayah.english[2]`.

Same architecture as rungs 1-2. Rung 3 is the first rung to set `mini:1`, turning block D on (`if(L.mini){...push block D from P(MINISTORY)...}`); `rhyme` is unset so block C is skipped; `wtier:'a'` still shares `SIGHT.a` with rungs 1-2; `write:'sent'` pulls `MWRITE.sent` ("Write TWO sentences about what you did today").

## What rung 3 tests

`mini:1` → **MINISTORY**, the mini-story comprehension bank (a story text `t` + 2-3 questions `q` + matching answers `a`, picked at random via `P(MINISTORY)`). This is the biggest content gap flagged in the task brief: rungs 3, 4, and 5 (and further rungs 6-9 at `mini:2`) *all* draw from the same single `MINISTORY` array — before this task, only 3 stories existed for the entire back half of the ladder.

## What was added (this task's core deliverable — shared across rungs 3-5)

**`MINISTORY`: 3 → 9 stories.** New 6, split into two groups:

**NCERT-grounded (4 new, real sources shortened to age-6 length, 3-5 sentences):**
- "Three Little Pigs" (Sonu/Monu/Gonu) — condensed from the full multi-page story in `sources/english/ncert-class1-english-marigold.pdf` down to 4 sentences while keeping the plot beats (straw/sticks/bricks, wolf blows two down, brick house survives).
- "OUT! OUT!" (Jeet & Babli's bat-and-ball game) — condensed from `sources/english/ncert-class2-english-mridang-ch02.pdf`'s "Picture Reading" chapter (the ball is lost in a locked garden, Babli makes a new ball from rags/paper/string, the game restarts).
- "Seeing without Seeing" (Onshangla & her mother Ava) — condensed from `sources/english/ncert-class2-english-mridang-ch04.pdf`; kept the empathy/kindness core (a new blind classmate; Ava plays a blindfold guessing game with a rose to help Onshangla understand) while dropping the fuller dialogue.
- "Between Home and School" (Ravi) — condensed from `sources/english/ncert-class2-english-mridang-ch06.pdf`'s walk-to-school story (paddy fields, mango grove, jumping canals, climbing trees, reaching school on time).

**Original, honestly not NCERT-sourced (2 new — general early-reader themes per the task's fallback instruction: kindness, animals):**
- "Maya's little brother" — a sibling-kindness story (falls down, Maya comforts him).
- "Coco the duckling" — a lost-and-found-mother farm-animal story.

All 6 follow the existing 3 stories' shape: 3-5 short sentences, 2-3 comprehension questions, answers accepting reasonable phrasing (e.g. "any one" for open multi-part answers).

## Dead-flag audit

`L.mini` traced: rung 3 sets `mini:1` (truthy) → block D renders. Verified empirically: `ws_english_mirayah(2,'Test Book')` × 20 always included "D. Read &amp; answer" with a story body and matching question count. No off-by-one bug — `mini:0` (rungs 1-2) is falsy, `mini:1`/`mini:2` (rungs 3-9) are truthy, and the code just checks truthiness, so the flag behaves correctly for every value it's ever set to on this ladder.

## Verification

- `node --check`: pass.
- `ws_english_mirayah(2,'Test Book')` × 20: no `undefined`/`NaN`/`[object Object]`; block D present all 20 times, story text + answer count matched question count each time.
- `MINISTORY.length` confirmed 9 (was 3).
