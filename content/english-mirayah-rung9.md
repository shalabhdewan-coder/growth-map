# Mirayah — English — Rung 9 "Read & answer"

**Knob:** `{n:'Read & answer', wtier:'c', mini:2, write:'para3', qs:1}` — `LEVELS.mirayah.english[8]`.

First rung carrying `qs:1`. A prior agent's rung-5 audit had flagged this flag as dead: it was set on rungs 9-10 but never read anywhere in `ws_english_mirayah`. This task's primary job was to fix that.

## The `qs:1` fix

**Decision on what `qs` should gate:** rungs 9 ("Read & answer") and 10 ("Bridge to big-girl") are the two hardest rungs on Mirayah's ladder, immediately before she moves onto Sarah's ladder. The rung names themselves point at comprehension depth ("Read & answer" is literally block D's own heading — `'D. Read &amp; answer'`), so `qs` gating an *extra, harder* comprehension question onto block D — rather than, say, cosmetically renaming a heading or gating an unrelated block — is the reading most consistent with both the rung names and the existing block lettering (A/B/C/D/E stay fixed; nothing new is inserted).

**Implementation:** every `MINISTORY` entry (all 12) now carries two new fields:
- `x` — one additional inference/reasoning question ("why do you think…", "what does this tell you…", "what lesson…") that cannot be answered by literal recall alone, unlike the existing `q` array's fact-retrieval questions.
- `xa` — its answer, written in the same "accept reasonable answer" style already used elsewhere in the bank for open-ended items (e.g. entry 7's existing `'jumps over canals / climbs trees / swings from branches (any one)'` pattern).

In `ws_english_mirayah`, block D now reads:
```js
if(L.mini){const ms=P(MINISTORY);let dq=ms.q.slice(),da=ms.a.slice();
  if(L.qs){dq=dq.concat([ms.x]);da=da.concat([ms.xa]);}
  blocks.push({h:'D. Read &amp; answer',passage:ms.t,items:dq,write:1});ans.push('D: '+da.join(' | '));}
```
When `L.qs` is falsy (every rung except 9-10), behaviour is byte-identical to before this task. When `L.qs` is truthy, exactly one extra question+answer is appended — never mutates the shared `MINISTORY` array itself (uses `.slice()`/`.concat()`).

**Verified functionally, not just traced:** built a deterministic test (mocked `Math.random()=>0` so `P()` always draws `MINISTORY[0]`, the "Sam's red ball" story) and compared rung 9 (`qs:1`) against a synthetic clone of the same knob with `qs:0`. Result — `qs:1` produces 3 D-block questions ending in `"Q) How do you think Sam felt when he finally found the ball?"` with matching answer `"happy / relieved — he had looked and looked"`; `qs:0` produces exactly the original 2 questions, nothing appended. Confirms the flag is now live, not cosmetic.

## Other knobs at this rung

`wtier:'c'` and `mini:2` are both carried over from rungs 7-8's bank extensions (`SIGHT.c` → 12, `MINISTORY` → 12) — no rung-9-specific bank work needed beyond the `x`/`xa` fields above. `write:'para3'` reuses the 3-template pool extended in rung 7's task.

## Verification

- `node --check`: pass.
- `ws_english_mirayah(8,'Test Book')` × 5+: no `undefined`/`NaN`/`[object Object]`; block D consistently 1 question longer than the same-story draw at rungs 7-8 (mini:2, qs unset).
