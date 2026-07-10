# Jefferson notation — technical grammar breakdown

Programmer-oriented specification of the notation, distilled from Jefferson (2004),
Hepburn & Bolden (2013/2017), Hutchby & Wooffitt (2008), CHAT-CA (TalkBank), and
DOTE. Where sources disagree, see [ambiguities.md](ambiguities.md).

**Encoding assumption:** files are UTF-8. Historically the system predates
computers (typewriter + pen); several "symbols" are typographic effects
(underlining, superscripts, column alignment) with multiple plain-text encodings
in the wild. All are listed here.

## 1. Document structure (line model)

Following DOTE's formalization, a transcript is a sequence of *neighbourhoods*
(temporally ordered blocks). Line types:

```
transcript      := line*
line            := speaker-line | silence-line | comment-line | continuation-line | blank
speaker-line    := line-number? margin-arrow? speaker-label WS talk
silence-line    := line-number? WS* pause                     ; e.g. "7     (2.8)"
comment-line    := line-number? WS* comment+                  ; e.g. "3   ((door slams))"
continuation-line := line-number? WS* talk                    ; overlap-aligned continuation
```

- `line-number` — decimal integer, left margin, optional but near-universal in
  published work. Not part of the notation itself; added for citability.
- `margin-arrow` — `→` (U+2192) or ASCII `->` in the left margin: analyst's
  pointer to lines discussed in the text. Not speech.
- `speaker-label` — identifier anchored at start of line (after number/arrow),
  terminated by `:` (U+003A) then whitespace. Conventionally 3–8 chars, often
  abbreviated names (`Nan:`, `CPO:`), may include digits (`??:` for unknown
  speaker). **Disambiguation:** a colon at line start position followed by
  whitespace is a label terminator; colons inside talk are sound stretches.
- Alignment: talk columns are padded with spaces so overlap brackets line up
  vertically. Alignment is *semantically load-bearing* for `[`/`]` (see §3.1)
  and requires a monospace font. Spaces inserted *inside* words for alignment
  carry no phonetic meaning (Hepburn & Bolden fn. 10).

## 2. Silences

| Notation | Code points | Meaning | Rule |
|---|---|---|---|
| `(0.5)` | `(` U+0028, digits, `.` U+002E, `)` U+0029 | timed silence, tenths of a second | regex `\(\d+\.\d\)`; values ≥ 1.0 s stay in this format (`(1.2)`), whole seconds written `(1.0)` not `(1)` |
| `(.)` | U+0028 U+002E U+0029 | micropause, hearable, < ~0.2 s | exact literal |

Placement semantics (not grammar-relevant but worth knowing): silence between
turns ("gap") goes on its own line; within a turn ("pause") stays on the
speaker's line.

## 3. Temporal / sequential symbols

### 3.1 Overlap

| Notation | Code points | Meaning |
|---|---|---|
| `[` | U+005B | overlap onset; vertically aligned across ≥2 adjacent lines |
| `]` | U+005D | overlap offset; alignment as above; offset marking is optional ("lazy overlap") |
| `[[` | U+005B ×2 | **legacy**: two turns starting simultaneously (Sacks-era; DOTE rejects) |
| `//` | U+002F ×2 | **legacy**: overlap onset in 1960s–70s transcripts (e.g. *Systematics* 1974) |
| `⌈ ⌉ ⌊ ⌋` | U+2308, U+2309, U+230A, U+230B | typographic variant of `[ ]` used with CAfont/CLAN for pixel-precise alignment (top corners = onset, bottom = offset) |
| `{ }` | U+007B, U+007D | DOTE extension: "non-sequential" overlap (ambient sound overlapping talk) |

Notes for a grammar: `[` and `]` are unpaired *within a line* — the pair spans
two or more *lines*. A per-line tokenizer should treat each bracket as a
standalone token, not try to match pairs on one line (though pairs on one line
are common when one speaker's talk contains a complete overlapped span).

### 3.2 Latching

| Notation | Code points | Meaning |
|---|---|---|
| `=` | U+003D | no discernible silence between two turns/TCUs. Turn-final `=` links to turn-initial `=` on a following line (possibly not adjacent, when overlap lines intervene); also marks one speaker's through-produced talk split across transcript lines |

## 4. Speech delivery

### 4.1 Terminal / unit intonation (punctuation ≠ grammar)

| Notation | Code points | Meaning |
|---|---|---|
| `.` | U+002E | falling ("period") intonation |
| `,` | U+002C | slightly rising ("continuing") intonation |
| `?` | U+003F | strongly rising intonation |
| `¿` | U+00BF | rise stronger than `,`, weaker than `?` (variant: `?,`) |
| `_` (turn-final) | U+005F | level/flat intonation (some transcribers leave flat unmarked) |
| `!` | U+0021 | animated/emphatic tone (Hutchby & Wooffitt; not in Jefferson 2004 core) |

### 4.2 Stretch, cut-off, pace

| Notation | Code points | Meaning | Rule |
|---|---|---|---|
| `:` `::` `:::` | U+003A ×n | stretch of preceding sound; count ∝ duration (analog, unbounded) | must not match speaker-label colon; attach to preceding grapheme |
| `-` | U+002D | cut-off of preceding word/sound (glottal stop) | word-final position; distinguish from intra-word hyphens in orthography |
| `>text<` | U+003E … U+003C | compressed/rushed talk | non-greedy, single line |
| `<text>` | U+003C … U+003E | slowed/drawn-out talk | non-greedy, single line |
| `<` (unpaired, turn-medial) | U+003C | "jump-start"/left-push: following talk begins abruptly early (Schegloff 2005) | makes naive pair-matching fail; see ambiguities |

### 4.3 Volume and emphasis

| Notation | Code points | Meaning |
|---|---|---|
| underlining | see below | stress/emphasis; more underlining = more emphasis |
| `CAPITALS` | A–Z | markedly loud talk (≥2 caps to avoid `I`, proper nouns) |
| `°text°` | U+00B0 … U+00B0 | markedly quiet |
| `°°text°°` | U+00B0 ×2 | whispered / sotto voce / mouthed |

Underlining encodings found in the wild:

1. Word-processor underline formatting (lost in plain text) — the historical norm.
2. Combining low line U+0332 after each underlined character (CLAN/CAfont, e.g. `U̲o̲:̲h̲`).
3. `_word_` underscore-wrapping — used in some plain-text/email contexts; **not**
   attested in the formal literature (our repo's assumption; see ambiguities).
4. Capitalization as a lossy substitute (discouraged; collides with loudness).

Watch for degree-sign confusables users may type: U+00BA (º masculine ordinal),
U+02DA (˚ ring above), U+2070 (⁰ superscript zero). Canonical is U+00B0.

### 4.4 Pitch

| Notation | Code points | Meaning |
|---|---|---|
| `↑` | U+2191 | sharp pitch rise / register reset up (placed before the shift; may bracket a span: `↑we pl'se bring↑`) |
| `↓` | U+2193 | sharp pitch fall |
| `↑↑` `↓↓` | ×2 | extreme pitch reset |
| `^` | U+005E | ASCII variant of `↑` |
| `˘` or `\|` | U+02D8, U+007C | variants of `↓` (breve, pipe) |
| underline+colon combos | U+0332 etc. | intra-word contour: underlined letter + colon = up-then-down through the word (`pa̲:ssing`); underlined colon = slide up (`shi:̲rt`) — typographically fragile, see ambiguities |

### 4.5 Voice quality

| Notation | Code points | Meaning |
|---|---|---|
| `£text£` | U+00A3 | smile voice / suppressed laughter (Jefferson 2004b) |
| `#text#` | U+0023 | creaky voice (Hepburn & Bolden) |
| `~text~` | U+007E | tremulous/wobbly voice, upset (Hepburn 2004) |
| `*` | U+002A | croaky pronunciation of following section (Hutchby & Wooffitt variant) |
| `gh` inside word | letters | guttural pronunciation (`Thaght`) |

## 5. Breathing, laughter, crying

| Notation | Code points | Meaning | Rule |
|---|---|---|---|
| `hh`, `hhh` | U+0068 ×n | out-breath; count ∝ length | standalone token or word-attached (`yeahh`) |
| `.hh` | U+002E + h's | in-breath | leading `.`; older texts superscript the dot; CLAN uses `∙h` (U+2219) or `·h` (U+00B7) |
| `(h)` | U+0028 h U+0029 | plosive laughter particle interpolated in a word (`thi(h)nk`) | inside word boundaries |
| `huh/hah/heh/hih/heh` | letters | free-standing laughter tokens, open set, case carries volume (`HAH`) | no closed vocabulary — treat as lexical, not symbolic |
| `.snih`, `.shih`, `.skuh` | letters + `.` | sniff/sob tokens (crying; Hepburn 2004) | leading `.` = inhaled |
| `tk`, `tch`, `pt` | letters | clicks/lip-smacks | lexicalized |

## 6. Transcriber metacommentary

| Notation | Code points | Meaning | Rule |
|---|---|---|---|
| `( )` | U+0028, spaces, U+0029 | unclear talk, no hearing achieved | ≥1 space inside; width ≈ duration (analog!) |
| `(word)` | parens + letters | uncertain hearing / best guess | |
| `(it's/its)` | with U+002F | alternative hearings | slash-separated |
| `((description))` | U+0028 ×2 … U+0029 ×2 | transcriber's comment/description, not speech | non-greedy; may fill a whole line; overlap brackets inside are undefined (DOTE disallows) |

## 7. Parenthesis-family disambiguation order

All of these start with `(` U+0028. A grammar must try patterns
most-specific-first:

```
1. ((...))          analyst comment        \(\((?:[^)]|\)[^)])*\)\)
2. (0.7) / (1.2)    timed silence          \(\d+\.\d\)
3. (.)              micropause             \(\.\)
4. (h)/(hh)         laughter particle      \(h+\)         (word-internal)
5. (   )            unclear, no guess      \(\s+\)
6. (word/word)      alternative hearings   \([^()\s][^()]*/[^()]+\)
7. (word)           uncertain hearing      \([^()]+\)     (fallback)
```

Similar precedence chains needed for:

- `:` — speaker label (line-anchored) → stretch (talk-internal).
- `.` — in-breath prefix (`.hh`) → intonation (post-word) → decimal point (inside `(n.n)`).
- `<`/`>` — `>fast<` → `<slow>` → unpaired `<` jump-start.
- `_` — turn-final level intonation vs. underscore-emphasis convention (if adopted).
- `°` — pair vs. doubled pair (`°°`): match longest first.

## 8. Canonical-vs-variant summary (tiering proposal for our grammar)

**Tier 1 (Jefferson 2004 / Hepburn & Bolden core):**
`[ ]  =  (n.n)  (.)  .  ,  ?  ¿  :  -  >< <>  °  °°  CAPS  underline  ↑ ↓  hh  .hh
(h)  ( )  (word)  ((comment))  £  #  ~  line numbers, speaker labels, → margin arrow`

**Tier 2 (documented variants/legacy — highlight but maybe flag):**
`^  ˘  |  ?,  _final  !  *  //  [[  { }  ⌈⌉⌊⌋  ∙h/·h  U+0332 underlining  ↑↑ ↓↓`

**Out of scope:** GAT-2, CHAT-CA-specific markup (`&=laughs`, header tiers), DOTE
subtiers (`@gls`, `.eng` translation lines) — though the line model should not
*break* on interlinear gloss/translation lines.

Sources: see [bibliography.md](bibliography.md). Unresolved issues: see
[ambiguities.md](ambiguities.md).
