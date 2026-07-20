# Sarah — English — Rung 10 "Debate — both sides"

**Knobs:** `{n:'Debate — both sides', vt:'harder', pt:3, gt:'hard', gc:5, write:'debate', tech:1}` — `eLevelTag` = "Debate — argue both sides".

**No curriculum PDF applies to this rung.** Formal debate structure (arguing both sides of a motion deliberately, as a discipline) is not primary-school NCERT/MOE/SOF curriculum content — it's authored from general knowledge of debate-primer pedagogy (the kind of "argue FOR, then argue AGAINST the same motion" exercise used in junior debate clubs), not sourced from or fabricated as coming from any curriculum PDF. Same honest-authorship note as rung 9 — see that file and the plan's Phase 3 callout for the full reasoning.

## What rung 10 tests (per its own knobs)

Same tier selection as rung 9 (`vt:'harder'`, `pt:3`, `gt:'hard'`, `gc:5`, `tech:1`) — consistent with the established pattern on this ladder (rungs 6-8 and 6-7 also shared identical tiers, differentiated only by `write`; see `english-sarah-rung7.md`). The only real difference between rung 9 and rung 10 is the writing task shape:

- `write:'debate'` → `WRITE_TASKS.debate`: "Take the topic 'Homework should be optional.' Write ONE paragraph FOR and ONE paragraph AGAINST — argue both sides fairly." This is a meaningfully different skill from rung 9's `rhetoric` task (persuade in one direction using technique) — rung 10 requires holding two opposing positions in the same piece of writing, which is the actual differentiator the plan's rung-map intends at this step (rhetoric technique → debating both sides is a real escalation in cognitive demand, not just a relabelled version of rung 9).
- `tech:1` — same new Block E ("Spot the technique") as rung 9, wired this task (see `english-sarah-rung9.md` for the implementation and for the correction that `tech:1` actually spans rungs 9-12, not just 9-11). At rung 10 it still fires correctly: the block isn't debate-specific, it's a shared rhetoric-device drill that happens to also serve the debate rung well (rule of three and rhetorical questions are exactly the tools a "both sides" paragraph needs).

## Bank changes this task

`VOCAB.harder` was extended from 8 → 16 words as one shared edit covering rungs 9-12 (documented in full in `english-sarah-rung9.md`; new words: `articulate`, `persuasive`, `coherent`, `concede`, `rebuttal`, `nuanced`, `credible`, `anecdote` — chosen for the skill these four rungs drill, argument/reasoning/voice, rather than tied to any one passage theme). No rung-10-specific bank change needed beyond that shared extension.

`GRAMMAR.hard` and `PASSAGES` tier 3 unchanged this task — both already sized correctly from rungs 5-8's work and re-verified to hold up at rung 10 (see Verification below).

## Dead-flag audit

Same six knobs (`vt`, `pt`, `gt`, `gc`, `write`, `tech`) all read correctly by `ws_english_sarah` after this task's fix. `L.tech` is now a live flag, confirmed firing on rung 10.

## Verification

- `node --check`: pass.
- `ws_english_sarah(9,'Test Book')` (rung 10, `li=9`): no `undefined`/`NaN`/`[object Object]`. Block E present (2 devices, correctly distinct draw from rung 9's on repeated sampling), Block F resolves to the `debate` template (FOR/AGAINST wording), visibly different instructional text from rung 9's `rhetoric` template despite identical `vt`/`pt`/`gt`/`gc`/`tech` knobs.
- Full 4-rung + regression sweep recorded once in `content/english-sarah-rung12.md`.
