# Mirayah — English — Rung 8 "Because & why"

**Knob:** `{n:'Because & why', wtier:'c', mini:2, write:'because'}` — `LEVELS.mirayah.english[7]`.

Shares `wtier:'c'` (SIGHT.c, extended as part of rung 7's task — 12 entries, no further work needed here) and `mini:2` (MINISTORY, also extended as part of rung 7's task). The rung's own name is a direct thematic echo of `SIGHT.c`'s content (causal connectors: because/so/but/when/while/though) and of `write:'because'`, which is new here.

## What was added

**`MWRITE.because`: 1 template → array of 3.** Same reasoning as rungs 6-7 — a single fixed cloze sentence gave zero variety across every worksheet at this rung:
- `'Finish this and add one more line: "My favourite day is ____ because ____."'` (existing, kept)
- `b=>'Finish this: "My favourite part of ‘'+b+'’ is ____ because ____." Then add one more line.'` (new — book-tied, and doubles as reading-comprehension reinforcement since it forces the child to reference the actual assigned book, not a generic prompt)
- `'Finish this and add one more line: "I like my best friend because ____."'` (new)

All three keep the rung's defining shape (a because-clause the child must complete and justify), matching the rung's name and the causal-reasoning focus already established by `SIGHT.c`.

## Dead-flag audit

No new flags at this rung.

## Verification

- `node --check`: pass.
- `ws_english_mirayah(7,'Test Book')` × 5+: no `undefined`/`NaN`/`[object Object]`; block E draws all 3 `MWRITE.because` variants across repeated calls, book-tied variant correctly interpolates title; block D present at base question count (mini:2, qs not set on this rung, no extra question — same confirmation pattern as rung 7).
