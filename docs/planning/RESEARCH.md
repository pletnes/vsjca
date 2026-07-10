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

**Decision (partial), 2026-07-10:** Skipped `yo code` entirely and hand-rolled the
scaffold. Rationale: a language-only extension is six declarative files
(`package.json` contribution points, `language-configuration.json`, grammar JSON,
launch config, ignore files); the generator's value is its TypeScript/esbuild
template, which Phase 1 doesn't need (zero runtime code). Revisit the official
template when Phase 2 introduces `src/extension.ts`. `vscode-tmgrammar-test`
v0.1.x installed and works (its `vscode-tmgrammar-snap` binary is the snapshot
runner we use; both CLIs ship in the same package; it auto-discovers the grammar
from `package.json`). `engines.vscode` set to `^1.90.0` per TECH-STACK.md.

Still open:

- [ ] Confirm minimum `engines.vscode` to target (older versions = more uni machines covered; with no runtime code we could likely go much lower than 1.90)
- [ ] Read the "Syntax Highlight Guide" and "Language Extensions Overview" in the official VS Code docs — note anything that changed since training

## 4. Deployment & onboarding for non-programmers

**Question:** What is the absolute lowest-friction path from "I have Word open" to "highlighted transcript in VS Code"?

- [ ] Test the full journey on a clean machine (or VM): download VS Code → install extension → open file. Time it. Every minute counts.
- [ ] Investigate `contributes.walkthroughs` (VS Code's native onboarding UI) — can we teach the whole notation workflow in-editor?
- [ ] Locked-down university machines: can users install VS Code without admin rights? (User-scope installer exists on Windows — verify current state.) Can they install extensions offline via `.vsix`?
- [ ] VSCodium/Open VSX relevance for institutions that block Microsoft telemetry
- [ ] Draft the onboarding text: aim for one page, screenshots, zero jargon. Test on a real non-programmer.
- [ ] Where do CA researchers congregate? (Mailing lists, EMCA wiki, ROLSI journal community, university methods courses) — for both recruitment and announcement

## 5. Jefferson notation edge cases (grammar correctness)

**Findings recorded 2026-07-10** — full literature survey in `docs/jefferson/`
(bibliography, plain-language overview, technical grammar spec with Unicode code
points, ambiguities list).

- [x] Authoritative symbol list compiled — Jefferson (2004, pp. 13–31) is canon;
  Hepburn & Bolden (2013/2017) is the best systematic description. Full tiered
  list in `docs/jefferson/grammar-spec.md` (§8): tier 1 = Jefferson-2004/H&B core
  incl. `°`, `°°`, `.hh`/`hh`, `£`, `#`, `~`, `¿`, cut-off `-`; tier 2 = variants
  (`^`, `|`, `˘`, `*`, `//`, `[[`, `?,`, `!`, `⌈⌉⌊⌋`, U+0332 underlining).
- [x] Underlining in plain text: `_word_` is NOT attested in the literature or
  corpora — real practice is Word underline formatting or combining low line
  U+0332 (CLAN/TalkBank/CAfont). **Decision pending:** support `_..._` (typeable)
  and/or U+0332 (interoperable); see `docs/jefferson/ambiguities.md` §2.1.
  Note: underline is degree-bearing and combines with `:` for pitch contours.
- [x] Multi-line overlaps: alignment is purely columnar (monospace + manual space
  padding; padding spaces are semantically void per H&B fn. 10). No in-band
  anchor exists. DOTE ships a realignment heuristic; for us: recommend monospace
  font in docs (Phase 1), alignment helper stays in IDEAS.md.
- [ ] Collect real published transcript excerpts as grammar test fixtures —
  candidate sources listed in `docs/jefferson/bibliography.md` §4 (LISO PDFs,
  CABank/TalkBank licensed corpora, Hepburn & Bolden excerpts). Licensing note
  there: quote small excerpts with attribution; don't commit bulk corpora.
- [ ] NEW (from survey): decide handling of unpaired `<` (jump-start) and lazy
  unclosed `]` before implementing pace rules and Phase-2 auto-pairing —
  see `docs/jefferson/ambiguities.md` §3.
