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
- If it cannot find exactly the three expected lists, the run **fails loudly** rather than
  silently archiving a partial edition.

## Scope

Raw PDFs only — no parsing into structured data (for now). The canonical bytes are the
point; a queryable layer can be added later without losing any archived edition.

---

_Inspiration: [termoficare-data](https://github.com/FlorinPopaCodes/termoficare-data) (the
git-scraping pattern) and the structured RA-APPS viewer at rapps.mirceaserdin.ro, which
first showed this data could be tracked._
