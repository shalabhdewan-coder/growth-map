# Sarah — English — Rung 9 "Rhetoric & technique"

**Knobs:** `{n:'Rhetoric & technique', vt:'harder', pt:3, gt:'hard', gc:5, write:'rhetoric', tech:1}` — `eLevelTag` = "Rhetoric & technique".

**No curriculum PDF applies to this rung.** Persuasive rhetoric technique (rhetorical questions, rule of three, repetition as a deliberate craft device, hyperbole, alliteration) is not primary-school NCERT/MOE/SOF curriculum content — none of the sourced material in `sources/` reaches this topic at all; NCERT English readers up to Class 6 do not teach rhetorical device vocabulary as a named skill. This rung, and rungs 10-12 after it, are deliberately advanced stretch content for an 8-year-old reading above grade level, authored from general knowledge of rhetoric/persuasive-writing pedagogy (the same kind of "rhetorical devices" vocabulary taught in UK secondary-school English / early debate-club material), not fabricated as sourced from any NCERT/MOE/SOF document. This is the exact gap the curriculum-upgrade plan called out in advance (`docs/superpowers/plans/2026-07-14-curriculum-upgrade.md`, Phase 3: "Sarah rungs 9-12 (Rhetoric → Interview reasoning): no curriculum PDF applies").

## What this task fixed: the `L.tech` dead flag

`L.tech` has been set on rungs 9-12 (`{n:'Rhetoric & technique',...tech:1}`, `{n:'Debate — both sides',...tech:1}`, `{n:'Personal voice',...tech:1}`, `{n:'Interview reasoning',...tech:1}` — re-verified directly against `LEVELS.sarah.english[8..11]` in this task: all four carry `tech:1`, not just 9-11) since these rungs were first written, but `ws_english_sarah(li,book)` never read `L.tech` anywhere in its body — confirmed a dead flag by two independent prior agents (recorded in `content/english-sarah-rung1.md`'s original dead-flag audit and re-confirmed in every rung-5-through-8 audit since; those audits, and the task brief handed to this session, said "rungs 9-11," which undercounted rung 12 — corrected here). Fixed this task:

- Added a `RHETORIC` bank (5 rhetorical-device entries: rhetorical question, rule of three, repetition, hyperbole, alliteration — name + kid-friendly explanation + example) right after `WRITE_TASKS` in `growth-map.html`.
- Added a real `L.tech`-gated block to `ws_english_sarah`, lettered **E. Spot the technique** (inserted between the existing D-grammar block and the writing-challenge block, which becomes F when this block fires — rungs 9-12 — and stays E on rungs 1-8, which don't set `tech`; dynamic via a `wLetter` variable, so lettering stays sequential/consistent rather than skipping a letter on non-tech rungs).
  - Picks 2 random devices from `RHETORIC`.
  - Shows each device's name/explanation/example (items 1-2).
  - Item 3 asks the child to find one of the two techniques in the Block C passage above and quote+name it — or, if neither appears in that particular passage draw, explain in their own words how the writer achieves a similar effect. (Worded this way deliberately: the tier-3 passage pool is narrative prose, not rhetoric-tagged text, so a strict "it must be in there" instruction would sometimes fail; this keeps the task honestly answerable every time.)
  - Item 4 asks the child to write their own original sentence using one of the two devices — direct rehearsal for `WRITE_TASKS.rhetoric`'s own instruction ("use at least one rhetorical question and one rule of three").
- This block only appears when `L.tech` is truthy — verified rungs 1-8 (no `tech` flag) render without it, and rungs 9-12 all render with it (see Verification below and the batch-wide sweep in `english-sarah-rung12.md`).

## What rung 9 tests (per its own knobs)

- `vt:'harder'` — first rung to pull from `VOCAB.harder`. Extended this task: 8 → 16 words (see rung 10's file for the shared bank-size note; the extension is one shared edit covering rungs 9-12).
- `pt:3` — same tier-3 passage pool as rungs 5-8 (8 entries, unchanged this task — see "PASSAGES tier-3 pool" decision below).
- `gt:'hard', gc:5` — same `GRAMMAR.hard` pool as rungs 6-8 (20 entries, unchanged, still not saturated at a 5-slice draw).
- `tech:1` — now wired to the new Block E, described above.
- `write:'rhetoric'` → `WRITE_TASKS.rhetoric`: "Write a short persuasive piece about a change you want at home or school. Use at least one rhetorical question and one 'rule of three'." Already present in the bank (pre-existing, not added this task) and now directly reinforced by the new Block E rather than standing alone.

## PASSAGES tier-3 pool — decision: no new passages needed for this rung

Checked whether tier 3's 8 existing passages (all narrative short stories, added under rungs 5-8's task, each already carrying a "technique" comprehension question) need rhetoric-specific replacements for rungs 9-12. Decision: **reuse the existing pool, no new passages authored.**

Reasoning: `WRITE_TASKS` for rungs 9-12 (`rhetoric`, `debate`, `personal`, `interview`) ask the child to *produce* their own persuasive/reflective/argumentative writing, not to analyze a supplied argumentative text — none of these four rungs' writing tasks reference "the passage" at all. The new `L.tech` block (Block E) is the one place rung 9-11 worksheets touch technique-in-a-text, and it's deliberately written to work with *any* narrative passage (it asks the child to hunt for a device or explain the writer's effect in their own words, not to find a device that's guaranteed present) — so it doesn't require passages purpose-built with tagged rhetorical devices. Building a second, argumentative-essay-style tier-3 pool just for 4 rungs that don't actually read a passage closely would be scope beyond what the knobs call for.

## Verification

- `node --check` on the extracted `<script>` block: pass.
- `ws_english_sarah(8,'Test Book')` (rung 9, `li=8`): no `undefined`/`NaN`/`[object Object]`. Block E present, 2 distinct devices shown, item 3/4 render correctly, `ans` array has a matching `E techniques:` entry. Block F (writing) correctly resolves to the `rhetoric` template.
- Confirmed `VOCAB.harder.length === 16` post-edit; Block A draws correctly from the extended pool.
- Full 4-rung + regression sweep recorded once in `content/english-sarah-rung12.md` (closing task for the whole 12-rung ladder).
