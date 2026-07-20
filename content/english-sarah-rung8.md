# Sarah — English — Rung 8 "Essay basics"

**Knobs:** `{n:'Essay basics', vt:'hard', pt:3, gt:'hard', gc:5, write:'essay'}` — `eLevelTag` = "P6 · essay structure".

See `content/english-sarah-rung1.md` for shared architecture; `-rung5.md`/`-rung6.md` for the PASSAGES tier-3 / VOCAB.hard / GRAMMAR.hard extensions this rung inherits.

## What rung 8 tests (per its own knobs)

Same tier selection as rungs 6-7 (`vt:'hard'`, `pt:3`, `gt:'hard'`, `gc:5`) — rung 8 is the last rung before the ladder switches to `vt:'harder'`/`tech:1` at rung 9 (out of scope for this batch, see dead-flag note below). Same reasoning as rung 7: the differentiation at this rung lives entirely in `write`.

- `write:'essay'` → `WRITE_TASKS.essay`: "Write a mini-essay (intro, one body paragraph, conclusion): 'What is the most important idea in ...'" — the most formally structured of the four writing tasks in this batch (multi → persuade → argument → essay is a genuine escalation: two linked paragraphs → persuasion with counter-reason → claim/evidence/conclusion → full intro/body/conclusion essay shape). This progression is real and was checked directly against the plan's rung-map intent, not assumed.

## Bank changes this task

None specific to rung 8 — inherits the same `VOCAB.hard`/`GRAMMAR.hard`/`PASSAGES` tier-3 extensions made under rungs 5-6's tasks. Re-verified below.

## Batch-wide summary (rungs 5-8, this task)

| Bank | Before | After | Rungs that draw from it |
|---|---|---|---|
| `VOCAB.hard` | 8 | 16 | 6, 7, 8 (3 rungs) |
| `GRAMMAR.hard` | 12 | 20 | 4, 5, 6, 7, 8, 9, 10, 11 (8 rungs — heaviest-hit bank on the whole ladder) |
| `PASSAGES` tier 3 | 2 | 8 | 5, 6, 7, 8, 9, 10, 11, 12 (8 rungs — tied heaviest) |
| `WRITE_TASKS.multi/persuade/argument/essay` | 1 template each | unchanged | audited in `-rung7.md`; single template per type judged sufficient, not a real gap |

## Dead-flag audit (batch-wide, rungs 5-8)

Checked every knob rungs 5-8 set (`vt`, `pt`, `gt`, `gc`, `write`, `n`) against `ws_english_sarah`'s body — all read correctly (block A reads `vt`, block C reads `pt` via `pickPassage`, block D reads `gt`/`gc`, block E reads `write`). **Confirmed `L.tech` is NOT set on any of rungs 5-8** (only rungs 9, 10, 11 set it, per the finding already recorded in `english-sarah-rung1.md`; `ws_english_sarah` still never reads `L.tech` anywhere, so it remains a dead flag scoped to rungs 9-11, unrelated to this batch). No other knob or field was found silently ignored.

## Verification

- `node --check` on the full extracted `<script>` block: pass, no syntax errors.
- Ran `ws_english_sarah(li, 'Test Book')` for `li` 4, 5, 6, 7 (rungs 5-8) via a Node harness built from the live file (script extracted up to the DOM-init tail, `module.exports` appended — same approach as prior rungs' verification): no `undefined`, `NaN`, or `[object Object]` in any of the four outputs.
- Confirmed bank sizes post-edit: `VOCAB.hard.length === 16`, `GRAMMAR.hard.length === 20`, `PASSAGES.filter(p=>p.tier===3).length === 8`.
- Sampled 30 draws of rung 7's passage block: all 8 distinct tier-3 passages appeared (2 original + 6 new), confirming the pool is actually being used, not silently falling back to a smaller tier.
- Confirmed real differentiation across rungs 5-8 despite three of them (6-8) sharing identical `vt`/`pt`/`gt`/`gc`: rung 5 uses `mid` vocab (harder rungs use `hard`), and rungs 6/7/8 are distinguished entirely by their `write` task text (persuade → argument → essay), each verified to resolve to visibly different instructional wording in the generated worksheet.
- Also spot-checked rungs 1-4 still produce valid output after this batch's edits (no regression to earlier rungs from the shared-bank extensions).
