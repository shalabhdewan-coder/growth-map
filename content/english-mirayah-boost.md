# Mirayah — English — Ladder-Extension ("Independent Study") Content

**Problem being fixed:** same root cause as Sarah's ladder (see `content/english-sarah-boost.md`) —
`intensifyRung` only scales *numeric* knobs. Mirayah's `wtier` (`'a'/'b'/'c'`, controls `SIGHT` sentence
difficulty) is a string, so every synthetic rung past rung 10 ("Bridge to big-girl") kept drawing from
`SIGHT.c` forever, unchanged from rung 10 itself, just relabelled "Independent Study N".

## What was added

**`SIGHT.d`** — 6 new fill-in-the-blank items (`growth-map.html`, end of the `SIGHT` object), genuinely
harder than tier c:
- Longer sentences (10-15 words vs. tier c's 6-9).
- Less common connector/conjunction words: `until`, `unless`, `since`, `although`, `whenever`, `instead` —
  a step up from tier c's `because/but/while/though/so/when`.
- **4 answer options instead of 3** (one extra distractor per item) — genuinely more distractors, as asked.
- Themes stay concrete and age-6-appropriate on purpose (piano practice, rain, dinosaurs, a snowman,
  visiting grandmother, a broken swing) — this is a harder tier-c-plus-one step, not a jump to Sarah's
  register.

No curriculum PDF reaches this — checked `sources/english/` (NCERT Class 1-2 Mridang/Marigold, MOE
syllabus) has nothing past the connector words already used in tier c. Honestly original, same disclosure
standard as the rest of this bank (see e.g. `content/english-mirayah-rung7.md`'s tier-c extension note).

## The shift mechanism

Reuses the same `effTier()` helper written for Sarah's ladder (defined once, shared across both girls'
generator functions in `growth-map.html`):

```js
const WT_ORDER=['a','b','c','d'];
```
```js
function ws_english_mirayah(li,book){const L=LEVELS.mirayah.english[li];let blocks=[],ans=[];
 const effWtier=effTier(WT_ORDER,L.wtier,L.boost);
 ...
 const sf=shuf(SIGHT[effWtier]).slice(0,4),sa=[];
```
`effWtier` replaces `L.wtier` when indexing `SIGHT`. Same guarantee as Sarah's `effVt`/`effGt`: `L.boost` is
only set by `intensifyRung` on synthetic rungs past `BASE_LADDER_LEN.mirayah.english` (10) — undefined on
hand-authored rungs 1-10, so `effWtier===L.wtier` there, mathematically (not just empirically) a no-op.
Every 3 "Independent Study" levels shifts one tier; since `intensifyRung` clones rung 10 (`wtier:'c'`), the
first shift (boost≥3) reaches `SIGHT.d` in one step.

## Decision: `MINISTORY` does not need a "harder" pool

Per the task brief's own steer ("use judgment... don't over-build"): **not added.** Reasoning:
- `MINISTORY` was never tiered per rung to begin with — the same 12-entry pool already serves *every*
  `mini`-gated rung (3 through 10) regardless of difficulty level; the pool's own content was never the
  axis this ladder escalates on.
- The real difficulty drivers at Mirayah's hardest rungs are `wtier` (now fixed above) and `qs` (the
  extra-inference-question flag added by a prior agent, `content/english-mirayah-rung9.md`).
- `qs` and `mini` are both numeric knobs already scaled generically by `intensifyRung`
  (`ceil(1*1.15^tier)`, `ceil(2*1.15^tier)`) — they stay truthy at every boost level (confirmed below), so
  the extra-question mechanism keeps firing on every synthetic rung without any further code change.
- Building a second, harder-labelled `MINISTORY` pool for a handful of ladder-extension rungs, when the
  existing pool + `qs` mechanism already deliver the intended escalation, would be scope beyond what the
  rung's own knobs call for — consistent with how Sarah's rungs 9-12 deliberately reused the existing
  tier-3 `PASSAGES` pool rather than building a parallel one (`content/english-sarah-rung9.md`).

## Verification

- `node --check` on the extracted `<script>` block: pass.
- Ran `ws_english_mirayah(li,book)` via the same `vm`-sandboxed harness at boost 0, 3, 6, 9 (reached via
  repeated real `doneLevelUp('mirayah','english')` calls from rung 10):
  - **boost=0** (rung 10 itself): `L.wtier='c'`, `effWtier='c'` — identical pool. Block B sight items drawn
    from `SIGHT.c` only. Zero `undefined`/`NaN`/`[object Object]`.
  - **boost=3/6/9**: `effWtier='d'` confirmed at all three; Block B items observed included the new tier-d
    sentences ("Priya visits her grandmother ____ she gets the chance." with 4 options
    `when/whether/where/whenever`; "Maya kept practising the piano piece ____..." with
    `until/under/uncle/unite`; the "Instead of complaining..." item) — genuinely drawing from `SIGHT.d`,
    not `SIGHT.c`. Zero bad-content hits at any boost level.
- 40-draw sample at boost=3: confirmed `SIGHT.d` sentences appear in Block B.
- `SIGHT.d.length===6` — bank size as documented above.
- Rungs 1-10 (indices 0,3,6,9 sampled directly, `L.boost===undefined` throughout) all render with zero
  bad-content hits and unchanged tier selection — confirms boost=0 behavior is unaffected by this change.
