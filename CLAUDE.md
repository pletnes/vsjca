# CLAUDE.md — Jefferson CA for VS Code

## What this project is

A VS Code extension providing **syntax highlighting for Jefferson notation** (Conversation Analysis transcription), aimed at **non-programmer linguistics researchers**. Later phases add code completion, export tooling (pandoc → Word), and markdown integration.

## Who the user is

- Paul Anton: ML engineer, PhD, comfortable in TypeScript/Python/terminal.
- **End users are NOT programmers.** They are CA researchers who currently fight with Word. Every user-facing decision (install flow, docs, error messages) must assume zero technical background.

## Guiding principles

1. **Ship phase 1 (highlighting) before touching phase 2.** Resist scope creep.
2. **Declarative before imperative.** Basic highlighting needs only a TextMate grammar (JSON) + language configuration — no runtime code. Do not introduce a language server until completion/diagnostics genuinely require it.
3. **The grammar is the product.** Jefferson symbols are punctuation-heavy and overload common characters (`( )`, `[ ]`, `< >`, `:`, `=`). Regex precedence and disambiguation matter more than anything else. Test cases first.
4. **Unicode matters.** `↑ ↓ →` and degree signs appear in real transcripts. Files must be treated as UTF-8. Provide snippets/completions for symbols users can't type.
5. **Non-destructive by default.** Never modify a user's transcript on open/save without explicit action.

## Repository conventions

- Language: TypeScript (strict mode). Node LTS.
- Bundler: esbuild (via the official extension template).
- Package manager: npm (lockfile committed).
- Tests: `vscode-tmgrammar-test` for grammar snapshot tests; `@vscode/test-electron` only when runtime code exists.
- Formatting: Prettier defaults. No debates.
- Grammar lives in `syntaxes/jefferson.tmLanguage.json`. Keep scope names TextMate-conventional (`punctuation.definition.*`, `markup.*`, `constant.*`) so all themes color them sensibly.
- Documentation is, generally, in `docs/**`
- Documentation of Jefferson Transcription System, JTS, is in `docs/jefferson`
- Planning documents are in `docs/planning/*.md`

## Jefferson notation reference (canonical for this repo)

| Symbol | Meaning | Suggested scope |
|---|---|---|
| `(.)` | micropause | `constant.language.pause.micro` |
| `(0.7)` | timed pause | `constant.numeric.pause.timed` |
| `[` `]` | overlap boundaries | `punctuation.section.overlap` |
| `>text<` | faster pace | `markup.italic.pace.fast` |
| `<text>` | slower pace | `markup.italic.pace.slow` |
| `(   )` | unclear speech | `comment.block.unclear` |
| `((comment))` | analyst/transcriber comment | `comment.block.analyst` |
| underlining (`_word_` in plain text) | emphasis/volume | `markup.underline.emphasis` |
| `↑` / `↓` | intonation rise/fall | `keyword.operator.intonation` |
| `→` | analyst interest marker | `keyword.control.analyst-arrow` |
| `CAPITALS` | loud/shouted | `markup.bold.loud` |
| `(h)` | laughter | `constant.language.laughter` |
| `=` | latching (no pause between turns) | `keyword.operator.latch` |
| `:::` | stretched sound | `keyword.operator.stretch` |
| `Speaker:` at line start | turn label | `entity.name.tag.speaker` |

Disambiguation rules the grammar MUST get right:
- `(.)` vs `(0.7)` vs `(   )` vs `((...))` vs `(h)` — all start with `(`. Order patterns from most to least specific.
- `Speaker:` colon vs stretch colons — speaker labels anchor to line start.
- `>fast<` vs `<slow>` — greedy matching will break nesting; keep matches non-greedy and single-line.

## Workflow for Claude Code sessions

- Read `TODO.md` at session start; work top-down within the current phase.
- Update `TODO.md` checkboxes as part of the same commit as the work.
- New ideas go to `IDEAS.md`, not into scope.
- Open research questions live in `RESEARCH.md`; when answered, record the decision + rationale there and remove the question.
- Small commits, imperative mood messages ("Add timed-pause grammar rule").

## Commands

```bash
npm install            # deps
npm run watch          # esbuild watch (once template is generated)
npm test               # grammar snapshot tests
npx vsce package       # build .vsix for local install
```

## Out of scope (do not implement without explicit ask)

- CLAN/CHAT (`.cha`), ELAN (`.eaf`), Praat formats — different conventions, different projects.
- Audio alignment / playback.
- Anything requiring a hosted backend.
