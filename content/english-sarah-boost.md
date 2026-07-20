# Sarah — English — Ladder-Extension ("Independent Study") Content

**Problem being fixed:** `intensifyRung(base,tier)` (growth-map.html) clones the last hand-authored rung
(rung 12, "Interview reasoning") and generically scales every *numeric* knob by `Math.ceil(v*(1+0.15*tier))`
when a child presses "Done — level up" past the ladder's ceiling. For English this was a no-op on the knobs
that actually control content difficulty: `vt` (vocab tier) and `gt` (grammar tier) are **strings**
(`'easy'/'mid'/'hard'/'harder'` and `'easy'/'hard'`), so the numeric-only scaling loop skips them entirely.
Every synthetic rung past rung 12 kept drawing from `VOCAB.harder`/`GRAMMAR.hard` forever — same content,
just relabelled "Independent Study N". `pt` (passage tier, numeric 1-3) *did* scale automatically, but had
nowhere to escalate to since no tier-4 passages existed.

## What was added

1. **`VOCAB.expert`** — 10 new words (`growth-map.html`, end of the `VOCAB` object): `paradigm`, `juxtapose`,
   `ambiguous`, `discerning`, `pragmatic`, `empirical`, `cogent`, `dichotomy`, `ubiquitous`, `unequivocal`.
   Chosen for the skill the ladder-extension rungs keep drilling (sophisticated argument/rhetoric), not just
   "longer words" — deliberately going past `nuanced` (already in `.harder`). **No curriculum PDF reaches
   this register** — checked `sources/english/` (NCERT Class 1-5, MOE syllabus, SOF IEO samples up to
   Class 5) and none come close to SAT-adjacent vocabulary; this tier is honestly authored from general
   knowledge, same disclosure standard as rungs 9-12's own content notes (`content/english-sarah-rung9.md`
   through `-rung12.md`).
2. **4 new tier-4 `PASSAGES` entries** — longer (4 paragraphs vs tier 3's 3), denser sentence structure
   (subordinate clauses, semicolons), more abstract/adult themes, and each still carries the full
   fact/fact/vocab/inference/opinion/technique question shape (7 Qs vs tier 3's 6 — one extra inference
   question added). Also original, not sourced:
   - *"The Cartographer's Doubt"* — epistemic humility (certainty vs. truth), uses `paradigm`.
   - *"The Silent Vote"* — a school-council dichotomy resolved without a clean winner, uses `cogent`,
     `dichotomy`.
   - *"The Understudy"* — dramatic irony (the overlooked understudy saves the show), uses `pragmatic`,
     `discerning`.
   - *"Borrowed Time"* — a nonfiction/expository essay on procrastination (deliberately non-narrative —
     tests reading an argument's structure, not just a story), uses the "just start"/"just swim" comparison
     as its technique question.

## The shift mechanism

```js
const VT_ORDER=['easy','mid','hard','harder','expert'];
const GT_ORDER=['easy','hard'];
function effTier(order,cur,boost){const shift=Math.floor((boost||0)/3);return order[Math.min(order.length-1,order.indexOf(cur)+shift)];}
```

In `ws_english_sarah(li,book)`:
```js
const effVt=effTier(VT_ORDER,L.vt,L.boost), effGt=effTier(GT_ORDER,L.gt,L.boost);
```
`effVt` replaces `L.vt` when indexing `VOCAB`; `effGt` replaces `L.gt` when indexing `GRAMMAR`. `L.boost`
is **only ever set by `intensifyRung`**, on synthetic rungs past `BASE_LADDER_LEN.sarah.english` (12) — it
is `undefined` on every hand-authored rung 1-12. `Math.floor((undefined||0)/3) === 0`, so `effTier` returns
`order[order.indexOf(cur)+0] === cur` — mathematically a no-op at boost=0, not just "usually" a no-op.

Every 3 "Independent Study" levels (boost 3, 6, 9, …) shifts one tier further. Since `intensifyRung` clones
rung 12 (`vt:'harder'`), the first shift (boost≥3) takes it straight to `VOCAB.expert` (`harder`→`expert` is
only one step). `gt` stays `'hard'` — `GT_ORDER` deliberately has no tier past `'hard'` (see decision below).

`pt` (passage tier) needed **no code change** — it's already numeric and already scaled generically by
`intensifyRung`: `pt=3` at rung 12 becomes `ceil(3*1.15)=4` at the very first "Independent Study" press
(boost=1), and `pickPassage()` already had graceful tier-fallback logic (`filter(tier<=pt)`) that picks up
the new tier-4 pool the moment it exists — confirmed in verification below.

## Decision: no `GRAMMAR.expert` tier

Per the task brief's own steer ("use judgment on whether grammar needs a new tier"): **not added**.
`GRAMMAR.hard` already serves 8 of Sarah's 12 hand-authored rungs (the deepest-hit bank on the whole
ladder, 20 entries) and `gc` (grammar-question count, numeric) already scales generically via
`intensifyRung` — at boost=9 it reaches ~12 questions drawn from the same 20-entry pool, which is real
escalation in *volume/difficulty-of-a-full-worksheet* without needing new error-pattern content. Building a
whole new grammar-error tier for a handful of "Independent Study" rungs was judged out of scope relative to
what the rungs' own knobs call for.

## Verification

- `node --check` on the extracted `<script>` block: pass.
- Ran `ws_english_sarah(li,book)` via a `vm`-sandboxed harness (stubbed `document`/`window`) at boost 0, 3,
  6, 9 (reached by repeatedly calling the real `doneLevelUp('sarah','english')` from rung 12, exactly the
  path a real "Done — level up" click takes):
  - **boost=0** (rung 12 itself): `L.vt='harder'`, `effVt='harder'` — identical pool, vocab words drawn
    from `VOCAB.harder` only. Zero `undefined`/`NaN`/`[object Object]`.
  - **boost=3/6/9**: `effVt='expert'` confirmed at all three; Block A vocab words observed included
    `ambiguous`, `juxtapose`, `empirical`, `ubiquitous`, `pragmatic`, `cogent`, `discerning`,
    `unequivocal` — i.e. genuinely drawing from the new `VOCAB.expert` pool, not `harder`. Zero bad-content
    hits at any boost level.
  - `L.pt` confirmed scaling per `intensifyRung`'s compounding formula (3→9→52→555 across repeated
    "Done" presses — each press re-scales the *already-scaled* value, which is why it grows fast); at every
    tested boost, `pickPassage()` resolved without error and (via a 40-draw sample at boost=3) was
    confirmed to reach the new tier-4 pool.
- 40-draw sample at boost=3: `VOCAB.expert` words appeared in Block A's answer key, and tier-4 passages
  (identifiable by their 7-question shape vs. tier 3's 6) appeared in Block C — both confirmed present.
- `VOCAB.expert.length===10`, `PASSAGES.filter(p=>p.tier===4).length===4` — bank sizes as documented above.
- Rungs 1-12 (indices 0,3,8,11 sampled directly, `L.boost===undefined` throughout) all render with zero
  bad-content hits and unchanged tier selection — confirms boost=0 behavior is unaffected by this change.
