# Mirayah — English — Rung 4 "Short comprehension"

**Knob:** `{n:'Short comprehension', wtier:'b', mini:1, write:'sent'}` — `LEVELS.mirayah.english[3]`.

Same architecture. Rung 4 is the first rung to move up to `wtier:'b'` (harder sight-word tier — connector words like "or", "than", "to" rather than rung 1-3's concrete nouns/verbs), keeps `mini:1` (block D on, shared `MINISTORY` pool with rung 3/5 — see `content/english-mirayah-rung3.md` for the bank extension), and reuses `write:'sent'`.

## What rung 4 tests

`wtier:'b'` → **SIGHT.b**. Before this task it held 6 entries (same thinness as `SIGHT.a` — see rung 1 file). Rung 4 is the first of three rungs sharing `SIGHT.b` (rung 4 "Short comprehension," rung 5 "Retell it," and — checking `LEVELS.mirayah.english` — no others until `wtier:'c'` starts at rung 7 "Mini paragraph").

## What was added

**`SIGHT.b`: 6 → 12 entries.** New 6, matching tier b's existing style (mix of function-word puzzles with phonetically-similar distractors, e.g. existing `['He gave the book ____ his friend.',['to','too','two']]`, and content-word items with unrelated-noun distractors, e.g. existing `['They are playing ____.',['outside','orange','under']]`). Sourced from NCERT Class 2 Mridang connector-word and prepositional-phrase sentences: "Take a bus ____ take a train" (or/oar/ore — from ch5's "Come Back Soon" recitation), "We go to school ____ foot" (on/one/ant — from ch6's "Between Home and School"), "She tied the ball ____ a piece of string" (with/wing/was — from ch2's "OUT! OUT!" story, also feeding the new rung-3 ministory), "I can climb ____ a cat" (like/lake/lick — from ch3's "It is Fun" poem), "We reach school ____ time every day" (on/ten/ant — from ch6), "The teacher put ____ a new chart" (up/cup/pup — from ch6's "ch" fill-in-the-blank exercise).

## Dead-flag audit

Rung 4 sets no new flags beyond what rungs 1-3 already exercise (`wtier`, `mini`, `write` all previously verified). No additional dead-flag risk introduced at this rung.

## Verification

- `node --check`: pass.
- `ws_english_mirayah(3,'Test Book')` × 20: no `undefined`/`NaN`/`[object Object]`; block B correctly draws from `SIGHT.b` (spot-checked distinct sentence text vs. rung 1-2 output which draws from `SIGHT.a`); block D present all 20 times (mini:1); block C absent all 20 times (no `rhyme` flag on this rung).
- `SIGHT.b.length` confirmed 12 (was 6).
