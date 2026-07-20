# Sarah — English — Rung 12 "Interview reasoning" (closing task — full 12-rung ladder sweep)

**Knobs:** `{n:'Interview reasoning', vt:'harder', pt:3, gt:'hard', gc:5, write:'interview', tech:1}` — `eLevelTag` = "Interview / seminar reasoning".

**No curriculum PDF applies to this rung.** Interview reasoning (defending a real point under questioning, in writing, the way an admissions interview works) is not primary-school NCERT/MOE/SOF curriculum content — authored from general knowledge of interview-reasoning / seminar-discussion pedagogy, not sourced from or fabricated as coming from any curriculum PDF. Same honest-authorship note as rungs 9-11 — see `english-sarah-rung9.md` and the plan's Phase 3 callout.

## Correction to the task brief: `L.tech` spans all four rungs 9-12, not just 9-11

The task handed to this session (and the two prior agents' findings it was based on) said `L.tech` is set on "rungs 9-11." Directly re-reading `LEVELS.sarah.english[8..11]` in `growth-map.html` shows this undercounts by one: **all four** of rungs 9, 10, 11, and 12 (`Rhetoric & technique`, `Debate — both sides`, `Personal voice`, `Interview reasoning`) carry `tech:1`. Only rung 12's `write:'interview'` differs from the earlier three in what makes it distinct — the `tech:1` flag itself is not what separates rung 12 from 9-11. This was caught during this task's own Node verification pass (comparing `L.tech` for `li=7..11` showed `true` at all of 8/9/10/11, not just 8/9/10) and corrected in `english-sarah-rung9.md`, `-rung10.md`, and `-rung11.md` before writing this file.

## What rung 12 tests (per its own knobs)

Same tier selection as rungs 9-11 (`vt:'harder'`, `pt:3`, `gt:'hard'`, `gc:5`, `tech:1`):

- `write:'interview'` → `WRITE_TASKS.interview`: "You have 5 minutes with an admissions tutor. In writing, answer: 'Tell me about a book that changed how you think, and why.' Make a real point and defend it." This is the ladder's final and most demanding writing register — not just producing an opinion (rung 4), a persuasive piece (rung 9), both sides of a debate (rung 10), or a reflective personal story (rung 11), but defending a single real point under implied questioning, closest to genuine oral-exam reasoning compressed into writing.
- `tech:1` — same shared Block E fires here, confirmed rendering correctly (see sweep below).

## Bank changes — final state, all shared across rungs 9-12

| Bank | Before this batch | After | Rungs that draw from it |
|---|---|---|---|
| `VOCAB.harder` | 8 | 16 (new: `articulate`, `persuasive`, `coherent`, `concede`, `rebuttal`, `nuanced`, `credible`, `anecdote`) | 9, 10, 11, 12 (all 4 `harder`-tier rungs) |
| `RHETORIC` (new bank) | did not exist | 5 devices (rhetorical question, rule of three, repetition, hyperbole, alliteration) | 9, 10, 11, 12 (via new `L.tech`-gated Block E) |
| `PASSAGES` tier 3 | 8 (already extended under rungs 5-8's task) | unchanged — reused, no new passages | 5-12 (8 rungs, unchanged span) |
| `GRAMMAR.hard` | 20 (already extended under rungs 4-6's tasks) | unchanged | 4-12 (9 rungs, unchanged span) |

**PASSAGES tier-3 decision (documented in full in `english-sarah-rung9.md`):** no new passages authored for rungs 9-12. `WRITE_TASKS` for these four rungs ask the child to produce their own rhetoric/debate/personal/interview writing, not analyze a supplied argumentative text — the existing 8-entry tier-3 narrative pool, already carrying "technique" comprehension questions, is sufficient. The new `L.tech` block is written to work with any narrative passage (it asks the child to find a device or explain the writer's effect in their own words, not to find a device guaranteed present), so it doesn't require purpose-built argumentative texts.

## `L.tech` fix — final summary

Added a `RHETORIC` bank (5 entries) and a new `L.tech`-gated **Block E "Spot the technique"** to `ws_english_sarah`, inserted between the D-grammar block and the writing-challenge block (which shifts to letter F on tech rungs, stays E on non-tech rungs, via a `wLetter` variable — sequential lettering preserved, no skipped letters). The block: shows 2 randomly-drawn rhetorical devices with kid-friendly explanations/examples, asks the child to find one in the Block C passage (or explain the writer's effect in their own words if neither literally appears), then write an original sentence using one device — direct rehearsal for `WRITE_TASKS.rhetoric`'s own instruction to "use at least one rhetorical question and one rule of three." Fires on rungs 9-12 only; confirmed absent (no block, no error) on rungs 1-8.

## Full 12-rung × 5-sample closing sweep

Ran `ws_english_sarah(li, book)` for every `li` 0-11 (all 12 rungs) × 5 different book titles (`Test Book`, `The Hobbit`, `Matilda`, `Anne of Green Gables`, `Charlotte's Web`) = **60 total generated worksheets**, via a Node harness built from the live `growth-map.html` (script block extracted, DOM-init tail stripped, `module.exports` appended — same method used by every prior rung's verification).

**Result: 0/60 bad-content hits.** No `undefined`, `NaN`, or `[object Object]` anywhere in any generated worksheet's JSON-serialized output.

Block-letter shape confirmed correct at every rung:
- Rungs 1-8: `A, B, C, D, E` (E = writing challenge; no tech block, as expected — none of these 8 rungs set `tech`).
- Rungs 9-12: `A, B, C, D, E, F` (E = new "Spot the technique" block, F = writing challenge) — confirmed at all four rungs, not just 9-11.

Bank sizes confirmed post-edit: `VOCAB.harder.length === 16`, `RHETORIC.length === 5`, `GRAMMAR.hard.length === 20` (unchanged from rungs 4-8's work), `PASSAGES.filter(p=>p.tier===3).length === 8` (unchanged from rung 5's work).

## Escalation read — does the ladder climb smoothly end to end?

Yes, on every axis the ladder is supposed to escalate on:

- **`vt` (vocab tier):** `easy` (rungs 1-2, 16 words) → `mid` (rungs 3-5, 16 words) → `hard` (rungs 6-8, 16 words) → `harder` (rungs 9-12, 16 words). Four clean tiers, each equally well-stocked after this batch's extensions (no thin tier left — `harder` was the last one sitting at only 8 before this task).
- **`pt` (passage tier):** `1` (rung 1) → `2` (rungs 2-4) → `3` (rungs 5-12). Genuine step up in passage length/complexity at each tier (tier 1 = single paragraph, 4 Qs; tier 2 = 3 paragraphs, 5 Qs; tier 3 = 3 longer paragraphs, 6 Qs including a "technique" question) — confirmed by re-reading the `PASSAGES` array directly, not assumed.
- **`gt`/`gc` (grammar tier/count):** `easy`×4 (rungs 1-3) → `hard`×4 (rung 4) → `hard`×5 (rungs 5-12) — both the error-pattern sophistication (`GRAMMAR.easy` = basic subject-verb/tense errors; `GRAMMAR.hard` = modal-perfect, dangling modifiers, reported speech, `neither...nor` agreement) and the per-worksheet draw count step up together, never regress.
- **`write` (writing-task complexity):** `para` (topic sentence + 2 details) → `describe` → `opinion` (claim + 2 reasons) → `multi` (2 linked paragraphs) → `persuade` (claim + 2 reasons + counter-reason) → `argument` (claim→evidence→conclusion, named parts) → `essay` (intro/body/conclusion) → `rhetoric` (persuasive piece with named techniques) → `debate` (both sides of a motion) → `personal` (first-person reflective) → `interview` (defend a real point under implied questioning). This is a real, checked progression, not just an assumption — each step was individually verified against its own `WRITE_TASKS` template text during rungs 5-12's tasks, and every template resolves to genuinely distinct instructional wording (confirmed again in this sweep).
- **`tech` (technique-analysis, new this task):** absent on rungs 1-8, present and correctly firing on rungs 9-12 — the one axis that was previously a complete no-op across the whole "advanced" quarter of the ladder is now live, closing the gap the two prior agents flagged.

**Overall:** Sarah's 12-rung English ladder is now internally consistent end to end — bank-tier progression (vocab/passage/grammar), draw-count progression (`gc`), and writing-task-complexity progression all climb together with no flat/regressing stretch, and the one genuinely dead mechanism (`L.tech`) is now a real, verified, working block rather than a declared-and-ignored flag. This closes Sarah's English ladder (Phase 3 of the curriculum-upgrade plan, Sarah half) — Mirayah's English ladder and Sarah/Mirayah Science remain separate, not-yet-done phases.

## Verification

- `node --check` on the full extracted `<script>` block, post all edits in this batch: pass, no syntax errors.
- 60/60 generated worksheets (12 rungs × 5 books) clean — see sweep above.
- Confirmed no regression to rungs 1-8 (already-completed rungs) from this task's shared-bank/shared-function edits (`VOCAB.harder`, `RHETORIC`, `ws_english_sarah`'s new Block E logic) — all 8 still produce correct `A,B,C,D,E` block shape with no tech block.
