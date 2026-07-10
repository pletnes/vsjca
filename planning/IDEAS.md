# IDEAS.md — Backlog

Nothing here is committed. Ideas graduate to TODO.md only after Phase 1 ships and users ask for them.

## Export & interop

- **Pandoc → Word**: one command, `.jca`/markdown in, journal-ready `.docx` out, with real underlining instead of `_underscores_`
- **Word interop, non-destructive**: open a `.docx`, extract the transcript, work in plain text, export a fresh copy — never edit the original in place
- **Markdown-first authoring**: injection grammar so ```jefferson fenced blocks highlight inside `.md`; whole paper in one repo, pandoc builds the submission
- **Web export**: generate Monarch (Monaco) / highlight.js / CodeMirror grammars from one source of truth — reuse for the existing Monaco web app
- **LaTeX export**: some CA venues use LaTeX; `gb4e`/custom environments for numbered extracts
- **HTML export with CSS**: for teaching materials and blogs; underline/color preserved

## Editing experience

- **Timestamp/pause arithmetic**: select region, command shows total timed-pause duration
- **Speaker management**: rename speaker across file; consistent color per speaker
- **Alignment helper**: monospace overlap alignment assistant — place `[` in the right column relative to the line above
- **Symbol hover docs**: hover `↑` → "rise in intonation" (great for students learning the notation)
- **Transcript linting**: style-guide checks (e.g., timed pauses ≥ 0.2s, micropause otherwise)
- **Snippet library per "school"**: Jeffersonian variants differ slightly by lab/journal; shippable presets

## Teaching & community

- **Interactive tutorial transcript**: a sample file that teaches the notation as you scroll (pairs with the walkthrough)
- **"CA starter kit"**: VS Code settings profile (monospace font, word wrap off, zoom) importable in one click
- **Video walkthrough**: 3-minute screen recording for the Marketplace page
- **University methods-course outreach**: instructors are the multiplier — one convinced teacher = 30 users/year

## Long shots

- **Audio companion**: open the audio file alongside; keyboard shortcut inserts current timestamp (competes with dedicated tools — only if users beg)
- **Collaboration**: Live Share works out of the box — document it rather than build anything
- **CHAT/CLAN converter**: bridge to CHILDES ecosystem (`.cha` ↔ Jefferson, lossy but useful)
- **LLM-assisted first-pass transcription cleanup**: speech-to-text output → rough Jefferson skeleton for the human to refine (Paul's home inference rig could prototype this)
