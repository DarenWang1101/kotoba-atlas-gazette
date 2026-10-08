# The Kotoba Gazette

An interactive newspaper-style atlas of Japanese-language education, with structured evidence from The Japan Foundation (JF).

**[Open the live atlas](https://darenwang1101.github.io/kotoba-atlas-gazette/)** · **[Browse the open data](data/README.md)**

## Evidence release 0.2.0 — 8 October 2026

- 1,296 page-linked named-material observations across 123 country/area profiles; 166 profiles reviewed within documented scope.
- Taiwan's six titles restored with PDF p. 7 citations and separate labels for reported use versus publication mentions.
- Exact source-title wording, source pages, setting context, source hashes and review coverage retained.
- Seven survey waves (2006–2024) published as structured data; profile edition year is kept separate from survey/observation year.
- CSV, JSON, a data dictionary, source registry, checksums and rights documentation in [`data/`](data/).

Coverage is still partial where a source could not be retrieved. A reviewed section without named titles does not imply no textbook use. Historical/publication mentions are not current adoption; local percentages are not national market shares. No readability scores or exact-edition matches are inferred.

## Develop and validate

The deployable source is also provided in `gazette-source.zip`. Extract it to obtain `dist/`, `data/`, `scripts/` and `tests/`.

```sh
unzip -oq gazette-source.zip 'dist/*' 'scripts/*' 'tests/*'
python scripts/build-open-data.py
node --check dist/app.js
node tests/history.cjs
node tests/textbooks.cjs
node tests/cluster-core.cjs
node tests/ui-smoke.cjs
```

Canonical textbook curation lives in `data/profile_reviews.json`; `scripts/build-open-data.py` regenerates the evidence tables and site data together. The direct `data/` files are authoritative. The GitHub Pages workflow extracts application code from the archive, rebuilds the website data from the repository tables, runs the checks and deploys `dist/` through `scripts/prepare-pages.cjs`.

## Attribution

Source: The Japan Foundation / 出典：国際交流基金（加工して作成）. Independent project, not affiliated with JF or any newspaper publisher. Original curation/annotations are CC BY 4.0; source PDFs, textbook text and covers are not relicensed or included in the open-data package. See [rights and attribution](data/RIGHTS.md).
