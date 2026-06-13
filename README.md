# ra-apps-data

A git-scraping archive of the official **RA-APPS** lists of state-owned properties
(*imobile*) in Romania.

RA-APPS publishes three "Transparență" PDFs twice a year (10 June / 10 December) under
[apps.ro/liste-conform-hg/](https://www.apps.ro/liste-conform-hg/), as mandated by
HG 420/2026. Each edition stays online only **six months** before it is overwritten.
This repo downloads those PDFs on a schedule and commits them, so the editions that would
otherwise disappear are preserved as a git history.

See [CONTEXT.md](./CONTEXT.md) for terminology and [docs/adr/](./docs/adr/) for decisions.

## How it works

- A weekly GitHub Action runs a small script that reads `apps.ro/liste-conform-hg/`,
  selects the three imobile lists (legal persons / natural persons / unoccupied), downloads
  them to stable paths under `data/`, and commits any changes.
- It then parses those PDFs into matching CSVs (`*.csv` next to each `*.pdf`) and commits
  them separately.
- If it cannot find exactly the three expected lists, or a PDF's layout drifts from what the
  parser expects, the run **fails loudly** rather than silently archiving a partial edition
  or emitting a misaligned CSV.

## Scope

Two layers under `data/`:

- **PDFs** — the canonical, legally-mandated bytes. Source of truth. Never parsed in place.
- **CSVs** — a derived, regenerable view of each PDF (near-verbatim: values copied as
  printed, only intra-cell whitespace collapsed; no date/number normalization). The PDF
  always wins; a CSV can be regenerated from it at any time. Parsing is decoupled from and
  runs after the archive, so a parser bug can never cost an edition.

See [docs/adr/0001](./docs/adr/0001-archive-official-pdfs-not-structured-parse.md) (why raw
PDFs) and [docs/adr/0002](./docs/adr/0002-derive-csvs-from-archived-pdfs.md) (why a derived
CSV layer).

---

_Inspiration: [termoficare-data](https://github.com/FlorinPopaCodes/termoficare-data) (the
git-scraping pattern) and the structured RA-APPS viewer at rapps.mirceaserdin.ro, which
first showed this data could be tracked._
