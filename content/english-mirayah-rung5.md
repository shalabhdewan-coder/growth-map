# Mirayah — English — Rung 5 "Retell it"

**Knob:** `{n:'Retell it', wtier:'b', mini:1, write:'retell'}` — `LEVELS.mirayah.english[4]`.

Same architecture as rungs 1-4. Rung 5 shares `wtier:'b'` with rung 4 (`SIGHT.b`, extended in the rung 4 task) and `mini:1` with rungs 3-4 (`MINISTORY`, extended in the rung 3 task). The only new element at this rung is `write:'retell'`, which pulls `MWRITE.retell` — "Tell the story again in your own words — write 2 sentences," a direct pairing with block D's story, which is the pedagogical point of this rung's name ("Retell it").

## What rung 5 tests

No bank is uniquely gated to rung 5 alone — it's the convergence point of the `SIGHT.b` and `MINISTORY` extensions done for rungs 3-4, plus the pre-existing `MWRITE.retell` entry (already present, unchanged). This rung's "content gap" was therefore inherited entirely from the shared-bank thinness fixed upstream, not a rung-5-specific bank.

## Design note: does the retell prompt actually connect to the story shown?

Checked whether block D's randomly-picked story and block E's `MWRITE.retell` prompt reference the same passage. They do implicitly — `MWRITE.retell`'s text ("Tell the story again...") is generic and doesn't hardcode a title, so it correctly applies to whichever of the 9 `MINISTORY` entries `P(MINISTORY)` happens to draw for that worksheet instance. No mismatch risk: the prompt is written to work with any story in the pool, which is why extending `MINISTORY` (rung 3's task) directly benefits this rung's usability without any code change needed here.

## Dead-flag audit

`write:'retell'` traced to `MWRITE.retell` — key exists, was already in the bank before this task (`retell:'Tell the story again in your own words — write 2 sentences.'`), confirmed reachable via `MWRITE[L.write]||MWRITE.word` fallback logic (fallback never triggers here since the key exists). No dead flag.

## Full-ladder mini/rhyme gate summary (rungs 1-5, consolidated finding)

| Rung | `mini` | `rhyme` | Block C (rhyme) | Block D (ministory) |
|---|---|---|---|---|
| 1 Sight words | 0 | — | absent | absent |
| 2 Rhyming words | 0 | 1 | present | absent |
| 3 First sentences | 1 | — | absent | present |
| 4 Short comprehension | 1 | — | absent | present |
| 5 Retell it | 1 | — | absent | present |

Confirmed empirically (20 draws/rung, 100 draws total) — matches this table exactly in every draw. **No off-by-one or dead-flag bug found on `mini`/`rhyme` for rungs 1-5.** (Separately, `qs:1` on rungs 9-10 is a genuine dead flag — never read by `ws_english_mirayah` — but those rungs are outside this task's scope; left unfixed and flagged for a future pass.)

## Verification

- `node --check`: pass.
- `ws_english_mirayah(4,'Test Book')` × 20: no `undefined`/`NaN`/`[object Object]`; block D present all 20 times; write prompt correctly resolves to `MWRITE.retell` text every time.
- Full rung 0-4 sweep (100 draws) cross-checked against the table above: zero mismatches.
