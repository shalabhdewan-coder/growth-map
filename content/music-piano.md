# Music (Piano) — MUSIC ladder rebuild

**Source:** `sources/music/abrsm-piano-syllabus-2025-2026.pdf` — ABRSM *Qualification Specification: Practical Music, Piano 2025 & 2026*, © 2024 The Associated Board of the Royal Schools of Music. 63 pages, verified complete via full read (Introduction pp.3-10, Piano Practical Grades Syllabus pp.11-52, Assessment/marking pp.53-61, exam form p.62). Valid 1 Jan 2025 – 31 Dec 2026 (p.3) — current edition, not stale.

This ladder is shared between Sarah and Mirayah (`Object.assign(LEVELS.sarah/mirayah, {music:MUSIC})`), they just sit at different indices (`LEVEL.sarah.music`, `LEVEL.mirayah.music`).

## What the placeholder got wrong

The old 12-entry `MUSIC` array invented a "Grade 1–8 + performance/diploma" pathway that didn't match ABRSM's actual grade count or content, and specifically:

- **No "Initial Grade" existed in the old ladder** — ABRSM's real entry-level qualification (introduced in recent syllabus cycles) sits below Grade 1: *ABRSM Entry Level Award in Practical Music (Initial Grade) (Entry 3)* (p.5). The old rung 1 ("Notes & rhythm basics") was a vague pre-Grade-1 warm-up that doesn't correspond to anything ABRSM actually publishes as a gate.
- **The old ladder claimed a "Grade 5 (+theory gate)" rung** — "Grade 5 scales; pass Grade 5 Theory" (old rung 8). This is wrong on two counts, confirmed on p.5 and re-stated identically on pp.36, 39, 42 ("ENTRY REQUIREMENTS" boxes for Grades 6/7/8): (1) the theory-passing requirement is a **prerequisite to *entering* Grade 6, 7 or 8** — it is not something tested or required *at* Grade 5 itself; (2) it is **not Theory-only any more** — the actual rule is candidates must already hold "ABRSM Grade 5 (or above) in **Music Theory, Practical Musicianship, or a Practical Grades solo Jazz instrument**" (three accepted routes, p.5). I've moved this gate to the Grade 6 rung and stated it correctly.
- **The old ladder had a separate "Performance & expression" rung (old #9) with no grade number** — not a real ABRSM Practical Grade; it conflated ABRSM's *separate* Music Performance qualification track (p.4, "Practical Music candidates may wish to focus on performance skills and take a Music Performance qualification" — a different, parallel qualification, not a rung inside the Practical Grades ladder) with the graded pathway. Dropped — out of scope, it's a different qualification product.
- **"Aural & theory base" (old rung 6) as a standalone rung** — ABRSM doesn't have a standalone "aural/theory" grade; aural tests are integrated into every single grade exam (p.9, "What is assessed: Three set pieces / Scales and arpeggios / Sight-reading / Aural tests" — all four components at every grade, not a separate rung). Dropped as a distinct rung; every rung now carries its own real aural-test content instead.

## Real structure: 9 discrete steps, not 12

ABRSM Piano Practical Grades are **Initial Grade + Grades 1–8 = 9 formal, named, discrete qualifications** (Contents p.2; qualification numbers table pp.5-6; RQF/EQF mapping p.7). There is no official sub-grade or "half-grade" structure — a candidate is preparing for exactly one of these 9 named exams at a time. Padding this out to 12 rungs by inventing sub-stages (e.g. "early Grade 1" / "late Grade 1") would be fabricating structure ABRSM doesn't publish, which the plan's hard constraint rules out ("never fabricate content without having actually read the sourced PDF... if a source isn't available yet, the task is blocked, not guessed" — same principle applies to inventing structure the source doesn't contain).

So: **`MUSIC` shrinks from 12 hand-waved rungs to 9 real ones** (Initial, 1, 2, 3, 4, 5, 6, 7, 8). This is the "smallest sensible adjustment" the plan allows for when the source's real structure doesn't cleanly map onto the assumed rung count — here the honest adjustment is *fewer*, not more, because inflating to 12 has no basis in the source.

**Existing `LEVEL` indices stay valid with no re-pointing needed:** `LEVEL.sarah.music=2` → now lands on new index 2 = **Grade 2** (was old rung 3 "Scales & sight-reading", roughly Grade-1/2-ish territory — a reasonable, slightly-more-precise landing). `LEVEL.mirayah.music=0` → index 0 = **Initial Grade**, which is in fact the correct absolute beginner starting point (better than the old rung 1's vague "notes & rhythm basics" description). Both indices (2 and 0) are still `< 9`, so no `LEVEL` values needed changing.

## Per-rung content, sourced

Every field below is drawn from the syllabus's own per-grade "Scales and arpeggios" / "Sight-reading" / "Aural tests" boxes (one full page per grade, pp.20, 23, 26, 29, 32, 35, 38, 41, 44) plus the Aural test requirements section (pp.45-53) and the Piano repertoire lists (pp.18-44, three-piece structure explained pp.11-12). Page numbers are cited per rung.

### Initial Grade (p.18-20)
- **Pieces (p.11, p.18-19):** 3 pieces, one chosen from each of Lists A (fast/technical), B (lyrical/expressive), C (variety of styles/traditions) — e.g. List A "Gopak" (Alan Haughton), List B "Melody in G" (Beyer, Op.101 No.39), List C "The Elephant Herd" (June Armstrong). Duets allowed for one piece.
- **Scales/arpeggios (p.20):** C major scale + D minor scale (any form), 1 octave, hands separately; C major contrary-motion scale, a 5th, hands starting in unison; C major + D minor arpeggios, a 5th, hands separately. All from memory, legato, even notes.
- **Sight-reading (p.16 table):** 4 bars in 4/4 or 6 bars in 2/4; keys C major/D minor; each hand plays separately in 5-finger position (tonic–dominant); may include quarter/eighth/paired-eighth notes, legato phrases, staccato, *f*/*p*.
- **Aural (p.46):** clap the pulse of an examiner-played piece; clap back a 2-bar rhythm echo; sing back a 1-bar melodic echo (tonic–mediant range); answer one question about dynamics or articulation.

### Grade 1 (p.21-23)
- **Pieces (p.21-22):** e.g. List A "Fireworks Minuet" (Handel, arr. Bullard), List B "A Song of Erin" (Dunhill), List C "Cyberspace Detective" (Amit Anand).
- **Scales/arpeggios (p.23):** C major (hands together, 1 oct.); G, F majors + A, D minors (hands separately, 2 oct.); C major contrary-motion scale (1 oct.); G major + A minor arpeggios (hands separately, 1 oct.).
- **Sight-reading (p.16):** 3/4, 6 bars; keys add G, F majors + A minor; any 5-finger position; adds dotted-quarter/4-note-slurred groupings, slurs, accents, *mf*/*mp*, cresc./dim. hairpins.
- **Aural (p.46):** clap pulse + identify 2-time vs 3-time; sing 3-phrase echo (tonic–mediant); identify where in a phrase a pitch change happens (beginning/end); answer questions on 2 features (dynamics + articulation).

### Grade 2 (p.24-26)
- **Pieces (p.24-25):** e.g. List A "Sparkling Splashes & Smooth Water" (Barbara Arens), List B "The Singing Swan" (Alexis Ffrench), List C "Spooky Wood Hollow" (Heather Hammond).
- **Scales/arpeggios (p.26):** G, F majors + A, D minors hands together (2 oct.); D, A majors + E, G minors hands separately (2 oct.); C major contrary-motion (2 oct.); chromatic scale starting on D, hands separately (1 oct.); D, A major + E, G minor arpeggios hands separately (2 oct.).
- **Sight-reading (p.16):** up to 8 bars; keys add D major + E, G minors; playing together (not just separate hands); adds staccato/dotted-eighth patterns, tied notes, *pp*.
- **Aural (p.47):** clap pulse + 2/3-time id; sing 3-phrase echo (tonic–dominant range); identify a pitch **or** rhythm change; answer 2-feature questions (one from dynamics/articulation, plus tempo).

### Grade 3 (p.27-29)
- **Pieces (p.27-28):** e.g. List A "Allegro moderato" (Köhler, Sonatina in G), List B "Where is love?" (Bart, from *Oliver!*), List C "Allegretto" (Bartók, *For Children*).
- **Scales/arpeggios (p.29):** D, A majors + E, G minors hands together (2 oct.); Bb, Eb majors + B, C minors hands separately (2 oct.); E major contrary-motion (2 oct.); chromatic contrary-motion scale starting on D (1 oct.); D, A + Bb, Eb major and E, G + B, C minor arpeggios, mixed hands-together/separately (2 oct.).
- **Sight-reading (p.16):** up to 8 bars, 3/8 introduced; keys add A, Bb, Eb majors + B minor; outside 5-finger position; adds 2-note chords, eighth-sixteenth patterns, rests.
- **Aural (p.47):** clap pulse + 2/3/4-time id; sing 3-phrase echo in major **or minor**, octave range; identify pitch/rhythm change over up to 4 bars, major or minor; answer 2-feature questions incl. tonality (major/minor).

### Grade 4 (p.30-32)
- **Pieces (p.30-31):** e.g. List A "Allegro assai" (Benda, Sonata in G), List B "Waltz" (Grieg, *Lyriske smastykker* Op.12), List C "Danse du cocher" (Ibert).
- **Scales/arpeggios (p.32):** Bb, Eb majors + B, C minors hands together (2 oct.); B, F♯ majors + F♯, F minors hands separately (2 oct.); Eb major contrary-motion + C harmonic minor (2 oct.); chromatic scale starting F♯, hands together (2 oct.); Bb, Eb major + B, C minor arpeggios hands together, and B, F♯, Ab major + F♯, F minor arpeggios hands separately (2 oct.).
- **Sight-reading (p.16):** c.8 bars, 6/8 introduced; adds anacrusis, chromatic notes, pause signs, tenuto.
- **Aural (p.48):** sing/play from memory a melody played twice (octave range, major/minor, up to 3 sharps/flats); sing 5 notes from score at sight; answer 2-feature questions (one of dynamics/articulation/tempo/tonality, plus character); clap rhythm from an extract + identify 2/3/4-time.

### Grade 5 (p.33-35)
- **Pieces (p.33-34):** e.g. List A "La tarentelle" (Burgmüller, Op.100 No.20), List B "Someone You Loved" (Capaldi et al., arr. Iles), List C "The Village in May" (Hisaishi, *My Neighbour Totoro*, arr. Kawaura).
- **Scales/arpeggios (p.35):** A, E, B, F♯, Db majors + F♯, C♯, G♯, Eb, Bb minors hands together, legato (2 oct.); Ab major + F minor staccato hands separately (2 oct.); Db major legato contrary-motion + C♯ harmonic minor contrary-motion (2 oct.); chromatic contrary-motion starting a major 3rd apart (F♯ LH / A♯ RH) (2 oct.); same-key arpeggios as scales, legato hands together (2 oct.); diminished 7th starting on B, legato hands separately (2 oct.).
- **Sight-reading (p.16):** c.8-12 bars; keys add E, Ab majors + F♯, C minors; adds 4-part chords (max 2 notes/hand), simple syncopation, slowing tempo at end, *ff*.
- **Aural (p.49):** sing/play from memory a melody played twice (octave, major/minor, up to 3 sharps/flats); sing 6 notes from score at sight (5th above–4th below tonic, up to 2 sharps/flats); answer 2-feature questions (one of dynamics/articulation/tempo/tonality/character, plus style and period); clap rhythm + identify 2/3/4-time.

### Grade 6 (p.36-38) — **entry gate applies here, not at Grade 5**
- **Entry requirement (p.36, matches p.5):** candidate must already hold ABRSM Grade 5 (or above) in **Music Theory, Practical Musicianship, or a Practical Grades solo Jazz instrument** — this is the real theory gate, correctly placed at the Grade-6 rung (the old ladder wrongly attached it to "Grade 5").
- **Pieces (p.36-37):** e.g. List A "Invention No.14 in B flat" (J.S. Bach, BWV 785), List B "Bagatelle in F" (Hensel), List C "Stamping Dance" (Bartók, *Mikrokosmos* No.128).
- **Scales/arpeggios (p.38):** D, F, Ab, B majors + D, F, G♯, B minors (harmonic and melodic) hands together, legato or staccato at examiner's choice (4 oct.); same-key contrary-motion (2 oct.); chromatic scales starting G♯ and B, hands together (4 oct.); same-key arpeggios, hands together root position (4 oct.); dominant 7ths in D, F, Ab, B (4 oct.); diminished 7ths starting G♯ and B (4 oct.).
- **Sight-reading (p.16):** c.12-16 bars; 9/8, 5/8, 5/4 introduced; keys add C♯, F minors; adds triplet rhythms, clef changes, right pedal use.
- **Aural (p.50):** sing/play from memory the upper part of a 2-part phrase; sing a melody from score with piano accompaniment; identify a cadence as perfect or imperfect; answer 2-feature questions incl. texture/structure; clap rhythm + identify time signature.

### Grade 7 (p.39-41)
- **Entry requirement:** same Grade-5-Theory-or-equivalent gate as Grade 6 (p.39).
- **Pieces (p.39-40):** e.g. List A "Scherzo" (Beethoven, Sonata in A Op.2 No.2), List B "*" (Schumann, *Album für die Jugend* Op.68 No.30), List C "The Watermill" (Kahn).
- **Scales/arpeggios (p.41):** Db, E, G, Bb majors + C♯, E, G, Bb minors (harmonic and melodic), similar motion and thirds-apart, legato or staccato (4 oct.); same-key contrary-motion (2 oct.); G major legato **and** staccato scales in thirds, hands separately (2 oct.); chromatic contrary-motion starting a minor 3rd apart (C♯ LH / E RH) (2 oct.); same-key arpeggios hands together, first inversion only (4 oct.); dominant 7ths in Db, E, G, Bb (4 oct.); diminished 7ths starting Bb and E (4 oct.).
- **Sight-reading (p.16):** c.16-20 bars; 7/8, 7/4 introduced; adds tempo changes, 8va sign, una corda pedal use.
- **Aural (p.51):** sing/play from memory the lower part of a 2-part phrase; sing the upper part of a 2-part phrase from score with examiner playing the lower part; identify cadence as perfect/imperfect/interrupted, then name the two chords forming it (tonic/subdominant/dominant/dominant-7th/submediant); identify whether a modulation is to dominant/subdominant/relative minor; answer 2-feature questions incl. style/period/structure; clap rhythm + identify time signature incl. 6/8.

### Grade 8 (p.42-44) — top of the graded pathway
- **Entry requirement:** same Grade-5-Theory-or-equivalent gate (p.42).
- **Pieces (p.42-43):** e.g. List A "Alla Turca" (Mozart, Sonata in A K.331, 3rd movt), List B "La fille aux cheveux de lin" (Debussy, *Préludes* Book 1 No.8), List C "In the Dew, a Homage to Janáček" (Cheryl Frances-Hoad).
- **Scales/arpeggios (p.44):** C, Eb, F♯, A majors + C, Eb, F♯, A minors (harmonic and melodic), similar motion and 6ths-apart, legato/staccato (4 oct.); same-key contrary-motion (2 oct.); Eb major legato scale in thirds + C major staccato scale in sixths, hands separately (2 oct.); chromatic scale a major 6th apart (Eb LH / C RH) (4 oct.); whole-tone scales starting Eb and C, hands together (4 oct.); same-key arpeggios hands together, second inversion only (4 oct.); dominant 7ths in C, Eb, F♯, A (4 oct.); diminished 7ths starting Eb and C (4 oct.).
- **Sight-reading (p.16):** c.1 page; 12/8 introduced; keys add B, Db majors; adds 3-part chords in either hand, spread chords, simple ornaments, acceleration of tempo.
- **Aural (p.52-53):** sing/play from memory the lowest part of a 3-part phrase; identify a cadence as perfect/imperfect/interrupted/plagal, then name all three chords forming it; sing the lower part of a 2-part phrase from score with examiner playing the upper part; identify whether two different modulations are to dominant/subdominant/relative minor-or-major; freely describe the characteristic features (texture, structure, character, style, period) of an examiner-played piece.

## Honest gaps — what ABRSM does *not* specify

- **`mins` (daily practice minutes) is not an ABRSM-published figure anywhere in this document.** The syllabus publishes **Guided Learning Hours** and **Total Qualification Time** per grade (p.6) — but those are cumulative hours-with-a-teacher and hours-total-to-prepare-for-the-exam (spread over months), not a "practice X minutes today" figure the way the old ladder implied. I kept `mins` in the object (to preserve the exact shape `ws_music()` expects — see plan constraint) but the values are a **reasonable general-knowledge estimate** for a school-age learner's daily practice session length at each grade (rising 15→45 min, Initial→Grade 8), **not sourced from ABRSM** — flagged here per the plan's explicit instruction not to attribute invented numbers to the source.
- **Real piece titles are illustrative, not the full list.** Each grade publishes 16 pieces per list (48 total); I cited 1 real example per list per grade (verified from the syllabus tables) rather than reproducing the full copyrighted repertoire lists, consistent with "no piracy of copyrighted material" — candidates/teachers pick their own piece from the real published list, not a piece invented by this app.
- **Trinity syllabus was not sourced** (portal-gated, noted as a Phase-1 gap in the plan) — ABRSM is used as sole primary source per the plan's decision ("pick one as primary, note the other as cross-reference" — Trinity cross-reference is simply absent this round, not fabricated).

## Ladder-extension interaction (`intensifyRung`, `BASE_LADDER_LEN`)

`MUSIC` goes through the same generic extension system as every other ladder (`growth-map.html` lines ~419-427): once a learner passes index 8 (Grade 8, the last hand-authored rung), `intensifyRung` clones the Grade 8 object and scales every *numeric* field by `1 + 0.15*tier` — the only numeric field on a `MUSIC` entry is `mins`. So beyond Grade 8, the system's only mechanical way to "level up" is to inflate the practice-minutes number (Grade 8's ~45 min → ~52 → ~58 → …), labelled "Grade 8 — Independent Study 1/2/3…". This is a reasonable proxy signal (real post-Grade-8 progression is diploma-track ARSM/LRSM/FRSM performance qualifications — p.4 — which are a different, non-graded qualification family this ladder doesn't attempt to model) but it is purely a "practice more" nudge, not a real ABRSM diploma curriculum. Flagging per the task's own instruction rather than trying to extend the generator to fake diploma content.
