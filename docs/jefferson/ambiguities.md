# Unclear and ambiguous rules in Jefferson notation

The system was invented by a typist-turned-sociologist on a typewriter, evolved
organically alongside analysis for 40 years, and was never formally specified.
Jefferson (2004) is a narrated glossary with examples, not a grammar. The points
below matter for anyone writing a parser or highlighter, with pointers to
published discourse where it exists.

## 1. There is no spec, and usage genuinely varies

- Bucholtz (2007, "Variation in transcription") and Mondada (2007) document that
  different researchers apply the "same" conventions differently; Jefferson (1996,
  "A case of transcriptional stereotyping") showed even she was inconsistent.
- Hepburn & Bolden (2013) repeatedly note variant practices in footnotes: some
  transcribers leave level intonation unmarked, some underline whole words vs.
  vowels only (fn. 8), some use `^`/`|` instead of arrows (fn. 9).
- Tool authors confirm this: DOTE explicitly triages symbols into
  *normative / agnostic / clash* categories because it cannot parse everything
  claimed as "Jeffersonian"; GailBot (Umair et al. 2022) implements only the
  algorithmically decidable subset (timing, overlap, laughter, rate).
- **Consequence for us:** the grammar should be permissive: highlight what it
  recognizes, never error on what it doesn't. IDEAS.md's "snippet library per
  school" is well-founded.

## 2. Typographic effects don't survive plain text

The two worst offenders were both designed for typewriter/Word, not char streams:

### 2.1 Underlining (emphasis + pitch contours)

- Emphasis is *degree-bearing*: "the more underlining within a word, the greater
  the emphasis" (H&B). A binary `_word_` markup loses the per-character analog scale.
- The pitch-contour notation is worse: underlined-letter-then-colon (`pa̲:ssing`,
  up-then-down) vs. letter-then-underlined-colon (`les'n:̲s`, slide up). This is a
  *positional distinction between which character is underlined*. In Unicode it
  requires combining low line U+0332 on specific chars (CLAN's solution, needs
  CAfont or similar to render); in Word it's underline formatting; in ASCII it's
  unrepresentable.
- Our `_word_` assumption (RESEARCH.md §5) is a pragmatic invention, not attested
  practice. Real plain-text corpora (TalkBank) use U+0332. **Decision needed:**
  support both `_..._` (typeable) and U+0332 (interoperable)?

### 2.2 Overlap alignment is 2-D, not 1-D

- The `[` of the overlapping line must sit in the same *column* as the `[` above.
  Semantic content is carried by screen geometry (monospace font + manual space
  padding), not by the token stream.
- Padding spaces inserted inside/around words for alignment are semantically void
  (H&B fn. 10) — a parser cannot distinguish them from "unclear talk" width inside
  `( )`, which *is* meaningful (width ≈ duration). Two opposite meanings of runs
  of spaces.
- Multi-line overlaps chain with `=` continuation, giving interleaved
  bracket-and-equals structures (see the "Location, Location, Location" extract in
  H&B) that no regular grammar can validate; DOTE implements a whole
  "realign overlaps" heuristic command for this.

## 3. Character overloading (the `(` problem and friends)

- `(` opens seven constructs: `(.)`, `(0.7)`, `( )`, `(word)`, `(word1/word2)`,
  `(h)`, `((comment))`. Only resolvable most-specific-first (grammar-spec §7).
- `:` is speaker-label terminator *and* stretch marker *and* appears inside
  timestamps in some house styles.
- `.` is intonation *and* in-breath prefix (`.hh`) *and* decimal point *and*
  micropause body *and* ordinary abbreviation dot in comments.
- `.` `,` `?` mean intonation, but visually equal ordinary orthographic punctuation
  — and transcripts embed ordinary orthography inside `((comments))`. A highlighter
  cannot tell "intonation period" from "sentence period" in a comment; it must
  scope-switch inside `((...))`.
- `-` cut-off vs. hyphenated compounds ("twenty-two").
- `<` opens slow-pace spans *and* stands alone as jump-start — unbalanced by
  design (Schegloff 2005: "left push"). Naive auto-pairing (`<` → `>`) will
  actively annoy transcribers; validate the Phase 2 auto-close idea against this.
- `h` is a letter *and* the aspiration morpheme; `yeahh` (breathy) vs `yeah`
  (lexical) differ by counting h's. GailBot ducks this by treating laughter
  lexically; so should our tier 1.
- `=` latching vs. `=` in interlinear gloss lines (DOTE subtiers) if glosses appear.

## 4. Analog, unbounded notation

Several symbols encode *quantity by repetition* with no upper bound and no
canonical granularity — awkward for schema-minded tooling, fine for regex:

- colons (`no:` … `no::::::`), h's (`hh` … `HHHHH`), underlining degree,
  doubled degree signs, doubled arrows, width of `( )`.
- O'Connell & Kowal (1994) criticize exactly this pseudo-precision; GAT-2 was
  designed partly in response, with discrete categories.

## 5. Divergent symbol assignments between schools/eras

| Feature | Variant A | Variant B | Notes |
|---|---|---|---|
| pitch rise/fall | `↑`/`↓` (Jefferson 2004) | `^` / `˘` or `\|` (H&B fn. 9) | ASCII-driven variance |
| creaky voice | `#` (Hepburn & Bolden) | `*` (Hutchby & Wooffitt) | direct collision |
| overlap onset | `[` (modern) | `//` (pre-1980s), `[[` turn-initial | legacy corpora still circulate |
| intermediate rise | `¿` | `?,` | both live |
| in-breath dot | `.hh` baseline | superscript dot (print), `∙h`/`·h` (CLAN) | U+002E vs U+2219 vs U+00B7 |
| level intonation | `_` turn-final | unmarked | H&B fn. 6 |
| overlap brackets (print) | `[ ]` | `⌈⌉⌊⌋` U+2308–U+230B | CAfont/LaTeX practice (Albert 2015) |
| animated tone | `!` (Hutchby & Wooffitt) | not in Jefferson 2004 core | |

Unicode confusables typed by real users: `°` U+00B0 vs `º` U+00BA vs `˚` U+02DA;
`→` U+2192 vs `->`; smart quotes from Word round-tripping. The grammar (and later
diagnostics) should anticipate these — most transcripts pass through Word at some
point in their life.

## 6. Structural conventions that are convention, not rule

- Line numbers: near-universal in print, absent while transcribing. Both must parse.
- Speaker labels: no defined charset/length; `??:` (unknown), `Cont:`, digits,
  non-Latin names all occur. Re-used label = same speaker, but nothing enforces it.
- Gap-on-own-line vs pause-on-speaker-line placement (Sacks/Schegloff/Jefferson
  TCU semantics) is undecidable lexically — it encodes an *analysis* of turn
  structure, not a surface fact.
- Translation/gloss subtiers for non-English data (three-line format; H&B §3.3)
  have no Jefferson-canonical syntax at all; DOTE invented `@gls`/`.eng` suffixes.

## 7. Published critique wrt. formalization (pointers)

- **Ochs (1979) "Transcription as theory"** — transcripts encode theory; relevant
  because our *highlighting choices* (what gets colored as "one thing") are also
  theory-laden.
- **O'Connell & Kowal (1994)** — systematic external critique of CA notation's
  precision claims and internal inconsistency.
- **MacWhinney & Wagner (2010), CHAT-CA** — the TalkBank re-encoding: dedicated
  Unicode characters, strict word-level rules, lossy round-trip to classic
  Jefferson ("CA (approximate Jeffersonian)" is literally CLAN's export label).
  Proof that exact machine-readable Jefferson requires changing the notation.
- **Umair et al. (2022), GailBot** — which features are computable from audio and
  which require human judgment; their plugin taxonomy is effectively a
  formalization boundary.
- **DOTE help pages** (normative/agnostic/clash triage; neighbourhood model;
  realignment heuristics) — the closest existing thing to an implementer's spec.
- **Albert (2015)** and the EMCA-wiki file-format pages — practitioner pain around
  fonts (CAfont), combining characters, and Word interop.

## 8. Cross-check against this repo's own docs (gaps found)

Reviewed CLAUDE.md, planning/TODO.md, planning/RESEARCH.md against the literature:

1. **CLAUDE.md table says `(h)` = laughter** — correct but incomplete: it's a
   *word-internal* plosive laughter particle; free-standing laughter is `huh/heh/hah`
   tokens, and plain `hh`/`.hh` are breathing, not laughter. Grammar rules should
   separate these three.
2. **CLAUDE.md suggested scope `markup.italic.pace.fast` for `>text<`** — fine,
   but note the unpaired `<` jump-start means pace rules must not swallow it.
3. **CLAUDE.md `_word_` for underlining** — flagged above (§2.1): pragmatic, not
   canonical; consider also supporting U+0332 for TalkBank interop.
4. **TODO.md "Laughter `(h)`, breathiness variants `(hh)`, `.hh`"** — `.hh` is
   in-breath, not a laughter/breathiness variant of `(h)`; and `(hh)` inside
   parens vs bare `hh` differ (in-word particle vs free aspiration).
5. **CLAUDE.md `:::` listed as "stretched sound" symbol** — the symbol is `:`,
   repeatable; `:::` is not a distinct token. Matching should be `:+` after a
   sound, minus speaker-label context.
6. **Missing from CLAUDE.md core table but tier-1 in the literature:** cut-off `-`,
   latching context rules, `¿`, `°`/`°°`, `£`, `#`, `~`, `hh`/`.hh`, `(word)` best
   guess, `(a/b)` alternative hearings, `!`, turn-final `_`, `CAPITALS` minimum
   length caveat (`I` vs loud), margin arrow vs in-talk arrow.
7. **RESEARCH.md §1 file-extension question** — unaffected by findings, but note
   CHAT-CA (`.cha`) collision concern is confirmed: TalkBank *does* distribute
   Jefferson-derived material as `.cha`; staying clear of it is right.
8. **Phase 2 auto-pairing idea (TODO.md)** — literature gives concrete reasons to
   be careful: unpaired `<`, unpaired lazy `]`, `((` auto-close is safe, `[` is not
   (its pair lives on the *next* line).
