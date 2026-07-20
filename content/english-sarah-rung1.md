# Sarah — English — Rung 1 "Comprehension"

**Knobs:** `{n:'Comprehension', vt:'easy', pt:1, gt:'easy', gc:4, write:'para'}` — `eLevelTag` = "P3 · fact + vocab comprehension".

**Architecture note (applies to all four English rungs in this batch):** unlike Maths, Sarah's English generator (`ws_english_sarah`, `growth-map.html` ~line 774) is not knob-driven procedural generation — it is five fixed blocks (A vocab, B "about my book", C passage+Q&A, D grammar-fix, E writing task) that each pull from **shared content banks** (`VOCAB`, `GRAMMAR`, `PASSAGES`) gated by tier. A rung doesn't get bespoke logic; it gets a different tier selector. So "building" rung 1 means auditing/extending the banks at the tiers rung 1 actually pulls from (`VOCAB.easy`, `GRAMMAR.easy`, `PASSAGES` tier 1), not writing new generator code.

## Source check — what was actually readable

`sources/english/` now contains, beyond what the plan doc originally listed as available: full individual-chapter PDFs for **NCERT Class 2 "Mridang"** (13 chapters, ch01–ch13, each a genuine multi-page chapter with "New words" lists, "Let us Read/Speak/Learn" sections — verified with `pdftotext -layout`, not front-matter stubs) and **NCERT Class 4 "Santoor"** (12 chapters, ch01–ch12, same — verified readable, e.g. ch.3 "Be Smart, Be Safe" road-safety rules, ch.6 "Braille" biography, ch.8 "The Lagori Champions" dialogue, ch.11 "A Journey to the Magical Mountains", ch.12 "Maheshwar" fort/queen story). These are real, readable content, confirmed by direct `pdftotext` extraction (see `sources/english/ncert-class2-english-mridang-ch01.pdf` through `ch13.pdf` and `ncert-class4-english-santoor-ch01.pdf` through `ch12.pdf`).

Still-truncated / front-matter-only, confirmed by `pdfinfo`+`pdftotext` (do not cite content from these beyond ToC/prelims): `ncert-class3-english-santoor.pdf` (16pp, still only prelims/ISBN/publication-team pages — verified again this session, no chapter body), `ncert-class5-english-marigold.pdf` (6pp, prelims only), `ncert-class1-english-marigold.pdf` (15pp, not opened this session — flagged as likely-truncated per the original task note, unverified either way this pass since Sarah rungs 1-4 don't need Class 1 level). `sof-ieo-sample-class*.pdf` (2pp each) confirm skill-category framing (fact recall, inference, vocab-in-context are all real Olympiad English question types) but contain no usable passage text. `moe-primary-english-syllabus-2020.pdf` (54pp) not opened this pass — not needed since MOE strand-shape reasoning was already established for Maths and English differentiation here is bank-content-driven, not knob-driven.

## What rung 1 tests (per its own knobs)

- `vt:'easy'` → **VOCAB.easy** (8 adjectives before this task).
- `pt:1` → **PASSAGES tier 1** (1 passage before this task — the "Ravi found a paper boat" story). This was the single worst content gap in the whole English generator: rung 1 is the *only* rung that ever draws from tier 1, and with just one passage every rung-1 worksheet showed the identical passage every time.
- `gt:'easy', gc:4` → **GRAMMAR.easy** (6 sentence pairs before this task).
- `write:'para'` — unaffected by this task (WRITE_TASKS bank, not touched).

Per the plan's own differentiation goal (rung 1 "Comprehension" should be more literal/fact-based than rung 2 "Inference"), tier-1 passages are deliberately kept **short, single-paragraph, and fact-forward** (2 fact Qs + 1 light inference + 1 opinion) — this was already true of the one existing tier-1 passage and the new ones match that shape. Tier-2 passages (used by rungs 2-4) are longer, multi-paragraph, with a dedicated vocab-in-context question and heavier inference — that structural gap between tier 1 and tier 2 is what actually produces the "rung 1 more literal than rung 2" differentiation; it isn't encoded in a knob anywhere, it's encoded in which pool of passages exists at each tier.

## What was added (this task touches VOCAB.easy, GRAMMAR.easy, PASSAGES tier 1 — shared with rung 2 for GRAMMAR.easy, and with rungs 2-4 indirectly for VOCAB.mid/PASSAGES tier 2, documented in the other rung files)

**`VOCAB.easy`: 8 → 16 words.** New 8: quiet, joyful, peaceful, pleasant, mighty, colourful, careful, cheerful. Sourced from real NCERT "New words" lists — `quiet`/`joyful` (Mridang ch.4), `peaceful`/`pleasant`/`mighty` (Mridang ch.12), `colourful` (Mridang ch.10), `cheerful` (Santoor Cl.4 ch.7) — plus `careful`, which is not itself a listed vocabulary word but is grounded in Santoor Cl.4 ch.3 "Be Smart, Be Safe" (road-safety theme, matches one of the new tier-1 passages below). Definitions are hand-written kid-friendly glosses, same style as the existing 8 — not copied from NCERT (NCERT doesn't provide learner-facing one-line definitions in these chapters, only word lists).

**`GRAMMAR.easy`: 6 → 12 sentence-fix pairs.** New 6 cover subject-verb agreement (`he go`→`he goes`), plural nouns (`two brother`→`two brothers`), double-superlative (`most best`→`best`), existential agreement (`there is many`→`there are many`), present-progressive-for-habitual misuse (`i am liking`→`i like`), and be+past-tense stacking (`is went`→`went`). These are **hand-authored, not NCERT-sourced** — NCERT readers are literature/comprehension texts, they don't contain "fix the sentence" drill material. This matches the existing 6 entries' provenance exactly (they were also hand-authored to common Indian-English learner error patterns) — no fabricated sourcing claim is being made here.

**`PASSAGES` tier 1: 1 → 5 passages.** New 4, all original short stories (single paragraph, 4 Qs: 2 fact + 1 inference + 1 opinion), thematically grounded in real NCERT chapters but **not paraphrased or copied from the textbook text** — the stories themselves are freshly written:
- "Meera" (zebra-crossing) — theme grounded in Santoor Cl.4 ch.3 "Be Smart, Be Safe" road-safety rules.
- "the peacock and the sparrow" — theme grounded in Mridang Cl.2 ch.10's word list (colourful/feather/peacock).
- "Prakash learns Lagori" — theme grounded in Santoor Cl.4 ch.8 "The Lagori Champions" (a real NCERT dialogue about a boy learning the game Lagori from his cousin's friends; only the theme and character names Prakash/Ravi were borrowed, the story text is original).
- "Zara and Iqbal's lunch" — original, no specific chapter source (kindness/sharing theme, same register as the pre-existing "Ravi's paper boat" tier-1 passage).

## Dead-flag audit of `ws_english_sarah`

Checked every knob rung 1 sets against the function body: `vt` (read via `VOCAB[L.vt]`, block A) ✓, `pt` (read via `pickPassage(L.pt)`, block C) ✓, `gt`/`gc` (read via `GRAMMAR[L.gt]).slice(0,L.gc)`, block D) ✓, `write` (read via `WRITE_TASKS[L.write]`, block E) ✓. No dead knobs on rung 1 itself.

One dead flag found in the wider function, out of scope for rungs 1-4 but worth recording: **`L.tech` is set on rungs 9-11 (`{..., tech:1}`) but `ws_english_sarah` never reads `L.tech` anywhere** — it has no effect on the generated worksheet. (For contrast, `L.tech` *is* read correctly in `ws_music`/`ws_swim`, which are different arrays entirely — this is not a naming collision bug, just an unused knob on the English ladder.) Not fixed here since it's out of this task's rung range (1-4) and fixing it would mean designing new "technique focus" content for rungs 9-11, which belongs to the Phase 3 rhetoric/debate rungs, not this batch.

## Verification

- `node --check` on the extracted `<script>` block: pass, no syntax errors.
- Extracted the script (minus the trailing DOM-init/event-listener tail) into a standalone Node harness and called `ws_english_sarah(0,'Test Book')` directly (plus rungs 1-3, see other rung files): no `undefined`, `NaN`, or `[object Object]` anywhere in the output.
- Called rung 0 eight times in a row: passage rotates across all 5 tier-1 entries (confirmed non-repeating pool, was previously always identical).
- Confirmed rung 0 (tier-1 pool: short/literal) vs rung 1 (tier-2 pool: long/inference-heavy) produce visibly different passage length and question style — the literal-vs-inference differentiation the plan calls for is real, not cosmetic.
