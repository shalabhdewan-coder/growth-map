# Sarah — English — Rung 4 "Opinion & why"

**Knobs:** `{n:'Opinion & why', vt:'mid', pt:2, gt:'hard', gc:4, write:'opinion'}` — `eLevelTag` = "P4 · opinion + reasoning".

See `content/english-sarah-rung1.md` for the shared architecture note and source-readability check.

## What rung 4 tests (per its own knobs)

- `vt:'mid'` — same as rung 3 (16 words, extended under rung 3's task).
- `pt:2` — same tier-2 passage pool as rungs 2-3 (5 entries, extended under rung 2's task). Every tier-2 passage in the bank ends its question set with an explicit "(your opinion)" question requiring two reasons — this was already true of the pre-existing 2 passages and was matched in all 3 new ones added under rung 2, so rung 4 inherits well-formed opinion prompts automatically; no passage-level change was needed for rung 4 specifically.
- `gt:'hard', gc:4` → **GRAMMAR.hard** (8 pairs before this task) — this is the first rung in the batch to switch off the easy grammar tier. Correctly calibrated: rung 4's harder grammar-fix sentences (subjunctive `if i was you`, `whom` vs `who`, "reason is because") are noticeably tougher than the easy-tier subject-verb-agreement errors rungs 1-3 use.
- `write:'opinion'` → `WRITE_TASKS.opinion`: "Pick one choice a character made... state your opinion and give TWO reasons" — directly matches the rung's name and is the clearest single-knob-to-content link of any rung in this batch.

## What was added

**`GRAMMAR.hard`: 8 → 12 pairs.** New 4, calibrated to the same difficulty band as the existing 8 (subject-verb agreement with relative clauses, pronoun case after prepositions, double-negative, collective-noun agreement): `one of the student who always help`→`helps`, `between you and i`→`between you and me`, `he dont hardly ever complain`→double-negative fix, `the news are`→`the news is` (uncountable noun agreement, a classic Indian-English learner trap). Hand-authored, same provenance discipline as GRAMMAR.easy (not NCERT-sourced — these readers don't contain drill material; calibrated to be plausibly P4/early-P5 grammar-instruction scope, which is a step up from the P3 easy-tier errors, matching the plan's "Opinion & why" being pitched slightly above rungs 1-3).

VOCAB.mid and PASSAGES tier 2 were already extended under rungs 3 and 2's tasks respectively (shared tiers, no further change needed for rung 4).

## Dead-flag audit

Same result as the other three rungs: `vt`, `pt`, `gt`, `gc`, `write` all read correctly by `ws_english_sarah`. No dead flags found specific to rung 4.

**Batch-wide dead-flag summary (rungs 1-4):** none of the knobs these four rungs actually set (`vt`, `pt`, `gt`, `gc`, `write`) are dead — all five are read and materially affect the generated worksheet. The one dead flag found in `ws_english_sarah` during this audit (`L.tech`, set on rungs 9-11 but never read) does not touch rungs 1-4 and is recorded in `english-sarah-rung1.md` for whoever picks up the rhetoric/debate rungs later.

## Verification

- `node --check`: pass.
- `ws_english_sarah(3,'Test Book')` called directly: no `undefined`/`NaN`/`[object Object]`; block D grammar-fix sentences correctly draw from the (now 12-entry) hard tier and are visibly harder than rungs 1-3's easy-tier sentences; block E resolves to the `opinion` writing task.
- Ran all four rungs (0-3) through the same Node harness in one pass: confirmed all four are structurally sound and mutually distinguishable — rung 0/1 share easy vocab+grammar but differ in passage tier (1 vs 2) and passage-set size/complexity; rung 2/3 share mid vocab and tier-2 passages but differ in grammar tier (easy vs hard) and writing task (describe vs opinion). No rung is a silent duplicate of another.
