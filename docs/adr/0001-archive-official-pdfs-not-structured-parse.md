# Archive official apps.ro PDFs as a raw, unparsed snapshot

We archive the official RA-APPS imobile lists from `apps.ro/liste-conform-hg/` as opaque
raw PDF files, with no parsing into a structured form.

Why: the PDFs are the legally-mandated ground truth (HG 420/2026) and RA-APPS deletes each
edition after six months — so the canonical bytes are the irreplaceable thing worth
preserving. We accept the cost: the archive is opaque (no querying or content-diffing)
until a future parsing layer is added. Stable filenames with overwrite-per-edition keep
the edition history in git itself.

Considered and rejected: parsing the PDFs into CSV/JSON now (brittle column extraction,
more than "minimal" needs). A parsing layer can be added later without losing any
archived edition.
