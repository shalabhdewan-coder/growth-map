# Sarah — English — Rung 6 "Persuasive writing"

**Knobs:** `{n:'Persuasive writing', vt:'hard', pt:3, gt:'hard', gc:5, write:'persuade'}` — `eLevelTag` = "P5 · persuasive writing".

See `content/english-sarah-rung1.md` for shared architecture, `content/english-sarah-rung5.md` for the PASSAGES tier-3 extension (shared with this rung).

## What rung 6 tests (per its own knobs)

- `vt:'hard'` — this is the first rung on the ladder to pull from **VOCAB.hard** (rungs 6, 7 and 8 all use it — 3 rungs total; rung 5 still used `mid`). Before this task, `VOCAB.hard` had 8 words.
- `pt:3` — tier-3 passage pool, extended under rung 5's task (2→8, shared).
- `gt:'hard', gc:5` — GRAMMAR.hard, extended in this task (see below). `gc` steps up from 4 (rungs 4-5) to 5 here, meaning rung 6 pulls a 5th sentence from the same pool — worth having enough entries that a 5-slice doesn't feel repetitive across worksheets.
- `write:'persuade'` → `WRITE_TASKS.persuade`: "Persuade a friend to read (or NOT read)... Make a claim, give two reasons, and answer one reason someone might disagree" — directly matches the rung name (persuasive writing with a counter-argument component, appropriately harder than rung 4's plain "opinion + two reasons").

## What was added

**`VOCAB.hard`: 8 → 16 words.** New 8, thematically paired with the new tier-3 passages so a worksheet's vocab and its passage can plausibly reinforce each other: `valiant`/`wary` (Hekko folk tale — courage/caution), `diligent` (One Thing at a Time — focused effort), `integrity` (The Tinkling Bells — honesty), `steadfast` (Together We Can — loyal teamwork), `tranquil` (The Old Stag — calm), `bewildered` (Leaving the City — disorientation), `empathetic` (general — understanding others' feelings, ties to the Old Stag/Dev's-grandfather-style stories). Hand-written definitions, same style discipline as the existing 8.

**`GRAMMAR.hard`: 12 → 20 pairs.** New 8, calibrated one notch above the 4 added in rung 4's task (which were P4/early-P5). This extension targets P5/P6 error patterns: modal-perfect confusion (`should of`→`should have`), dangling modifiers, `each`-as-singular agreement, reported-speech tense shift, `lay`/`lie` confusion, double comparatives, `its`/`it's` + `there`/`their` stacked in one sentence, and `neither...nor` agreement with the nearer subject. This tier now has 20 entries because it is genuinely the single most-hit bank on the ladder — rungs 4 through 11 (8 of the 12 English rungs) all draw from `GRAMMAR.hard`, more than double any other tier's rung-count, so it was extended further than `VOCAB.hard`/`GRAMMAR.easy` on purpose rather than matching them 1:1.

## Dead-flag audit

Same five knobs (`vt`,`pt`,`gt`,`gc`,`write`) all read correctly. `L.tech` confirmed not set on rung 6.

## Verification

- `node --check`: pass.
- `ws_english_sarah(5,'Test Book')`: no `undefined`/`NaN`/`[object Object]`. Block A vocab correctly draws from the now-16-entry `VOCAB.hard` (sample draw showed `resilient, triumphant, cunning, empathetic, deliberate` — a mix of old and new entries). Block D grammar correctly slices 5 entries from the now-20-entry `GRAMMAR.hard`.
