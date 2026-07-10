# RESEARCH.md — Open Questions

When a question is answered, record **Decision + rationale + date**, tick it in TODO.md, and prune the alternatives.

## 1. File extension convention

**Question:** What should Jefferson transcripts be called on disk?

Known landscape to verify before deciding:
- `.cha` — taken (CHAT/CLAN, CHILDES corpus format — a *different* transcription system; do not collide)
- `.eaf` — taken (ELAN annotation XML)
- `.TextGrid` — taken (Praat)
- `.trs` — taken (Transcriber XML)
- `.txt` — what most researchers actually use today; extension should ALSO activate on `.txt` opt-in or via a "Change Language Mode" hint
- Candidates: **`.jca`** (Jefferson CA), `.jef`, `.jt`, `.catx`

Research tasks:
- [ ] Check none of the candidates collide with anything registered/popular (search Marketplace + file-extension databases)
- [ ] Ask CA researchers what they'd find intuitive
- [ ] Decide whether to register a `firstLine` or filename-pattern fallback so `.txt` users get a gentle "this looks like Jefferson — switch mode?" prompt

**Decision:** _pending_

## 2. Code completion scope (Phase 2 design)

**Question:** What completions actually help a transcriber mid-flow?

Ideas to validate with users:
- [ ] Symbol palette: type a word, get the symbol (`up`→↑, `deg`→°, `pause`→`(0.0)` with cursor in number)
- [ ] Speaker label completion: learn labels used earlier in the file, complete on line start
- [ ] Auto-pairing `((` → `))`, `[` → `]` — helpful or annoying while transcribing? Test.
- [ ] "Insert timed pause" command bound to a keyboard shortcut (transcribers use these constantly)
- [ ] Investigate how transcribers currently work (foot pedal + audio software alongside editor?) — completions should not fight their rhythm

## 3. VS Code extension development environment

**Question:** Confirm current best-practice toolchain.

- [ ] Verify `yo code` template still current vs newer official templates; check what scaffold it emits in 2026
- [ ] Confirm minimum `engines.vscode` to target (older versions = more uni machines covered)
- [ ] Read the "Syntax Highlight Guide" and "Language Extensions Overview" in the official VS Code docs — note anything that changed since training
- [ ] Check `vscode-tmgrammar-test` is still maintained; alternative: `vscode-tmgrammar-snap`

## 4. Deployment & onboarding for non-programmers

**Question:** What is the absolute lowest-friction path from "I have Word open" to "highlighted transcript in VS Code"?

- [ ] Test the full journey on a clean machine (or VM): download VS Code → install extension → open file. Time it. Every minute counts.
- [ ] Investigate `contributes.walkthroughs` (VS Code's native onboarding UI) — can we teach the whole notation workflow in-editor?
- [ ] Locked-down university machines: can users install VS Code without admin rights? (User-scope installer exists on Windows — verify current state.) Can they install extensions offline via `.vsix`?
- [ ] VSCodium/Open VSX relevance for institutions that block Microsoft telemetry
- [ ] Draft the onboarding text: aim for one page, screenshots, zero jargon. Test on a real non-programmer.
- [ ] Where do CA researchers congregate? (Mailing lists, EMCA wiki, ROLSI journal community, university methods courses) — for both recruitment and announcement

## 5. Jefferson notation edge cases (grammar correctness)

- [ ] Compile the authoritative symbol list — Jefferson (2004) "Glossary of transcript symbols" is the canonical source; our table is a simplified subset. Decide which extended symbols to support: `°quiet°`, `.hh`/`hh` breathing, `£smiley voice£`, `#creaky#`, `*` variants
- [ ] How is underlining represented in *plain text* in practice? `_word_` is our assumption — validate with real corpora
- [ ] Multi-line overlaps: how do transcribers align `[` across lines? (Monospace font requirement → recommend font settings in docs)
- [ ] Collect real published transcript excerpts as grammar test fixtures (check licensing before committing them)
