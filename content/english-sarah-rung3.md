# Sarah — English — Rung 3 "Vocab in context"

**Knobs:** `{n:'Vocab in context', vt:'mid', pt:2, gt:'easy', gc:4, write:'describe'}` — `eLevelTag` = "P4 · vocab-in-context".

See `content/english-sarah-rung1.md` for the shared architecture note and source-readability check.

## What rung 3 tests (per its own knobs)

- `vt:'mid'` → **VOCAB.mid** (8 words before this task — the step up from rung 1/2's "easy" tier is the whole point of this rung's name). This is the first rung in the batch to use the mid tier.
- `pt:2` → same PASSAGES tier 2 pool as rung 2 (now 5 entries, extended under rung 2's task — see `english-sarah-rung2.md`). Rung 3's "vocab in context" label is reinforced by the writing task, not the passage tier.
- `gt:'easy', gc:4` — still GRAMMAR.easy (12 pairs, extended under rung 1's task).
- `write:'describe'` → `WRITE_TASKS.describe`, which explicitly instructs "Use two of this week's vocab words" — this is the mechanism that actually makes this rung about vocab-in-context rather than just "harder words in block A." Not touched by this task (WRITE_TASKS bank out of scope), noted here only to confirm the knob-to-content link exists and is real.

## What was added

**`VOCAB.mid`: 8 → 16 words.** New 8: reflective, lush, sheltering, invented, teamwork, superiority, majestic, countryside. All sourced from real NCERT Santoor Class 4 "New Words" lists — `reflective` (ch.3 "Be Smart, Be Safe"), `lush`/`sheltering` (ch.5), `invented` (ch.6 "Braille"), `teamwork` (ch.8 "The Lagori Champions"), `superiority` (ch.9), `majestic` (ch.12 "Maheshwar"), `countryside` (ch.10). This is a genuinely stronger sourcing chain than the easy tier's additions — these are the textbook's own listed vocabulary words for the Class 4 (P4) reading level this rung targets, not words merely inspired by chapter themes. Definitions are hand-written kid-friendly glosses (NCERT gives no learner-facing definitions in these chapters).

No PASSAGES or GRAMMAR changes needed for rung 3 specifically — both were already extended under rungs 1 and 2's tasks (shared tiers).

## Dead-flag audit

Same result: `vt`, `pt`, `gt`, `gc`, `write` all read correctly by `ws_english_sarah`. No dead flags found specific to rung 3.

## Verification

- `node --check`: pass.
- `ws_english_sarah(2,'Test Book')` called directly: no `undefined`/`NaN`/`[object Object]`; block A vocab words are drawn from the (now 16-entry) mid tier and are visibly harder than rung 1/2's easy-tier picks (e.g. "majestic", "countryside", "lush" vs "mighty", "cheerful"); block E writing prompt correctly resolves to the `describe` task instructing use of two vocab words.
- Confirmed rung 3's vocab bank output is disjoint in register from rung 1/2's (mid-tier abstract/descriptive words vs easy-tier simple adjectives) — the "vocab in context" step-up is real content, not a relabeling of rung 2.
