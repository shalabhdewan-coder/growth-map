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

## SUPERSEDED — `MINISTORY` decision reversed (2026-07-21)

The section below was the original call made when this doc was written, and its Verification section
(further below) was validated against that state. **Both are now stale.** A later fix, commit `a629086`
("fix(english/mirayah): rung 10 ..."), changed rung 10's *own* config from `wtier:'c'` to `wtier:'d'` — a
correct fix for the rung9→10 transition, but it meant `intensifyRung` now clones a base rung that is
*already* at `WT_ORDER`'s ceiling tier (`WT_ORDER=['a','b','c','d']`). Every subsequent boost level clamps
at `'d'` via `effTier()` — the tier-shift mechanism this doc describes below has been completely inert
since that commit; there is no headroom left in `WT_ORDER`/`SIGHT` for it to shift into.

That left `mini`/`qs` as the only knobs `intensifyRung` still scales that could carry any further
escalation once `wtier` is maxed. The original reasoning below ("they stay truthy at every boost level ...
so the extra-question mechanism keeps firing") was true but was the bug, not a mitigation: staying truthy
forever is exactly why nothing escalated past rung 10 — a boolean check can't express "and now do it more."

**Decision reversed:** `ws_english_mirayah`'s Block D now reads `mini`/`qs` by *magnitude*, not just
truthiness, on synthetic rungs (`L.boost` set): the number of `MINISTORY` passages drawn scales with `mini`
(`Math.min(MINISTORY.length, L.mini-1)`, min 1), and how many of those passages carry their own harder
`x`/`xa` inference question scales with `qs` (`Math.min(storyCount, L.qs)`). This reuses the same 12-entry
`MINISTORY` pool — no new pool built, no new curriculum content invented — and is bounded by
`MINISTORY.length` so it can't run past the bank. Hand-authored rungs (`L.boost` unset) are byte-for-byte
unaffected: exactly one passage, plus the extra question only when `qs` is truthy, same as before this fix.

## Original doc (kept for history — see superseding note above)

### Decision: `MINISTORY` does not need a "harder" pool

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

### Verification (validated against the pre-`a629086` state — rung 10 was still `wtier:'c'` — see
superseding note above; no longer representative of current behavior)

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

## Current verification (2026-07-21, post-`a629086` + Block D magnitude fix)

Live-generated `ws_english_mirayah(li,'Test Book')` via the node harness in the audit skill, walking
synthetic rungs built the same way `doDoneLevelUp` builds them (`intensifyRung(lad[lad.length-1], tier)`
repeatedly from rung 10):

| rung | boost | mini | qs | passages in Block D | Block D question count |
|------|-------|------|----|--------------------------|-------------------------|
| 9 (base, "Bridge to big-girl") | — | 2 | 1 | 1 | 3 |
| 10 ("Independent Study 1") | 1 | 3 | 2 | 2 | 8 |
| 11 ("Independent Study 2") | 2 | 4 | 3 | 3 | 11 |
| 12 ("Independent Study 3") | 3 | 6 | 5 | 5 | 19 |
| 13 ("Independent Study 4") | 4 | 10 | 8 | 9 | 33 |

Confirmed: each rung's `ans` array stays correctly aligned to its `items` array (per-story `q`/`a` pairs
followed by that story's `x`/`xa` when included), passage text differs per rung (different `MINISTORY`
entries drawn), and `node --check` passes on the full extracted script. `effWtier` is confirmed stuck at
`'d'` for every rung 9 and above (the tier-shift mechanism above is inert, as described in the superseding
note) — the escalation from rung 10 onward now comes entirely from Block D's passage/question count
growth, not from `SIGHT` tier.
