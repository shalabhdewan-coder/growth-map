# Mirayah — English — Rung 10 "Bridge to big-girl"

**Knob:** `{n:'Bridge to big-girl', wtier:'c', mini:2, write:'para3', qs:1}` — `LEVELS.mirayah.english[9]`.

Final rung of Mirayah's 10-rung English ladder. Identical knob shape to rung 9 (`wtier:'c'`, `mini:2`, `write:'para3'`, `qs:1`) — the pairing is intentional: rung 9 ("Read & answer") and rung 10 ("Bridge to big-girl") are the two hardest rungs, both now getting the `qs:1` extra inference question (fixed in this task, documented fully in `content/english-mirayah-rung9.md`), both drawing from the same fully-extended `SIGHT.c` (12) and `MINISTORY` (12) pools. No rung-10-specific bank work was needed — this rung's entire job was to consume everything rungs 6-9 built and hold at the ladder's ceiling before Mirayah moves onto Sarah's rung-1 territory.

## This task's closing sweep (Step 7 of the assignment)

Ran the full verification protocol across **all 10 rungs × 5 samples = 50 draws** (script: forced content extraction + `vm` sandbox with stubbed `document`/`window`, calling `ws_english_mirayah(li,'Test Book N')` directly, then regex-scanning the full JSON output for `undefined`/`NaN`/`[object Object]`).

**Result: 0 bad-content hits out of 50 draws.**

### Escalation table (confirms an honest, non-flat difficulty climb)

| Rung | Name | wtier | mini | write | qs | D-block Q count |
|---|---|---|---|---|---|---|
| 1 | Sight words | a | 0 | word | — | 0 (no block D) |
| 2 | Rhyming words | a | 0 | word | — | 0 (no block D) |
| 3 | First sentences | a | 1 | sent | — | 2-3 |
| 4 | Short comprehension | b | 1 | sent | — | 2-3 |
| 5 | Retell it | b | 1 | retell | — | 2-3 |
| 6 | Two details | b | 1 | two | — | 2-3 |
| 7 | Mini paragraph | c | 2 | para3 | — | 2-3 |
| 8 | Because & why | c | 2 | because | — | 2-3 |
| 9 | Read & answer | c | 2 | para3 | 1 | 3-4 (+1 vs rungs 3-8) |
| 10 | Bridge to big-girl | c | 2 | para3 | 1 | 3-4 (+1 vs rungs 3-8) |

`wtier` climbs a→a→a→b→b→b→b→c→c→c→c (sight-word difficulty: concrete nouns → connector words → causal connectors). `write` climbs word→word→sent→sent→retell→two→para3→because→para3→para3 (single word → 2 sentences → retell → 2-detail → 3-sentence paragraph → causal cloze → 3-sentence paragraph, now with the harder `qs` question attached). `qs` cleanly separates rungs 9-10 from 7-8 despite identical `mini:2` — this is the one place `mini`'s bare on/off value (1 vs 2) doesn't itself add difficulty; that job was correctly reassigned to `qs`, which is now a real functional gate (see rung 9 file for the deterministic before/after proof).

## Note on `mini`'s numeric value (1 vs 2)

Confirmed `mini` is read only as `if(L.mini)` — a boolean gate for block D's presence, same as `rhyme`. The value 2 (vs 1) carries no independent meaning in the code; this was true before this task and was left unchanged, since fixing `qs` — not re-purposing `mini` — is what the rung names and the task explicitly called for. `mini:2`'s rungs (7-10) are still meaningfully harder than `mini:1`'s rungs (3-6) via `wtier` and `write`, and rungs 9-10 are harder still via `qs`. No dead-flag risk remains: `wtier`, `mini`, `write`, `rhyme`, and `qs` are all now confirmed live and read.

## Verification

- `node --check`: pass.
- `ws_english_mirayah(9,'Test Book')` × 5+, plus full 50-draw sweep above: no `undefined`/`NaN`/`[object Object]` anywhere.
- `qs:1` fix verified both statistically (Dcount +1 on rungs 9-10 across every sample in the sweep) and deterministically (mocked-random single-story comparison in rung 9's file).
- Bank sizes at close of this task: `SIGHT.a`=12, `SIGHT.b`=12, `SIGHT.c`=6→12, `MINISTORY`=9→12 (all with new `x`/`xa` fields), `MWRITE.two`/`para3`/`because` each 1→3 templates (mix of static + book-tied via `mwPick()`).
