# Derive CSVs from the archived PDFs

We parse the three archived RA-APPS PDFs into three CSVs (`persoane-juridice.csv`,
`persoane-fizice.csv`, `neocupate.csv`) committed alongside them under `data/`. This
supersedes the "no parsing" deferral of [ADR 0001](./0001-archive-official-pdfs-not-structured-parse.md).

The PDFs remain the canonical ground truth; the CSVs are a **derived, regenerable**
layer. Consequences of that framing, which are the actual decision:

- **The parser never gates the archive.** PDF download/commit runs and succeeds on its
  own; parsing is a separate step that runs *after* it. A parser bug or a source layout
  change can never cost us an edition — the canonical PDF is already committed and the
  CSV can be regenerated from it later.
- **Fail loud, don't emit garbage.** Each list has a pinned header signature and cheap
  sanity checks (non-zero rows, every data row carries a `sucursala`). On any mismatch
  the parser exits non-zero and writes nothing, rather than committing a misaligned CSV.
- **Near-verbatim transcription, not normalization.** Values are copied as printed
  (Romanian decimal commas, original date strings, footnote text); only intra-cell
  newlines/whitespace are collapsed. Reinterpreting dates/numbers is a downstream concern
  that can be redone anytime from the committed CSV — keeping it out of this layer avoids
  silent corruption.
- **`nr_crt` is transcribed but is not a key** — it is a positional row number that
  renumbers per sucursala and per edition. Row identity, if ever needed, comes from
  content, not `nr_crt`.

Why now (vs. staying raw-only): the canonical bytes are preserved either way, so adding a
derived layer carries no risk to the archive, and it makes the data queryable/diffable
without waiting. The cost — parser maintenance against an inconsistent PDF layout — is
bounded by fail-loud: when the layout drifts, we find out immediately instead of silently.

Considered and rejected: parse-and-discard the PDF (makes the parser a single point of
failure for the whole archive); normalize values into the CSV now (lossy, bakes
interpretation into the archive layer); one unified CSV across the three lists (forces a
fuzzy discriminator and many empty columns — the three schemas genuinely differ).
