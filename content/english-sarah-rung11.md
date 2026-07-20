# Sarah — English — Rung 11 "Personal voice"

**Knobs:** `{n:'Personal voice', vt:'harder', pt:3, gt:'hard', gc:5, write:'personal', tech:1}` — `eLevelTag` = "Personal-statement voice".

**No curriculum PDF applies to this rung.** Personal-statement / reflective-writing craft (writing in your own authentic voice about a real experience, the way an admissions personal statement begins) is not primary-school NCERT/MOE/SOF curriculum content — authored from general knowledge of personal-statement writing pedagogy, not sourced from or fabricated as coming from any curriculum PDF. Same honest-authorship note as rungs 9-10 — see `english-sarah-rung9.md` and the plan's Phase 3 callout.

## What rung 11 tests (per its own knobs)

Same tier selection as rungs 9-10 (`vt:'harder'`, `pt:3`, `gt:'hard'`, `gc:5`, `tech:1`) — the differentiation is again entirely in `write`:

- `write:'personal'` → `WRITE_TASKS.personal`: "Reflective writing: describe a time you found something hard and kept going. What does that story show about you? (This is how a personal statement begins.)" A genuinely different register from rungs 9-10 — first-person reflective narrative about the writer's own life, rather than persuading a reader (rung 9) or arguing a position (rung 10). This is the clearest voice-shift on the whole 12-rung ladder, appropriately placed second-to-last.
- `tech:1` — same shared Block E fires here too. This is a slightly odd fit on paper (rhetorical-device-spotting doesn't map as directly onto personal-statement writing as it does onto rhetoric/debate), but it's still legitimate: the "Spot the technique" block operates on the Block C reading passage, not on the child's own writing task, so it stays a consistent skill-drill across all three `tech:1` rungs regardless of what each rung's own writing prompt asks for. Confirmed it renders with correct, non-broken content at this rung (see Verification).

## Bank changes this task

None specific to rung 11 — `VOCAB.harder` (8→16) was already extended under rung 9's task as a shared edit across rungs 9-12; `GRAMMAR.hard` and `PASSAGES` tier 3 unchanged. Re-verified all three hold up at this rung.

## Dead-flag audit

Same six knobs read correctly. Correction versus this task's original brief: `L.tech` is **not** unique to rungs 9-11 — rung 12 (`Interview reasoning`, `LEVELS.sarah.english[11]`) also carries `tech:1` (re-confirmed by directly reading the array: all four of indices 8-11 set the flag). See `english-sarah-rung12.md` for that rung's own detail and the corrected batch-wide statement.

## Verification

- `node --check`: pass.
- `ws_english_sarah(10,'Test Book')` (rung 11, `li=10`): no `undefined`/`NaN`/`[object Object]`. Block E present and correctly formed, Block F resolves to the `personal` template, visibly distinct from rungs 9/10's writing prompts.
- Full 4-rung + regression sweep recorded once in `content/english-sarah-rung12.md`.
