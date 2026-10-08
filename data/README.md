# Kotoba Atlas: JF survey and material evidence

Version 0.2.0 · 8 October 2026. Source: The Japan Foundation (JF), edited and structured by Kotoba Atlas. Independent project; not an official JF dataset.

This release links 1,299 material observations across 123 country/area profiles. 166 profiles were reviewed; consult `review_coverage.csv` for the remaining sources and exact review scope. Missing evidence does **not** mean no textbook is used. The review targets the 教材 subsection; some reviews also cover adjacent digital resources, as recorded in their scope. It is not an exhaustive bibliography of every title anywhere in every PDF.

## Tables

- `textbook_evidence.csv` / `.json`: one source-title / setting / page observation. Includes supplementary materials, historical use, publications and digital resources. Filter `evidence_type` and `material_type` before comparing adoption.
- `profile_reviews.json`: maintained source of textbook curation, including corrections and per-profile review scope. Rebuild with `python scripts/build-open-data.py` from the repository root.
- `titles.csv`: display identifiers and family labels. A title family is not an edition, translation or ISBN match.
- `review_coverage.csv`: every atlas area, including reviewed profiles without named titles, unavailable sources and areas without a profile link.
- `sources.csv`: JF source links, profile edition, retrieval/review status and PDF SHA-256 where available. Hashes identify the reviewed file; JF may update a URL in place.
- `survey_observations.csv`: long-form country/area counts for 2006, 2009, 2012, 2015, 2018, 2021 and 2024.
- `survey_waves.json`: original panel with source URLs, category definitions, totals and comparability notes. This release retains the previously validated panel rather than claiming a new cell-by-cell survey audit.
- `quantitative_evidence.json`: separate scoped response-share and usage observations, with original populations and denominators. Profile-level adoption observations also remain nested under `adoption` in the textbook table.
- `areas.csv` / `.json`: atlas area identifiers and 2024 baseline metadata. IDs are atlas keys, not a guaranteed ISO vocabulary; historical aliases are preserved.
- `datapackage.json`: file inventory, byte counts and SHA-256 checksums.

## Reading the evidence

`reported_use` means that the profile reports use in the described setting. `historical_use` explicitly concerns earlier or suspended activity. `mentioned` establishes a named mention/publication, not current classroom adoption. `curriculum_material` and `approved_category` establish curriculum or approval context. Additional source-specific distinctions are retained verbatim in the data dictionary and records. One record can describe several settings or periods: read `setting_detail` and `note` before making a setting-level claim.

The source title preserves the PDF wording, with layout whitespace normalized. Display labels may group a series, expand a clearly identified abbreviation, or normalize an English/Japanese family name. Distinct books, workbooks, translations and editions are not interchangeable. Grouped source variants are preserved in `source_title_variants`. Physical PDF page numbers are one-based. Profile edition 2025 is **not** an observation date and must never be used to backfill textbook use into 2006–2024.

`adoption.status = not_reported` has a null value, never 0%. A lower bound stays a bound. A local survey percentage stays local: for example, the Philippines' Irodori result refers to 54 responding institutions in a 2024–2025 local survey; Mongolia's Dekirumon observation refers to 15 of 30 school institutions in a 2020 survey. Neither is a national all-institution textbook market share. Counts of curated titles measure documentation, not adoption or readability.

## Historical comparisons

Missing cells are null (JSON) or empty (CSV), not zero. Explicit reported zeros remain zero. The 2006 primary/secondary categories are not artificially split. The 2009/2012 multiple-stage category remains distinct. Teacher counting changed across waves; read each wave's notes before interpreting growth. These are repeated institutional surveys, not a fixed panel. Present-day map geometry is not historical boundary evidence. Unimported questionnaire categories are outside this release.

## Readability and future contributions

No readability score, text-length measure or edition identification is inferred from a country profile. A future readability corpus needs separately licensed text and verified edition/ISBN links. To propose a correction, provide area ID, source URL, PDF page, exact title, education setting and whether the text establishes use, publication, historical use or a quantified observation. Keep a record's source wording when changing a display label. Validate with `node tests/textbooks.cjs` and the other repository checks.

See `DATA_DICTIONARY.md`, `RIGHTS.md` and `CHANGELOG.md`. Source PDFs and textbook covers are not redistributed in this data package.
