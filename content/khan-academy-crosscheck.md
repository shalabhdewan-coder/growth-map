# Cross-check: Khan Academy real NCERT course structure vs. the existing rung-map

**Purpose:** the whole 12-rung Sarah / 10-rung Mirayah maths+English+science ladder was built this session from NCERT PDFs (many of them front-matter-only truncations — see rung 1-12 audit notes, which repeatedly flag "16-page file, TOC visible, chapter bodies not captured"), Singapore MOE syllabus, and SOF/Math-Kangaroo/AMC8 samples. Khan Academy's `khanacademy.org` hosts full, free, browsable NCERT-aligned course structures (real unit + skill lists, not just a TOC page) that were **not** used during the original build. This note cross-validates the existing rung-map against that second, independent, real source. Per the task brief: **verification pass, not a rebuild** — only fix what's clearly wrong.

Browsed live via Playwright MCP on 2026-07-20 (khanacademy.org is JS-rendered, plain fetch returns an empty shell).

## What Khan Academy actually shows, class by class

### NCERT Math Class 2 — `khanacademy.org/math/in-in-class-2nd-math-cbse`
4 units · 28 skills: **Numbers from 1 to 100**, **Addition and subtraction without regrouping**, **Addition and subtraction with regrouping**, **Geometry and measurement**. No multiplication unit at all.

### NCERT Math Class 3 — `khanacademy.org/math/in-in-class-3rd-math-cbse`
7 units · 59 skills: **Numbers from 1 to 1000**, **Addition and subtraction without regrouping**, **Addition and subtraction with regrouping**, **Multiplication**, **Division**, **Smart charts**, **Geometry and measurement**. No fractions unit.

### NCERT Math Class 4 — `khanacademy.org/math/in-in-class-4th-math-cbse`
6 units · 37 skills: **Addition and subtraction**, **Multiplication** (multiply by area model → distributive property → multiply by 1-digit numbers → multi-digit multiplication, up to roughly 4-digit×1-digit / light 2-digit×2-digit), **Division** (relate to multiplication → place value/area models → divide by 1-digit → multi-digit no-remainder long division), **Smart charts**, **Halves and quarters** (fractions-of-shapes, equal parts, "that's not fair" — purely visual/identification, no numeric a/b computation), **Geometry and measurement** (length/volume/mass/time estimation, perimeter, area of rectangles).

### NCERT Math Class 5 — `khanacademy.org/math/in-in-class-5th-math-cbse`
11 units · 66 skills: **Addition and subtraction**, **Multiplication**, **Division**, **Parts and wholes** (fractions: intro → recognise → numerator/denominator → word problems → **equivalent fractions** — stops there, no comparing/ordering/mixed-numbers unit visible), **Tenths and Hundredths** (decimals), **Factors and multiples**, **Smart charts**, **How big? How heavy?** (measurement estimation), **Shapes and angles**, **Area and its boundary**, **Identify patterns**.

### Science
`khanacademy.org/science/in-science-ncert` course-summary nav lists **NCERT Science starting at Class 6** (Class 6 → Class 10, then separate Physics/Chemistry/Biology 11-12). There is no Khan Academy NCERT Science/EVS course for Classes 1-5 at all — confirms (independently of this project's own PDF-sourcing difficulty) that Science as a distinct, unit-structured NCERT subject genuinely does not exist below Class 6; EVS at the lower grades is the real curriculum's answer, and it isn't unit-broken-out on Khan Academy either. This matches, rather than contradicts, this project's own finding that "NCERT Class 4 EVS confirmed too narrative/integrated for direct citation" and that Sarah/Mirayah's science ladders were correctly built from MOE Science syllabus + SOF samples instead of NCERT.

### English
Only one combined `khanacademy.org/humanities/in-english-grammar` course exists — not broken out per class the way Math is. Can't cross-check Sarah/Mirayah's English rung-map with the same unit-by-unit precision used for maths below. Not pursued further (matches task brief: only chase this "if a clean per-class breakdown exists" — it doesn't).

## Cross-check against Sarah's current placement (`LEVEL.sarah.maths=5` → rung 6 "Multiplication mastery")

`content/sarah-placement-assessment.md` placed Sarah at maths rung 6 from 14 real school PDFs (Pathways School Gurgaon PYP3, Nov 2025-March 2026): routine graded work paces rung 4-6 (Feb fraction sample = comparing 5/6 vs 3/4, ordering 4 fractions, improper-from-mixed — already **above** what Khan's Class 5 "Parts and wholes" unit covers, which tops out at equivalent fractions with no comparing/ordering/mixed-number skill listed); a recurring "critical thinking" track (3 independent dated samples, Nov→March) is genuinely rung 10-12/AMC8-entry caliber.

Rung 6's actual knobs (`growth-map.html` line 183): `{n:'Multiplication mastery', longmul:1, frac:1, s:3, bar:'frac', patt:'tri', chal:3}`. Checked what `longmul` and `frac` actually generate:
- `longmul` (`m_warm`): 2-digit×2-digit multiplication, factors in the R(12,29)×R(11,19) range — genuine long multiplication, a full step past Class 4's area-model/distributive-property introduction to multi-digit×1-digit work, and in the same territory as what Class 5's "Multiplication" unit would be expected to extend to (not deep-dived skill-by-skill, but Class 4→5 multiplication progression on Khan runs single-digit-multiplier → multi-digit-multiplier, matching `longmul`'s jump).
- `frac` (`m_words`/`m_bar`): the same fraction-of-a-quantity + comparison content calibrated for rung 4 (see `content/maths-sarah-rung4.md`), carried forward as continued practice rather than re-escalated — reasonable, since rung 6's headline skill is multiplication, not fractions.

**Verdict: rung 6 placement holds up.** Real Class 5 fraction content is, if anything, *behind* what Sarah's actual schoolwork (and this ladder's rung 4/6 fraction content) already demands — confirms the blend approach was necessary, not that the ladder over-shot. No placement change made.

## One gap found and closed: rung 6 never got its own audit note

Every other Sarah-maths rung (1-5, 7-12) has both a `content/maths-sarah-rungN.md` audit file and a dedicated `feat(maths/sarah): rung N ...` calibration commit. **Rung 6 has neither** — it was only ever touched incidentally by `fb42749` ("fix(maths): m_warm longmul dead when frac also set on same rung... hit rung 6 specifically") and by the original placement commit `cfe46cb`. It never went through the standard per-rung dead-flag/narrow-reach audit the other 11 rungs received.

Traced `longmul` and `frac` through all five generators (`m_warm`, `m_words` blocks 1-3, `m_bar`, `m_mistake`) for rung 6's exact knob combination — no masking or narrow-reach bug found (see `content/maths-sarah-rung6.md`, written as this task's fix, following the same audit format as every other rung). Verified: `node --check` passes; `ws_maths_sarah(5)` called 6 times directly in Node produces zero `undefined`/`NaN`/`[object Object]` occurrences.

## Rung-map table accuracy (Sarah rungs 1-9, cross-checked against real Class 2-5 unit lists)

| Rung | Blended target | Khan Academy reality check |
|---|---|---|
| 1-2 (Cl.3-equiv) | SG P3/CBSE Cl.3 | Matches — Class 3 has no fractions, no `frac` flag set on rungs 1-2. Confirmed. |
| 3 (Cl.4-equiv) | SG P4/CBSE Cl.4 | Matches — Class 4's unit order is mult→div→data→**fractions last**; rung 3 correctly still has no `frac` flag, rung 4 is the first to set it. Confirmed by the existing rung-3 audit ("no code change needed") and now independently by Khan's Class 4 unit ordering. |
| 4 "Fractions begin" | SG P4 fractions/Beast Academy 4A | Real NCERT Class 4 ("Halves and quarters") is purely visual (shape-halving, no numeric computation) — **easier** than this rung's actual content (fraction-of-a-quantity with real numerator/denominator arithmetic). Intentional: the plan explicitly blends in Beast Academy/SG for this rung rather than using NCERT alone. Not a mismatch. |
| 5 "Decimals & money" | SG P4-P5/CBSE Cl.5/Saxon 6/5 | **Closes a previously-flagged gap, doesn't change anything.** `content/maths-sarah-rung5.md` explicitly flagged "no chapter titled 'Decimals' or 'Money'... zero grep hits" in the (16-page-truncated) Class 5 source PDF, and treated the Cl.5 half of the blended target as unconfirmed. Khan Academy's real Class 5 unit list has **Unit 5 "Tenths and Hundredths"** — decimals genuinely is a Class 5 NCERT strand; the PDF truncation, not the curriculum, was the gap. Rung 5's placement and content were already correctly calibrated (from MOE + general knowledge); this just confirms the CBSE Cl.5 citation was directionally right. |
| 6 "Multiplication mastery" | SG P5/Beast Academy 4C | See above — holds up. |
| 7-9 | SG P5-P6+/Saxon 7·6-8·7/Beast Academy 5B-5D | Not deep-dived unit-by-unit on Khan (ratio/percentage/pre-algebra aren't separate NCERT primary units — they first appear as named strands from Class 6 onward, consistent with these rungs' blended targets already pointing past pure-NCERT-primary into Saxon/Beast Academy territory). No contradiction found. |

## One real content gap flagged, not fixed (out of scope for a verification pass)

Khan's real NCERT Class 5 has a full dedicated unit, **"Factors and multiples"** (LCM/HCF/prime-factorisation territory) — and Beast Academy's 4C-5A guides are built heavily around the same topic (factor trees, GCF/LCM). Grepped `growth-map.html` for `LCM`/`HCF`/`factor`/`multiple`: **zero generator anywhere in Sarah's 12-rung maths ladder covers this topic.** This is a genuine, evidence-backed content gap — but adding it properly means a new `m_*` generator function plus rewiring 1-2 rungs' knobs, which is exactly the kind of task this project's own process (source note → knob update → new generator → verify → commit, one rung/topic at a time) is built for, not something to improvise inside a verification pass. Flagging here for a future dedicated task rather than fixing now, per the "don't make sweeping changes" instruction for this pass.

## Overall confidence

**High.** A second, independent, real-content source (Khan Academy's actual NCERT unit/skill lists, not just TOC pages) confirms the existing rung-map's grade-equivalence claims for rungs 1-6 either exactly or in the "intentionally harder than pure-NCERT" direction the plan's blend-not-backbone decision already called for. No placement changes were needed. One documentation gap (rung 6's missing audit note) was found and closed. One real content gap (factors/multiples/LCM/HCF) was found and flagged for a future task, not fixed now.
