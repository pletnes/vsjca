# Jefferson notation — research documentation

Findings from a literature survey of Jefferson (CA) transcription conventions,
compiled 2026-07-10 to ground the grammar work in this repo.

| File | Contents | Audience |
|---|---|---|
| [bibliography.md](bibliography.md) | Primary and secondary literature, sample-transcript sources, with descriptions | everyone |
| [syntax-overview.md](syntax-overview.md) | Plain-language description of the notation | non-programmers, end users |
| [grammar-spec.md](grammar-spec.md) | Explicit rules, every symbol down to Unicode code points, disambiguation order | grammar implementers |
| [ambiguities.md](ambiguities.md) | Unclear/ambiguous/contested rules and the published discourse about them | grammar implementers |

Key takeaways for this project:

1. The canonical source is Jefferson (2004), "Glossary of transcript symbols with an
   introduction" — but it is a *glossary with examples*, not a formal spec. No formal
   spec exists anywhere; every tool (CLAN, DOTE, GailBot) formalizes its own subset.
2. The notation was designed for typewriters and Word, not plain text. Underlining,
   overlap column-alignment, and superscript dots do not survive plain-text encoding
   without per-tool conventions (see ambiguities.md §2–3).
3. Symbol variants abound between "schools" (e.g. `↑` vs `^`, `#` vs `*` for creaky
   voice). The grammar should treat the Jefferson-2004/Hepburn-Bolden core as tier 1
   and variants as tier 2.
