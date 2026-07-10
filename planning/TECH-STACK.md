# Tech Stack & Dependencies

## Core stack

| Layer | Choice | Why |
|---|---|---|
| Language | TypeScript (strict) | Standard for VS Code extensions; typed APIs |
| Runtime | Node.js LTS | Required by VS Code extension host & tooling |
| Highlighting | TextMate grammar (JSON) | Declarative, zero runtime code, works everywhere VS Code themes do |
| Language config | `language-configuration.json` | Brackets, auto-close pairs, word boundaries — also declarative |
| Bundler | esbuild | Fast, the current official template default |
| Scaffold | `yo code` (generator-code) | Official Yeoman generator; "New Language Support" template does 80% of Phase 1 |
| Tests (grammar) | `vscode-tmgrammar-test` | Snapshot-tests scopes against sample files; catches regex regressions |
| Tests (runtime) | `@vscode/test-electron` | Only needed once Phase 2 adds real code |
| Packaging/publishing | `@vscode/vsce` | Builds `.vsix`, publishes to Marketplace |
| Alt registry | `ovsx` | Publishes to Open VSX (VSCodium users — common on locked-down/uni machines) |
| Formatting | Prettier | Zero-config consistency |

## Dev dependencies (expected `package.json` slice)

```jsonc
{
  "devDependencies": {
    "@types/node": "latest LTS types",
    "@types/vscode": "match engines.vscode",
    "typescript": "^5.x",
    "esbuild": "^0.2x",
    "prettier": "^3.x",
    "vscode-tmgrammar-test": "^0.1.x",
    "@vscode/vsce": "^3.x",
    "ovsx": "^0.9.x"
  },
  "engines": { "vscode": "^1.90.0" }
}
```

Phase 1 has **no runtime dependencies at all** — the extension is pure contribution points (language + grammar). This keeps install size tiny and eliminates a whole class of bugs.

## Phase 2 additions

- No new deps for completions/snippets — built-in `vscode` API (`CompletionItemProvider`, `contributes.snippets`).
- Diagnostics: also plain `vscode` API. **Avoid LSP** unless we target a second editor; it roughly triples complexity.

## Phase 3 candidates (evaluate when reached)

| Need | Candidate | Notes |
|---|---|---|
| → Word export | pandoc (external binary) | Users must install it, or we bundle per-platform; UX research needed |
| .docx read/write in-process | `docx` (npm) / `mammoth` (read) | Avoids pandoc install burden; less powerful |
| Web highlighting | Monaco Monarch grammar | Paul has prior Monaco work to reuse |
| Markdown embedding | TextMate injection grammar | No dependency — grammar-level feature |

## Accounts / infrastructure needed

- **GitHub repo** (issues doubles as feedback channel)
- **Azure DevOps org + Personal Access Token** — required by vsce for Marketplace publishing (annoying but one-time)
- **Open VSX account** (Eclipse Foundation login) for ovsx publishing
- Optional later: GitHub Actions CI — run grammar tests + `vsce package` on every push, attach `.vsix` to releases

## Explicitly rejected (for now)

- **Language Server Protocol** — overkill for one editor target
- **Semantic highlighting (runtime tokenizer)** — TextMate is sufficient for Jefferson; revisit only if regex genuinely can't disambiguate something
- **Custom webview editor** — destroys the "it's just a text file" property that makes plain-text CA valuable
