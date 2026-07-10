# Jefferson CA for VS Code

Syntax highlighting for [Jefferson notation](docs/jefferson/syntax-overview.md)
(Conversation Analysis transcription). Status: **pre-release, Phase 1 in
progress** — this README is developer-facing; the Marketplace page for
researchers comes with the first release.

## Develop

```bash
npm install        # dev tooling (no runtime deps — the extension is declarative)
npm test           # grammar snapshot tests
npm run test:update  # regenerate snapshots after an intentional grammar change
npm run package    # build the .vsix
```

Three ways to see the grammar working:

1. **F5 in VS Code** — launches an Extension Development Host with `samples/`
   open. After editing the grammar, run "Developer: Reload Window" in the dev
   host to pick up changes.
2. **Token inspector** — in the dev host, put the cursor on a symbol and run
   "Developer: Inspect Editor Tokens and Scopes" to see which grammar rule won.
3. **Terminal only** — `npm test` diffs every `tests/*.jca` fixture against its
   committed `.snap` file. No VS Code needed.

## Layout

- `syntaxes/jefferson.tmLanguage.json` — the TextMate grammar (the product)
- `language-configuration.json` — brackets, auto-closing pairs
- `tests/` — snapshot fixtures + committed snapshots
- `samples/` — realistic transcripts for eyeballing in the dev host
- `docs/jefferson/` — notation literature survey, grammar spec, ambiguities
- `docs/planning/` — project plan, TODO, research questions
