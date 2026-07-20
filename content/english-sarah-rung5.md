# Sarah — English — Rung 5 "Multi-paragraph"

**Knobs:** `{n:'Multi-paragraph', vt:'mid', pt:3, gt:'hard', gc:4, write:'multi'}` — `eLevelTag` = "P5 · multi-paragraph writing".

See `content/english-sarah-rung1.md` for the shared architecture note (banks, not per-rung generator logic) and source-readability check.

## What rung 5 tests (per its own knobs)

- `vt:'mid'` — same mid-tier vocab pool as rungs 3-4 (16 words, unchanged this task).
- `pt:3` → **PASSAGES tier 3** — this is the first rung on the ladder to pull from tier 3. Before this task, tier 3 had only 2 entries (the pre-existing "clockmaker" and "bookshop" passages), and it turns out to be the single most-hit pool on the whole English ladder: rungs 5 through 12 (8 of the 12 rungs) all draw from it. With only 2 passages, every one of those 8 rungs would eventually show the same 2 stories on rotation. This was the real priority of this batch.
- `gt:'hard', gc:4` → **GRAMMAR.hard** — already extended once (rung 4's task, 8→12). This task checked whether 12 was still enough now that 8 rungs (4-11) hit this tier, and extended it further (12→20, see rung 6's file for the detail, since GRAMMAR.hard is a shared edit covering this whole batch).
- `write:'multi'` → `WRITE_TASKS.multi`: "Write TWO linked paragraphs... paragraph 1 what happens, paragraph 2 what you think and why" — matches the rung name directly, and structurally forces the two-paragraph shape the rung is named for.

## What was added (shared across rungs 5-8, documented once here; see rung 6 for the GRAMMAR.hard detail)

**`PASSAGES` tier 3: 2 → 8 passages.** Six new passages added, each ~3 paragraphs, harder vocabulary and sentence structure than tier 1/2, 6 questions each (2 fact + 1 vocab-in-context + 1 inference + 1 opinion + 1 "technique" question about a writer's choice — matching the shape of the 2 pre-existing tier-3 entries, which already had that 6-Q "technique" pattern rungs 1-4's simpler passages don't use). All six are original stories, thematically grounded in real NCERT Class 4 "Santoor" chapters not yet drawn on by this bank:
- "The Relay" — ch.1 "Together We Can" (teamwork poem).
- "The Blue Envelope" — ch.2 "The Tinkling Bells" (Chinna/honesty story — found-money temptation theme).
- "The Metronome" — ch.4 "One Thing at a Time" (focus poem).
- "The Boy Who Challenged the River" — ch.9 "Hekko" (Naga folk tale of challenge/courage).
- "The Old Stag's Friends" — ch.5 "The Old Stag" (kindness returned during illness).
- "Leaving the City" — loosely grounded in the countryside imagery of ch.10 "The Swing" (a short Robert Louis Stevenson poem, not a narrative — flagged honestly since there is no story in that chapter to draw a plot from, only the countryside image; the story itself is original).

Verified via `pdftotext -layout` on the actual chapter PDFs before writing — themes are real, story text is freshly written, not paraphrased from NCERT.

## Dead-flag audit

`vt`, `pt`, `gt`, `gc`, `write` all read correctly by `ws_english_sarah` (same five knobs audited in rungs 1-4, no new knob types introduced by rung 5). Confirmed `L.tech` is **not** set on rung 5 (it only appears on rungs 9-11, per the dead-flag finding recorded in `english-sarah-rung1.md`) — no new dead flags found.

## Verification

- `node --check` on the extracted `<script>` block: pass.
- Direct `ws_english_sarah(4,'Test Book')` call via Node harness: no `undefined`/`NaN`/`[object Object]`; passage now correctly draws from the 8-entry tier-3 pool (confirmed by sampling 30 draws — all 8 distinct tier-3 openers appeared, no fallback to tier 1/2).
