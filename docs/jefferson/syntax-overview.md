# Jefferson notation, in plain language

Jefferson notation is the standard way conversation analysts write down talk.
It captures not just *what* people said, but *how* they said it: pauses, overlaps,
emphasis, volume, pitch, breathing, laughter. It was developed by Gail Jefferson
from the 1960s onward and is designed to be readable: it reuses familiar writing
habits (CAPITALS for loud, underlining for stress, arrows for pitch).

A transcript looks like this:

```
1  CPO:    It's a bit acrimonious [is it,]
2  Caller:                        [Ye:s  ] iddis.
3  CPO:    Ri:ght. Okay, .hh ER:m (0.3) tk (.) Ri:ght.
4          (0.5)
5  CPO:    .hh I MEAN THERE MAY BE (.) VARIous rea:sons,
```

Every line is numbered so researchers can point at exact moments. Each speaking
turn starts with the speaker's name (or initials) and a colon. Words are spelled
the way they were actually pronounced ("iddis" for "it is"), not corrected.

## Silences

- `(0.5)` — a silence, measured in tenths of a second. On its own line if it
  falls between speakers, on the speaker's line if it falls inside their talk.
- `(.)` — a "micropause": hearable, but shorter than about 0.2 seconds.

## Two people talking at once

- `[` marks where overlapping talk starts, `]` where it ends. The brackets on the
  two lines are aligned vertically, one directly above the other — which is why
  transcripts must use a monospace (typewriter-style) font.
- `=` ("latching") joins two stretches of talk with *no* gap at all between them —
  either two speakers, or two lines of one speaker interrupted on the page by
  someone else's overlap.

## How the words were delivered

- `wo:rd` — a colon means the sound before it was stretched. More colons,
  longer stretch: `no:::`.
- `wor-` — a dash means the word was cut off abruptly.
- `word` <u>underlined</u> — emphasis or stress. (In plain-text files this is the
  hard one; see the technical docs.)
- `WORD` — noticeably louder than the surrounding talk.
- `°word°` — noticeably quieter; `°°word°°` — whispered.
- `↑word` / `↓word` — a sharp jump up or down in pitch.
- `>words<` — spoken faster than the surrounding talk; `<words>` — slower.
- `.` `,` `?` at the end of a phrase mark intonation, *not* grammar: falling,
  slightly rising, and strongly rising. `¿` is sometimes used for "in between".

## Breathing, laughter, voice quality

- `hh` — audible out-breath; `.hh` — audible in-breath. More h's, longer breath.
- `huh / heh / hah / hih` — laughter, written out particle by particle.
- `wo(h)rd` — an `(h)` inside a word means the person laughed while saying it.
- `£word£` — said with a "smiley" voice; `#word#` — creaky voice; `~word~` —
  trembling voice (often crying).

## When the transcriber steps in

- `( )` — empty parentheses: something was said, but it couldn't be made out.
- `(word)` — the transcriber's best guess at unclear talk.
- `((coughs))`, `((phone rings))` — double parentheses hold the transcriber's own
  descriptions and comments, not speech.
- `→` in the left margin — the analyst is pointing at this line in the write-up.

That's the core system. Individual researchers and labs add or vary symbols
(there are documented variants for pitch, creak, and more), but a transcript using
only the conventions above will be understood by any conversation analyst.

Sources: Jefferson (2004), Hepburn & Bolden (2013/2017) — see
[bibliography.md](bibliography.md).
