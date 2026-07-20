# Sarah — English — Rung 2 "Inference"

**Knobs:** `{n:'Inference', vt:'easy', pt:2, gt:'easy', gc:4, write:'para'}` — `eLevelTag` = "P3–P4 · inference".

See `content/english-sarah-rung1.md` for the shared architecture note (bank-tier-gated, not knob-driven-procedural) and the full source-readability check — not repeated here.

## What rung 2 tests (per its own knobs)

- `vt:'easy'` — same VOCAB tier as rung 1 (now 16 words, see rung1 note). Rung 2's differentiation from rung 1 is **not** in vocabulary difficulty (both use "easy"), it's entirely in `pt`.
- `pt:2` → **PASSAGES tier 2** (2 passages before this task — "the lighthouse keeper Tomas" and "Aanya's bicycle savings"). This is the single most heavily reused pool in the whole English generator: **3 of the 4 rungs in this batch** (rung 2, 3, 4) all draw from tier 2, and 2 more rungs beyond this batch (5, 6 — "Multi-paragraph", "Persuasive writing") draw from tier 3, so tier 2 was carrying a disproportionate share of the generator's total passage traffic on only 2 entries.
- `gt:'easy', gc:4` — same GRAMMAR tier as rung 1 (now 12 pairs, see rung1 note).
- `write:'para'` — same writing task as rung 1; the only manipulated variable between rung 1 and rung 2's knobs is `pt` (1→2).

This confirms the "Inference" label is earned structurally: tier-2 passages are multi-paragraph with a dedicated inference question and a vocab-in-figurative-language question (e.g. "the sky turned the colour of a bruise" / "her ribs showed like the bars of a little cage"), which tier-1's shorter single-paragraph stories don't attempt.

## What was added

**`PASSAGES` tier 2: 2 → 5 passages.** New 3, all original multi-paragraph stories (5 Qs each: 2 fact, 1 vocab-in-context, 1 inference, 1 opinion — matching the existing 2 entries' exact shape), thematically grounded in real NCERT Santoor Class 4 chapters but written fresh, not paraphrased:
- "Dev's grandfather" (deafness / homemade joke-cards) — theme grounded in Santoor ch.6 "Braille" (the real chapter is Louis Braille's biography; rather than retell someone's real biography as if it were original fiction, this passage borrows only the chapter's *theme* — an invention born out of a disability, made from ordinary materials — and tells a wholly different, original story about a boy and his deaf grandfather).
- "Minam and her grandfather" (mountain walk, patience) — theme and character names (Minam, grandfather-was-a-Sherpa) grounded in Santoor ch.11 "A Journey to the Magical Mountains"; the plot (stopping to notice flowers vs rushing to the peak) is original, not present in the source chapter.
- "Aarav and the fort" (Maheshwar-style old fort, worn steps) — theme grounded in Santoor ch.12 "Maheshwar" (queen-built fort, overhanging balcony); the specific scene (worn stone steps, Aarav's realisation) is original.

VOCAB.easy and GRAMMAR.easy were already extended as part of rung 1's task (shared banks — see `english-sarah-rung1.md`); no further changes needed for rung 2 since it uses the same tiers.

## Dead-flag audit

Same result as rung 1: every knob rung 2 sets (`vt`, `pt`, `gt`, `gc`, `write`) is read by `ws_english_sarah`. No dead flags introduced or found specific to rung 2.

## Verification

- `node --check`: pass (shared script, checked once — see rung1 note).
- `ws_english_sarah(1,'Test Book')` called directly via the Node harness: no `undefined`/`NaN`/`[object Object]`; block C passage correctly draws from the (now 5-entry) tier-2 pool, questions include the inference/vocab items tier-1 passages don't have.
- Called rung 1 eight times: passage rotates across tier-2 entries (previously only 2 to choose from, now 5) — confirmed non-repeating.
- Confirmed rung 1's output is materially different from rung 0's (longer passage, 5 Qs not 4, dedicated vocab-in-figurative-language question) — the rung 1→2 "Comprehension"→"Inference" step-up is real in the generated content, not just in the rung's display name.
