# Growth Map Curriculum Upgrade — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the currently generic, below-grade daily worksheet content in Growth Map with content calibrated against real curricula (Singapore MOE, CBSE/NCERT, Saxon, Beast Academy) blended with Olympiad/competition material woven into the core difficulty ladder — across Maths, English and Science, for both Sarah (8) and Mirayah (6).

**Architecture:** Single-file app stays single-file (`growth-map.html`), no backend, no build step — that constraint doesn't change. What changes is the *content* feeding the existing `LEVELS` ladders and worksheet-generator functions: (a) maths generators get new/recalibrated procedural knobs so numbers and topics match real grade-level scope at each rung, (b) English/Science content banks (`VOCAB`, `GRAMMAR`, `PASSAGES`, `SCI_SARAH`, `SCI_MIRAYAH`, etc.) get extended with hand-authored entries sourced from the curricula below, (c) Music ladder gets rebuilt against the real ABRSM/Trinity piano syllabus.

**Tech Stack:** Vanilla HTML/CSS/JS, zero dependencies. Verification via Playwright MCP (headless browser driving the actual file — this project has no test framework, so Playwright *is* the test framework here).

## Global Constraints

- No backend, no build tooling, no npm deps — stays a single portable HTML file exactly like today.
- State persistence stays URL-hash-only (existing design choice) — do not silently switch to localStorage.
- Never fabricate "this is what Saxon Math says" content without having actually read the sourced PDF for that rung. If a source isn't available yet, the task is blocked, not guessed.
- Never use pirated copies of copyrighted material (Saxon, Beast Academy, Wordly Wise, My Pals Are Here, Evan-Moor). Only NCERT/MOE/Olympiad-sample content gets fetched directly; everything else waits in `sources/` for the user to supply.
- Decision (user, 2026-07-14): **no single curriculum backbone** — pull the best-fit / hardest version of each topic across all four sources per rung, not a fixed pacing spine from one system.
- Decision (user, 2026-07-14): **competition/Olympiad material is woven into the core ladder**, not kept as a bonus-only section (the existing `★ Challenge` block stays, but rungs themselves also get harder).
- Decision (user, 2026-07-14): Music = **piano**, source ABRSM/Trinity syllabus. Riding = **kept generic**, no syllabus sourcing this round. Madhurim = out of scope this round (no PDF equivalent).

---

## Project layout (already created)

```
~/code/growth-map/
  growth-map.html          ← the app (git baseline committed: c8ec1c7)
  sources/
    maths/    english/    science/    music/     ← drop supplied PDFs here
  content/                                        ← extracted structured notes per subject+girl (see Task format below)
  docs/superpowers/plans/2026-07-14-curriculum-upgrade.md   ← this file
```

`content/*.md` files are the intermediate work product of each "extract" step below — a short human-readable note per rung: what topic/skill this rung should now cover, which source(s) it came from, and why that's the right difficulty. This is what makes the later "write the generator/bank entries" step reviewable before it's turned into code — you can read `content/maths-sarah.md` and sanity-check the plan before I touch `growth-map.html`.

---

## Rung → grade-equivalent map (blended, no single backbone)

This is the organizing skeleton every content task below maps against. It's public curriculum-equivalence knowledge (not textbook content) — safe to lock in now, independent of which PDFs arrive first.

### Maths — Sarah (12 rungs, current `LEVELS.sarah.maths`)

| Rung | Current label | Blended grade-equivalent target |
|---|---|---|
| 1 | Tables & 2-step | SG P3 / CBSE Cl.3 / Saxon 5·4 core |
| 2 | Bigger numbers | SG P3 / CBSE Cl.3 upper / Saxon 5·4 upper |
| 3 | Three-step problems | SG P4 / CBSE Cl.4 / Saxon 6·5 early |
| 4 | Fractions begin | SG P4 fractions / Beast Academy 4A |
| 5 | Decimals & money | SG P4-P5 / CBSE Cl.5 / Saxon 6·5 |
| 6 | Multiplication mastery | SG P5 / Beast Academy 4C |
| 7 | Ratio & proportion | SG P5-P6 / Saxon 7·6 |
| 8 | Percentages | SG P6 / CBSE Cl.6 / Beast Academy 5B |
| 9 | Pre-algebra | SG P6+ / Saxon 8·7 / Beast Academy 5D |
| 10 | Algebra & geometry | Lower-secondary / Math Kangaroo Benjamin |
| 11 | Competition heuristics | Math Kangaroo Benjamin upper / SASMO upper tiers |
| 12 | The quantitative spike | AMC 8 entry-level / proof-lite |

### Maths — Mirayah (10 rungs, current `LEVELS.mirayah.maths`)

| Rung | Current label | Blended grade-equivalent target |
|---|---|---|
| 1 | Counting play | SG P1 / CBSE Cl.1 / Saxon Math 1 |
| 2 | Bonds to 10 | SG P1 / Saxon Math 1 upper |
| 3 | Add & take to 20 | SG P1-P2 / CBSE Cl.1 upper |
| 4 | Teen numbers | SG P2 / Saxon Math 2 early |
| 5 | Within 100 | SG P2 / CBSE Cl.2 |
| 6 | Story sums | SG P2 / Beast Academy 2A |
| 7 | Regrouping | SG P2 upper / Saxon Math 2 upper |
| 8 | Groups of 2·5·10 | SG P2-P3 bridge / Beast Academy 2C |
| 9 | Two-step (little) | Bridge into Sarah rung 1 territory |
| 10 | Bridge to big-girl | Math Kangaroo Pre-Ecolier |

### English / Science — same treatment, built during Phase 3/4 tasks below (deferred until Maths pattern is proven in Phase 2, since Maths is the highest-priority fix per your original complaint).

---

## Verification protocol (used by every task — this replaces "unit tests" for this stack)

There is no test framework in this project; Playwright MCP driving the real file is the test framework.

- [ ] **Syntax gate (fast, run first):** extract the `<script>` block and pipe through `node --check`
```bash
cd ~/code/growth-map
node -e "const fs=require('fs');const h=fs.readFileSync('growth-map.html','utf8');const m=h.match(/<script>([\s\S]*)<\/script>/);fs.writeFileSync('/tmp/gm-check.js', m[1]);" 
node --check /tmp/gm-check.js
```
Expected: no output (pass).

- [ ] **Playwright structural check (per rung touched):** open the file, select the girl/subject, step to the specific rung, press "Make worksheet", and assert on the rendered sheet:
  - no `undefined`, `NaN`, or `[object Object]` anywhere in `.sheet` text
  - answer-key line count matches question block count
  - for maths: every `____` blank has a corresponding answer line
  
  Drive this via `mcp__plugin_playwright_playwright__browser_navigate` to `file:///Users/shalabhgupta/code/growth-map/growth-map.html`, then `browser_evaluate` to call `LEVEL['sarah']['maths']=<rung-1>; renderSub('sarah','maths'); makeWS('sarah','maths'); document.getElementById('wsprev-sarahmaths').innerText` and inspect the returned text.

- [ ] **Human spot-check:** for content-bank tasks (English/Science), a machine check can't judge "is this actually harder / age-appropriate" — flag 2-3 generated sheets per changed rung for the user to eyeball before moving to the next rung.

- [ ] **Commit:** one commit per rung (or per generator function) touched, message states which source(s) it was calibrated against, e.g. `feat(maths/sarah): rung 4 fractions — recalibrate to SG P4 + Beast Academy 4A scope`.

---

## Phase 0 — Infra (done)

- [x] Create `~/code/growth-map/` project home
- [x] Copy `growth-map-v9.html` in as `growth-map.html`, `git init`, baseline commit `c8ec1c7`
- [x] Create `sources/{maths,english,science,music}/`, `content/`, `docs/superpowers/plans/`

## Phase 1 — Sourcing (blocks all content tasks)

**Files:** none touched yet — this phase only populates `sources/`.

- [ ] **Step 1: Fetch free/legal sources.** Use WebFetch/WebSearch to pull NCERT Maths/English/EVS Class 1–6 PDFs (ncert.nic.in), Singapore MOE Maths/English/Science syllabus docs (moe.gov.sg), Math Kangaroo + AMC 8 past papers, SASMO/NSO/iOS/iOEL sample papers, ABRSM/Trinity piano syllabus docs. Save each into the matching `sources/<subject>/` folder with a clear filename (`sources/maths/ncert-class3-maths.pdf`, etc).
- [ ] **Step 2: User supplies commercial sources.** User drops Saxon Math, Singapore Primary Mathematics/My Pals Are Here English, Beast Academy, Wordly Wise, Evan-Moor into the matching `sources/` subfolders.
- [ ] **Step 3: Inventory check.** List `sources/**` and cross-reference against the rung-map tables above — flag any rung with zero matching source (that rung's task in Phase 2+ stays blocked until either a source arrives or we explicitly decide to hand-author it from Olympiad-only material).

## Phase 2 — Maths (priority: this was your specific complaint)

One task per rung per girl — 12 for Sarah, 10 for Mirayah, 22 total. Each follows the same shape:

### Task shape (repeated per rung — example shown for Sarah rung 4 "Fractions begin")

**Files:**
- Read: `sources/maths/*` pages matching SG P4 fractions + Beast Academy 4A (per rung-map)
- Create: `content/maths-sarah-rung4.md` (extraction notes)
- Modify: `growth-map.html` — `LEVELS.sarah.maths[3]` (knob object) and `m_words()`/`m_bar()` in the maths generator section (currently `growth-map.html` lines ~462-550, will shift as earlier rungs are edited — always re-grep `LEVELS.sarah.maths` before editing)

**Interfaces:**
- Consumes: existing generator plumbing — `m_warm(L)`, `m_words(L)`, `m_bar(L)`, `m_pattern(L)`, `m_mistake(L)`, `m_algebra(L)`, `m_geometry()` all take/return the same shapes as today; new topic types get a new `m_*(L)` function following that exact pattern (`{items, a}` or `{h, items, write, a}`).
- Produces: `LEVELS.sarah.maths[3]` knob object stays consumed by `ws_maths_sarah(li)` — do not change that function's call signature, only the knob values and which `m_*` functions it calls.

- [ ] **Step 1:** Read the relevant source pages, write `content/maths-sarah-rung4.md`: what skill(s) this rung tests, at what difficulty, citing exact source+page/paper.
- [ ] **Step 2:** Update the rung's knob object in `LEVELS.sarah.maths[3]` to match (e.g. tighter/wider number ranges, new topic flags).
- [ ] **Step 3:** If the source introduces a topic type with no existing `m_*` generator (e.g. LCM/HCF, composite-shape area, speed-distance-time), write the new generator function and wire it into `ws_maths_sarah()`.
- [ ] **Step 4:** Run the verification protocol (syntax gate → Playwright structural check → generate 3 sample sheets for human spot-check).
- [ ] **Step 5:** Commit.

**Sarah rungs to run through this shape:** 1 Tables&2-step · 2 Bigger numbers · 3 Three-step · 4 Fractions begin · 5 Decimals&money · 6 Multiplication mastery · 7 Ratio&proportion · 8 Percentages · 9 Pre-algebra · 10 Algebra&geometry · 11 Competition heuristics · 12 Quantitative spike

**Mirayah rungs to run through this shape:** 1 Counting play · 2 Bonds to 10 · 3 Add&take to 20 · 4 Teen numbers · 5 Within 100 · 6 Story sums · 7 Regrouping · 8 Groups of 2·5·10 · 9 Two-step (little) · 10 Bridge to big-girl

## Phase 3 — English (same per-rung shape as Phase 2, adapted for content banks)

**Files:** `content/english-{sarah,mirayah}-rungN.md`, then `growth-map.html` — extend `VOCAB`, `GRAMMAR`, `PASSAGES` (shared banks, tier-gated by rung's `vt`/`gt`/`pt` knobs) and Mirayah's `SIGHT`/`RHYME`/`MINISTORY`/`MWRITE`.

- [ ] Sarah rungs 1-8 (Comprehension → Persuasive writing): source from NCERT English + Wordly Wise + My Pals Are Here, extend `VOCAB.easy/mid/hard`, `GRAMMAR.easy/hard`, add new `PASSAGES` entries at tiers 1-3 sourced from real grade-appropriate texts (not invented), add English-Olympiad-style inference/technique questions to existing passages.
- [ ] Sarah rungs 9-12 (Rhetoric → Interview reasoning): **no curriculum PDF applies** — author from debate-primer / interview-prep style sources (flag as separate research task, not blocked on `sources/english/`).
- [ ] Mirayah rungs 1-10: source from NCERT Class 1-2 English + Wordly Wise Book 2 + My Pals Are Here 1-2, extend `SIGHT` tiers a/b/c, `RHYME`, `MINISTORY`, add iOEL-style "spot the detail" questions at upper rungs.
- [ ] Verification protocol per rung (same as Phase 2), human spot-check is mandatory here (content-bank quality can't be asserted mechanically beyond "no undefined/NaN").

## Phase 4 — Science (same shape again)

**Files:** `content/science-{sarah,mirayah}-rungN.md`, then `growth-map.html` — extend `SCI_SARAH`/`SCI_MIRAYAH` arrays (each rung is one array entry with `vocab`/`facts`/`sc`/`method`/`lab`).

- [ ] Source from NCERT EVS/Science Class 1-6 + Singapore MOE Science syllabus + NSO/iOS sample papers per the rung topics already named in the array (`Living & non-living`, `Plants`, `Animals & classification`, … through `Scientific method & design`).
- [ ] Olympiad-woven rungs (per your "weave into core" decision): upper rungs' `sc` (think-it-through) and `method` (like-a-scientist) entries should read closer to NSO/iOS question style, not just curriculum-recall.
- [ ] Verification protocol per rung.

## Phase 5 — Music (piano ladder rebuild)

**Files:** `content/music-piano.md`, then `growth-map.html` — replace `MUSIC` array (currently 12 generic rungs) with real ABRSM (or Trinity — pick one as primary, note the other as cross-reference) syllabus-derived rungs.

- [ ] Source ABRSM piano syllabus doc (current edition) from `sources/music/`.
- [ ] Map syllabus grades 1-8 + pre-grade "Initial"/prep stage onto the existing 12-rung shape (`tech`/`piece`/`sight`/`aural`/`mins` fields) — real scale requirements, real piece-list tier, real sight-reading/aural standard per grade, sourced from the syllabus doc, not invented.
- [ ] Verification protocol.
- [ ] Note: `MUSIC` is shared across both girls (`Object.assign(LEVELS.sarah, {..., music:MUSIC, ...})` and same for mirayah) — one rebuild serves both, they just sit at different rungs (`LEVEL.sarah.music=2`, `LEVEL.mirayah.music=0` today).

## Phase 6 — Final pass

- [ ] Re-run the full verification protocol across every rung of every touched subject (not just the ones edited last) — a change to a shared function (e.g. `m_words`) can silently affect rungs edited earlier in the plan.
- [ ] Update this plan's checkboxes to reflect actual completion state.
- [ ] Diff `growth-map.html` against the `c8ec1c7` baseline commit and read the whole diff top-to-bottom as a final human-review pass before calling this done.
- [ ] Copy the finished `growth-map.html` back to `~/Downloads/growth-map-v10.html` (or wherever the user actually uses it from) — the git repo is the source of truth going forward, Downloads was never versioned.

---

## Self-review notes

- **Spec coverage:** All 4 subjects named by the user (Maths explicitly, English/Science/Music by "same for all subjects") are covered. Riding and Madhurim explicitly excluded per user's own answers — not silently dropped, just out of scope this round.
- **Blend-not-backbone honored:** rung-map tables cite multiple sources per rung rather than one fixed pacing spine, per user's explicit choice.
- **Competition-woven-into-core honored:** Phase 2-4 explicitly instruct recalibrating core rungs with Olympiad-tier material, not just the existing `★ Challenge` block.
- **No fabrication:** every content task is gated on an actual source in `sources/`; Sarah's English rungs 9-12 are explicitly called out as the one area with no textbook source, so that content gets authored transparently rather than mislabeled as "from Wordly Wise" when it isn't.
- **Placeholder scan:** Phase 2's task shape is fully concrete (real file paths, real function names, real verification commands). Phases 3-5 intentionally do not contain literal question text yet — that's not a placeholder, it's because that content is gated on Phase 1 sourcing completing first; each phase gives the exact source+target mapping needed to write it for real once sourced.
