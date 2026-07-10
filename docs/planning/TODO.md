# TODO

Work top-down within the current phase. Move stray ideas to `IDEAS.md`.

## Phase 0 — Decisions & scaffolding

- [ ] **Decide file extension convention** (see RESEARCH.md → answer, record decision here). Scaffold provisionally registers `.jca` — one line in `package.json` to change.
- [ ] Decide extension name + Marketplace publisher ID (needs an Azure DevOps account for vsce). Manifest uses `jefferson-ca` + a placeholder publisher until then.
- [x] Scaffold extension — hand-rolled instead of `yo code`: a language-only extension is six declarative files; the generator adds nothing for Phase 1 (rationale in RESEARCH.md §3)
- [x] Set up repo: Prettier, `.gitignore`, MIT license. TypeScript strict + esbuild **deferred to Phase 2 start** — Phase 1 has zero runtime code, so there is nothing to compile yet
- [x] Add `vscode-tmgrammar-test` and a `samples/` folder (`npm test` / `npm run test:update`)
- [ ] Collect 3–5 real(istic) Jefferson transcript samples into `samples/` (test corpus) — one synthetic demo exists; need realistic ones per bibliography.md §4

## Phase 1 — Syntax highlighting

- [x] `language-configuration.json`: comments (none), brackets `[]`/`()`, auto-closing `(`→`)` only. Deliberately **no** auto-close for `[` (its pair lives on the *next* line) or `<` (unpaired jump-start) — see ambiguities.md §8. Typing `((` naturally yields `(())`. Word pattern: default, revisit if double-click selection misbehaves
- [ ] TextMate grammar `syntaxes/jefferson.tmLanguage.json`:
  - [x] Speaker labels — incl. optional leading line number and margin arrow (`→`/`->`) per grammar-spec.md §1
  - [x] Micropause `(.)`
  - [x] Timed pause `(0.7)` — integer and decimal variants
  - [ ] Laughter `(h)`, breathiness variants `(hh)`, `.hh` — `(h+)` done; bare `hh`/`.hh` in-breath still todo (they are breathing, not laughter — ambiguities.md §8.4)
  - [ ] Unclear speech `(   )` and best-guess `(word)` — `(   )` done; `(word)` and `(a/b)` alternative hearings still todo (fallback rules, add last)
  - [x] Analyst comments `((...))` — single-line, non-greedy; inner symbols correctly scope-switch to comment
  - [x] Overlap brackets `[` `]` (standalone tokens; pairs span lines by design)
  - [x] Intonation `↑` `↓`, analyst arrow `→`
  - [x] Latching `=`
  - [ ] Stretch colons `:::` (disambiguate from speaker colon)
  - [ ] Emphasis `_word_` — blocked on RESEARCH.md §5 decision (`_..._` vs U+0332)
  - [ ] Loud speech `CAPITALS` (2+ letters)
  - [ ] Quiet/whisper `°text°` / `°°text°°`, voice quality `£ # ~`, cut-off `-`, `¿` (tier 1 per grammar-spec.md §8, missing from original list)
- [ ] Grammar snapshot tests for every rule + adversarial cases (nested/adjacent symbols) — `tests/starter.jca` covers the implemented rules; extend as rules land
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
