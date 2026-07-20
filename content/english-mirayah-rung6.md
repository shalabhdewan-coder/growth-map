# Mirayah — English — Rung 6 "Two details"

**Knob:** `{n:'Two details', wtier:'b', mini:1, write:'two'}` — `LEVELS.mirayah.english[5]`.

Last rung sharing `wtier:'b'` (SIGHT.b, unchanged from the rung-4 extension — 12 entries, no further work needed) and `mini:1` (MINISTORY, extended as part of this 5-rung task — see the rung-9/10 file for the full pool rationale). The new element here is `write:'two'` — previously a single fixed string ("Write about your best friend. Give TWO things about them."), now extended (see below).

## What was added

**`MWRITE.two`: 1 template → array of 3.** Checked whether it needed the same treatment as Sarah's `WRITE_TASKS` (book-parametrized functions) — yes: rung 6 draws this same key alone (no other rung uses `write:'two'`), so a single fixed template meant *zero* variety across every worksheet at this rung, ever. Extended to a 3-item pool, `mwPick(key,book)` resolves via `P()` and calls the entry if it's a function:
- `'Write about your best friend. Give TWO things about them.'` (existing, kept)
- `b=>'Write TWO things about your favourite character in "'+b+'."'` (new — book-tied, mirrors Sarah's `WRITE_TASKS.describe` pattern)
- `'Write TWO sentences about your favourite animal — what it looks like and what it does.'` (new — generic but more concrete/scaffolded than the original)

## Dead-flag audit

No new flags at this rung. `write:'two'` now resolves through `mwPick()` instead of the old `MWRITE[L.write]||MWRITE.word` direct lookup — confirmed both string and function entries resolve correctly, no `undefined`.

## Verification

- `node --check`: pass.
- `ws_english_mirayah(5,'Test Book')` × 5+: block E always renders real text (never `undefined`); across repeated draws all 3 `MWRITE.two` variants observed, including the book-tied one correctly interpolating the book title.
- Full-ladder sweep result: see `content/english-mirayah-rung10.md` (closing summary).
