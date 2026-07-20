# Mirayah — English — Rung 2 "Rhyming words"

**Knob:** `{n:'Rhyming words', wtier:'a', mini:0, write:'word', rhyme:1}` — `LEVELS.mirayah.english[1]`.

Same bank-driven architecture as rung 1 (see `content/english-mirayah-rung1.md`). Rung 2 differs only by `rhyme:1`, which turns on block C (`if(L.rhyme){...push block C from RHYME...}`); `mini:0` still skips block D; `wtier:'a'` still pulls `SIGHT.a` for block B (shared bank, extended in the rung 1 task — 6 → 12).

## What rung 2 tests

`rhyme:1` → **RHYME**, the rhyme-matching bank. Before this task it held only 6 word/rhyme-set pairs — the single worst gap for this specific rung, since rung 2 is the *only* rung in the whole ladder that ever sets `rhyme:1` (every other rung's knob omits the flag, confirmed by grepping `LEVELS.mirayah.english` — only index 1 has `rhyme:1`). With 6 pairs and `shuf(RHYME).slice(0,3)` drawing 3 per worksheet, repeat-worksheet variety was thin.

## What was added

**`RHYME`: 6 → 12 pairs.** New 6 sourced directly from NCERT Class 1 Marigold Unit 1's Teacher's Pages pronunciation drills and sight-word pairs (`sources/english/ncert-class1-english-marigold.pdf`, page 14): the drill lists `blow/flow/glow`, `brick/kick/stick`, `huff/puff/stuff` (from the "Three Little Pigs" story's own vocabulary — huff/puff literally appear in the story text) and the sight-word pairs `bad/sad`, `big/dig`, `sun/bun`, `red/bed`, `hot/cot`. Converted to the bank's `[word, [correct-rhyme, distractor, distractor]]` shape: `['bad',['sad','dog','milk']]`, `['big',['dig','car','sun']]`, `['sun',['bun','tree','king']]`, `['red',['bed','fish','four']]`, `['hot',['cot','book','star']]`, `['puff',['stuff','light','tree']]`. Distractors are drawn from the existing bank's own word pool (dog, milk, car, tree, king, fish, four, book, star, light) so the false options are still real, previously-seen sight words rather than random noise — matches the existing 6 entries' internal-consistency style.

## Dead-flag audit

`L.rhyme` traced end-to-end: rung 2 is the sole knob setting it; `if(L.rhyme)` correctly gates block C on and every other rung off. Verified empirically — `ws_english_mirayah(1,'Test Book')` × 20 always included block C ("C. Which word rhymes? (circle it)"); rungs 0, 3, 4 (indices for "Sight words", "Short comprehension", "Retell it") never did in the same 20-draw sweep each. No bug found.

## Verification

- `node --check`: pass.
- `ws_english_mirayah(1,'Test Book')` × 20 in Node harness: no `undefined`/`NaN`/`[object Object]`; block C present every time; block D (ministory) absent every time, consistent with `mini:0`.
- `RHYME.length` confirmed 12 (was 6).
