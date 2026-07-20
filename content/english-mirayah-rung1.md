# Mirayah — English — Rung 1 "Sight words"

**Knob:** `{n:'Sight words', wtier:'a', mini:0, write:'word'}` — `LEVELS.mirayah.english[0]`.

**Architecture note:** like Sarah's English generator, `ws_english_mirayah` (`growth-map.html` ~line 933) is bank-driven, not knob-driven procedural generation. Its five blocks are: A "about my book" (fixed), B sight-word fill-in-the-gap (`SIGHT[L.wtier]`), C rhyme (gated by `L.rhyme`), D mini-story comprehension (gated by `L.mini`), E write prompt (`MWRITE[L.write]`). Rung 1 pulls `SIGHT.a` only, skips C (no `rhyme` flag) and D (`mini:0`), and uses `MWRITE.word`.

## Source check

`sources/english/ncert-class1-english-marigold.pdf` (15pp) was flagged in the task brief as needing verification of whether it's real content or a truncated stub. Confirmed via `pdfinfo` + `pdftotext`: it is **real, full content** — Unit 1 "A Happy Child" (poem + new-words list: cry/day/red/sun), the "Three Little Pigs" story (new words: and/bad/big/but/not/one/pig/the/was), a farm/jungle/water animal-sounds page, and a Teacher's Pages section listing explicit sight-word pairs (bad/bed, big/dig, bun/sun, sad/red, cot/hot) and rhyme-drill word families (blow/flow/glow, brick/kick/stick, huff/puff/stuff). This is genuinely age-6 (Class 1) material, not a stub — corrects the earlier "unverified, likely-truncated" flag left by a prior agent's note in `content/english-sarah-rung1.md`.

`sources/english/ncert-class2-english-mridang-ch01.pdf` through `ch13.pdf` (confirmed real by two prior agents) supplied additional sight-word lists per chapter, e.g. ch1 "I | and | is | in | my", ch2 "This | a | how | also | they | after | and | of", ch3 "this | that | like | to | them | all | none", ch5 "take | is | the | or | two | may be".

## What rung 1 tests

`wtier:'a'` → **SIGHT.a**, the simplest tier. Before this task it had only 6 fill-in-the-gap sentences — thin, given SIGHT.a is also the sole tier for rung 2 ("Rhyming words," same `wtier:'a'`), so both of the first two rungs were drawing from the same 6-item pool.

## What was added

**`SIGHT.a`: 6 → 12 entries.** New 6, each a 3-word-choice fill-in-the-gap sentence, sourced from NCERT Class 1 Marigold Unit 1 vocabulary and sentence patterns: "I have a red ____" (bicycle/sad/three — bicycle borrowed from Mridang ch1's "I have a red bicycle" recitation, kept at tier-a difficulty), "The wolf was big and ____" (bad/soft/five — direct NCERT sentence, "Let's read" section), "The little pig was not ____" (big/milk/jump — direct NCERT sentence), "I ____ a dog in the garden" (saw/five/soap), "It is fun to ____ like a cat" (climb/red/two — from Mridang ch3's "It is fun" poem), "My friend and I ____ to school" (walk/six/blue — from Mridang ch6's "Between Home and School"). Distractors follow the existing tier-a pattern: one plausible word, two unrelated nouns/numbers/colours, so a child guessing by word-shape alone still has to actually read.

## Dead-flag audit (task-specified: check `mini:0` vs `mini:1`)

Traced `L.mini` from knob to render: `if(L.mini){...push block D...}`. Rung 1 sets `mini:0` (falsy in JS) → block D is correctly skipped. Verified empirically: `ws_english_mirayah(0,'Test Book')` called 20 times in a Node harness never produced a "D. Read & answer" block. **No off-by-one or wrong-comparison bug** — the `mini:0`/`mini:1` gate works exactly as the knob name implies. (A genuine, separate dead flag — `qs:1` set on rungs 9-10, indices 8-9 — is never read anywhere in `ws_english_mirayah`; that's outside this task's rung 1-5 scope and is left unfixed, flagged for a future pass.)

## Verification

- `node --check` on the extracted `<script>` block: pass.
- Standalone Node harness (script truncated before the DOM-init tail, at the `const GENS={...}` close) calling `ws_english_mirayah(0,'Test Book')` × 20: no `undefined`/`NaN`/`[object Object]`, block D never present.
- `SIGHT.a.length` confirmed 12 (was 6).
