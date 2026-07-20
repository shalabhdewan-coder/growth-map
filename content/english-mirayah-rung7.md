# Mirayah — English — Rung 7 "Mini paragraph"

**Knob:** `{n:'Mini paragraph', wtier:'c', mini:2, write:'para3'}` — `LEVELS.mirayah.english[6]`.

First rung at `wtier:'c'` — the hardest sight-word tier (causal/subordinating connectors: because, so, but, when, while, though — vs. tier b's simpler connectors like "to/than/or"). Also first rung at `mini:2` (functionally still gates block D on/off exactly like `mini:1` — see the rung-9/10 file for why `mini`'s numeric value itself isn't given new meaning, only `qs` is). `write:'para3'` is new here and reused twice more (rungs 9-10), so it needed real variety, not a single template.

## What was added

**`SIGHT.c`: 6 → 12 entries.** Was thin at 6 versus tier a/b's 12 each, and now has to serve 4 rungs (7-10) instead of 1. New 6 sourced from real NCERT Class 2 Mridang chapters newly available in `sources/english/` (ch09 "My Name", ch10 "The Crow", ch11 "The Smart Monkey" — extracted via `pdftotext -layout`):
- "The fly asked the tree ____ the ant did not know his name." (because/before/beside) — from ch09's chain-of-asking structure.
- "The leaf began to fly ____ the wind blew hard." (when/win/wind) — ch09's climax line ("when the wind blew").
- "Farida and Anju felt ashamed ____ they saw the monkey clean up." (after/at/ant) — ch11 "The Smart Monkey".
- "The monkey peeled the banana ____ he ate it." (before/beside/because) — ch11, sequencing (peel happens before eating).
- "The wind blew ____ the leaf was still on the ground." (while/white/wide) — ch09.
- "The crow was still a crow ____ he wore peacock feathers." (though/thought/throw) — ch10 "The Crow" (poem adapted).

**`MWRITE.para3`: 1 template → array of 3** (same treatment as rung 6's `two`, needed more urgently here since `para3` is drawn by 3 separate rungs):
- `'Write THREE sentences about your favourite animal.'` (existing, kept)
- `b=>'Write THREE sentences about "'+b+'" — who is in it, what happens, and what you liked.'` (new, book-tied)
- `'Write THREE sentences about your family — who they are and what you like doing together.'` (new)

**`MINISTORY`: 9 → 12 entries.** Pool now has to serve 8 rungs total (rungs 3-10, split 4/4 between `mini:1`/`mini:2`) — the prior agent's extension to 9 (for 3 rungs) was correctly flagged as tight for 5 more. Added 3 new entries grounded in the same newly-available NCERT chapters, shortened to age-6 length matching the existing entries' style: "My Name" (fly forgets his name, asks around, remembers when the leaf flies in the wind), "The Smart Monkey" (littering vs. the monkey's dustbin lesson), "The Crow" (vanity/self-acceptance, adapted from the poem). Every MINISTORY entry (all 12, including the original 9) also got a new `x`/`xa` field pair — see `content/english-mirayah-rung9.md` for why (the `qs:1` fix).

## Dead-flag audit

No new flags introduced by this rung specifically; `wtier:'c'` and `write:'para3'` both traced and confirmed reachable.

## Verification

- `node --check`: pass.
- `ws_english_mirayah(6,'Test Book')` × 5+: no `undefined`/`NaN`/`[object Object]`; block B correctly draws from `SIGHT.c` (12 entries, verified distinct from tier a/b output); block D present (mini:2 truthy); no extra qs question (qs not set on this rung — confirmed D block item count matches the base MINISTORY entry's own `q.length`, no +1).
