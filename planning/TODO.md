# TODO

Work top-down within the current phase. Move stray ideas to `IDEAS.md`.

## Phase 0 — Decisions & scaffolding

- [ ] **Decide file extension convention** (see RESEARCH.md → answer, record decision here)
- [ ] Decide extension name + Marketplace publisher ID (needs an Azure DevOps account for vsce)
- [ ] Scaffold extension: `npx --package yo --package generator-code -- yo code` → "New Language Support"
- [ ] Set up repo: TypeScript strict, esbuild, Prettier, `.gitignore`, MIT license
- [ ] Add `vscode-tmgrammar-test` and a `samples/` folder
- [ ] Collect 3–5 real(istic) Jefferson transcript samples into `samples/` (test corpus)

## Phase 1 — Syntax highlighting

- [ ] `language-configuration.json`: comments (none), brackets `[]`, auto-closing pairs `(( ))`, `[ ]`, word pattern
- [ ] TextMate grammar `syntaxes/jefferson.tmLanguage.json`:
  - [ ] Speaker labels (`^\s*\w+:`)
  - [ ] Micropause `(.)`
  - [ ] Timed pause `(0.7)` — integer and decimal variants
  - [ ] Laughter `(h)`, breathiness variants `(hh)`, `.hh`
  - [ ] Unclear speech `(   )` and best-guess `(word)`
  - [ ] Analyst comments `((...))`
  - [ ] Overlap brackets `[` `]`
  - [ ] Pace markers `>fast<` and `<slow>` (non-greedy, single-line)
  - [ ] Intonation `↑` `↓`, analyst arrow `→`
  - [ ] Latching `=`
  - [ ] Stretch colons `:::` (disambiguate from speaker colon)
  - [ ] Emphasis `_word_`
  - [ ] Loud speech `CAPITALS` (2+ letters)
- [ ] Grammar snapshot tests for every rule + adversarial cases (nested/adjacent symbols)
- [ ] Verify rendering under Dark+, Light+, and one popular third-party theme
- [ ] Extension icon (simple, recognizable at 128px)
- [ ] README for the Marketplace page: plain-language, screenshots/GIF
- [ ] Package with `vsce package`, install `.vsix` locally, dogfood on samples
- [ ] Publish to VS Code Marketplace
- [ ] Publish to Open VSX

## Phase 2 — Editing assistance

- [ ] Snippets: timed pause, analyst comment, overlap pair, arrows, degree signs
- [ ] CompletionItemProvider for symbol names → Unicode chars (`up` → ↑, `arrow` → →)
- [ ] Diagnostics: unbalanced `[ ]`, unclosed `((`, malformed `(0.7)`
- [ ] Decide: plain extension APIs vs Language Server Protocol (default: plain APIs, LSP only if a second editor target appears)
- [ ] In-editor walkthrough (`contributes.walkthroughs`): install → open file → type your first pause
- [ ] Onboarding doc for non-programmers: "VS Code for CA researchers in 10 minutes"

## Phase 3 — Feedback & export (prioritize after user feedback)

- [ ] Recruit 2–3 CA researchers to trial it; set up a feedback channel (GitHub issues template written for non-programmers + email fallback)
- [ ] Pandoc pipeline: `.jca`/markdown → `.docx` with true underline formatting
- [ ] Markdown injection grammar: highlight Jefferson inside fenced ```jefferson blocks
- [ ] Export grammar to web targets (Monaco monarch / highlight.js / CodeMirror)
- [ ] Non-destructive `.docx` reading (extract transcript text, never overwrite)

## Housekeeping (ongoing)

- [ ] CHANGELOG.md kept current per release
- [ ] Version tags + GitHub releases with `.vsix` attached (offline install path for locked-down university machines)
