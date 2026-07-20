# Sarah — English — Rung 7 "Structure & argument"

**Knobs:** `{n:'Structure & argument', vt:'hard', pt:3, gt:'hard', gc:5, write:'argument'}` — `eLevelTag` = "P5–P6 · structured argument".

See `content/english-sarah-rung1.md` for shared architecture; `content/english-sarah-rung5.md` and `-rung6.md` for the shared PASSAGES/VOCAB.hard/GRAMMAR.hard extensions this rung inherits (no rung-7-specific bank change needed — same tiers as rung 6).

## What rung 7 tests (per its own knobs)

Identical tier selection to rung 6 (`vt:'hard'`, `pt:3`, `gt:'hard'`, `gc:5`) — the only difference between rungs 6 and 7 is `write`. This is expected: per the architecture, English rungs are not knob-driven procedural difficulty like maths, they're bank-tier selectors, and consecutive rungs legitimately reuse the same content pool while the *writing task* (the one part of the worksheet that's genuinely rung-specific prose, not a bank pick) carries the differentiation.

- `write:'argument'` → `WRITE_TASKS.argument`: "Answer this in a structured way: 'Is the main character... brave or just lucky?' Claim → 2 reasons with evidence → conclusion." This is a meaningfully different task shape from rung 6's `persuade` (which argues FOR/AGAINST a reading choice with a counter-reason) — rung 7 requires a claim-evidence-conclusion structure with **explicit named parts**, one level more formal than persuasive writing. That's the actual differentiation the plan calls for at this rung, and it lives in `WRITE_TASKS.argument`'s wording, not in a knob.

## Bank changes this task

None specific to rung 7 — `VOCAB.hard` and `GRAMMAR.hard` were already extended under rung 6's task (shared tiers), and `PASSAGES` tier 3 under rung 5's. Re-verified both extensions hold up correctly when rung 7 is exercised (see Verification below).

## WRITE_TASKS bank — single-template audit (per task instruction to check if this needs to become a bank)

Checked whether `WRITE_TASKS.multi/persuade/argument/essay` (single fixed template string per type) is a real content gap. Conclusion: **left as-is, no bank needed**, for these reasons:
1. Each template already takes `book` as a parameter and is woven directly into a book-specific instruction ("Persuade a friend to read... '{book}'") — since the girl's current book rotates independently of her English rung, the same rung produces a visibly different, book-specific prompt on almost every worksheet in practice, not an identical one.
2. Unlike `VOCAB`/`GRAMMAR`/`PASSAGES` (which are drawn from repeatedly within *one* worksheet, so a thin pool causes visible repetition inside a single session), `WRITE_TASKS` fires exactly once per worksheet — there's no per-worksheet repetition to hide, only cross-session repetition of the *instructional framing*, which is arguably fine/expected for a rung that's meant to drill one specific writing skill (e.g. "always practise claim→evidence→conclusion" is the point of the argument rung, not something that should vary).
3. Turning this into a bank would add random-template-selection machinery for a case where the single template is doing its job (rung name ↔ task shape is a clean 1:1 mapping today) — judged as over-engineering relative to what was actually flagged as thin (VOCAB.hard/GRAMMAR.hard/PASSAGES tier 3, all now extended).

## Dead-flag audit

Same five knobs read correctly. `L.tech` confirmed not set on rung 7.

## Verification

- `node --check`: pass.
- `ws_english_sarah(6,'Test Book')`: no `undefined`/`NaN`/`[object Object]`. Block E writing task correctly resolves to the `argument` template (claim/evidence/conclusion wording), distinct from rung 6's `persuade` wording despite identical vt/pt/gt/gc tiers.
