# Jefferson CA for VS Code — Project Description

## The problem

Conversation Analysis researchers transcribe using Jefferson notation — a dense system of pauses `(0.7)`, overlaps `[ ]`, intonation arrows `↑↓`, latching `=`, and stretched sounds `:::`. Today they do this in Microsoft Word, which is painful: no visual distinction between notation and speech, autocorrect mangles symbols, special characters require Insert → Symbol gymnastics, and collaboration on plain conventions is fragile.

## The vision

Make VS Code a genuinely pleasant transcription environment for non-programmers:

1. **See** — syntax highlighting turns a wall of punctuation into readable, color-coded structure.
2. **Type** — completions and snippets remove the "how do I type ↑" problem entirely.
3. **Publish** — researchers write in plain text/markdown, and one command produces the Word doc that journals demand, without ever opening Word.

## Phases

### Phase 1 — Syntax highlighting (MVP)
A declarative VS Code extension: language definition + TextMate grammar + sensible default theme mappings. Zero runtime code. Installable from the Marketplace in two clicks.

**Done when:** a `.jca` file opens with all Jefferson symbols visibly distinguished under the default Dark+ and Light+ themes, and grammar tests pass on a corpus of real-world sample transcripts.

### Phase 2 — Editing assistance
- Completions/snippets for symbols (type `pause` → `(0.7)` with tab-stop on the number; `up` → `↑`).
- Bracket/paren matching config tuned for Jefferson (auto-close `((` → `))`).
- Basic diagnostics: unbalanced overlap brackets, malformed timed pauses.

### Phase 3 — Real-world feedback & export
Ship to actual researchers, then prioritize among:
- **Pandoc export** to `.docx` with proper underlining and formatting.
- **Markdown integration** — Jefferson blocks inside markdown (injection grammar), so the whole paper lives in one file.
- **Non-destructive Word interop** — read transcripts out of `.docx`, never write back destructively.
- **Web export** — emit highlighting rules for Monaco / highlight.js / CodeMirror so the notation renders on the web too.

## Audience & distribution

End users are linguistics researchers with no programming background. Distribution is the VS Code Marketplace (plus Open VSX for VSCodium). Onboarding must be: install VS Code → install extension → open file → it works. The extension ships an in-editor **walkthrough** (VS Code's built-in onboarding UI) and a README written for humans, with GIFs.

## Success criteria

- A researcher who has never used VS Code can go from nothing to a highlighted transcript in under 10 minutes using only our instructions.
- At least one real CA researcher uses it for a real transcript and gives feedback.
- Grammar handles messy real-world transcripts without catastrophic mis-highlighting.

## Relationship to prior work

Paul has previously prototyped Jefferson tooling as a Monaco web app and VS Code experiments. This project is the clean, distributable rebuild: Marketplace-ready, tested, documented for non-programmers.
