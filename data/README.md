# Kotoba Atlas: JF survey and material evidence

Version 0.3.1 · 9 October 2026. Source: The Japan Foundation (JF), edited and structured by Kotoba Atlas. Independent project; not an official JF dataset.

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
- `areas.csv` / `.json`: atlas area identifiers and 2024 baseline metadata. Published table and site identifiers normalize legacy DY → BJ (Benin) and HV → BF (Burkina Faso). Original input JSON retains those aliases; generated rows retain sourceAreaId where changed.
- `datapackage.json`: file inventory, byte counts and SHA-256 checksums.

## Reading the evidence

`reported_use` means that the profile reports use in the described setting. `historical_use` explicitly concerns earlier or suspended activity. `mentioned` establishes a named mention/publication, not current classroom adoption. `curriculum_material` and `approved_category` establish curriculum or approval context. Additional source-specific distinctions are retained verbatim in the data dictionary and records. One record can describe several settings or periods: read `setting_detail` and `note` before making a setting-level claim.

The source title preserves the PDF wording, with layout whitespace normalized. Display labels may group a series, expand a clearly identified abbreviation, or normalize an English/Japanese family name. Distinct books, workbooks, translations and editions are not interchangeable. Grouped source variants are preserved in `source_title_variants`. Physical PDF page numbers are one-based. Profile edition 2025 is **not** an observation date and must never be used to backfill textbook use into 2006–2024.

`adoption.status = not_reported` has a null value, never 0%. A lower bound stays a bound. A local survey percentage stays local: for example, the Philippines' Irodori result refers to 54 responding institutions in a 2024–2025 local survey; Mongolia's Dekirumon observation refers to 15 of 30 school institutions in a 2020 survey. Neither is a national all-institution textbook market share. Counts of curated titles measure documentation, not adoption or readability.

## Historical comparisons

Missing cells are null (JSON) or empty (CSV), not zero. Explicit reported zeros remain zero. The 2006 primary/secondary categories are not artificially split. The 2009/2012 multiple-stage category remains distinct. Teacher counting changed across waves; read each wave's notes before interpreting growth. These are repeated institutional surveys, not a fixed panel. Present-day map geometry is not historical boundary evidence. Unimported questionnaire categories are outside this release.

## Ratios and population denominators

Six measures are available on the map and timeline: learners/teacher, learners/institution, teachers/institution, and each of the three counts per 100,000 residents. A ratio is null when its denominator is missing or zero. A reported zero numerator remains zero. Region/world ratios divide sums over matched valid areas, not averages of area ratios; their coverage accompanies the result. Teacher definitions differ across survey waves. These measures describe surveyed institutional education relative to staffing or the whole resident population, not classroom size or proficiency.

For the 2024 wave, `jf_population_2024.json` transcribes all 150 country/area rows in the 12 regional tables of JF's 2024 full report (physical PDF pp. 32, 37, 45, 49, 54, 58, 62, 66, 72, 77, 81 and 85). All 149 printed population figures replace the previous external denominators; Kosovo's printed dash is null. Each record retains the physical/printed page, table number, count cross-check and printed rounded learner rate. Zero-learner rows display a dash in the PDF's rate column; calculated site rates are zero when the denominator is known.

JF's footnotes cite the UN Population and Vital Statistics Report available in January 2025, and Taiwan's December 2024 Ministry of the Interior population. The other underlying reference dates are not specified in these tables and must not be presented as 2024 estimates. Examples: Taiwan 23,400,220; China 1,409,778,724; India 1,210,854,977. The aim is to reproduce JF's denominators, not replace them with more recent estimates.

The 54 areas absent from those regional tables retain the previous external population snapshot; all have zero reported 2024 learners. Earlier survey waves remain unchanged: Wikipedia annual population for Taiwan, World Bank SP.POP.TOTL elsewhere, with missing Cook Islands, Niue and Vatican City. Overall coverage is 1,406 of 1,428 area-year records (200 of 204 in 2024; 201 in earlier waves). No population is interpolated or backfilled. `year` remains the survey-wave join key; `reference_year` is null when the source's population date is unspecified. The timeline marks the 2024 denominator-source break and suppresses a directly comparable growth percentage.

- `population.json` / `population.csv`: all area-year denominators, including missing rows, geographical notes, basis and source identifiers. Join `canonical_iso2` to generated survey `area_id`; `area_id` in the population inputs preserves legacy atlas keys.
- `population_sources.json`: source URLs, attribution and retrieval metadata. Population estimates may be revised; the published snapshot is frozen.
- `ratio_observations.csv`: all six derived measures, original numerator/denominator, multiplier and source links for every survey row.

## Encoded textbook features and clustering

`textbook_features.json` stores the default cohort's full vocabulary, document frequencies, inverse-document frequencies and normalized vectors. A document is one country/area. Only `reported_use` evidence enters; digital resources, websites and curriculum standards are excluded. Deduplicate titles across settings. Family keys use NFKC normalization, lower case and whitespace removal; they do not establish edition or ISBN equivalence. Retain titles appearing in at least two cohort areas. Binary presence uses weight 1; TF–IDF uses binary TF × (log((1 + N)/(1 + document frequency)) + 1), where N is the number of areas with reported-use title evidence. Both are L2 normalized and compared by cosine distance. Missing or empty vectors are unavailable, not zero evidence of use.

Each selected group has equal weight in the mixed distance. Numeric counts and ratios use log(1+x), cohort range scaling and within-group means; stages use mean absolute differences in learner shares; region/script/review status use mismatch; textbook settings use Jaccard distance. Missing groups are omitted pairwise; incomparable pairs stop the run. Title features are a single group, so a large vocabulary cannot overwhelm numeric groups.

`cluster_evaluation.json` compares 84 configurations on the same 108-area complete analysis cohort: three count/ratio feature combinations, two encodings, average linkage or deterministic alternating k-medoids, and k=2…8. Eligibility requires each cluster ≥5% of the cohort (at least 3 areas) and the largest ≤75%. Selection maximizes the equal-weight mean of silhouette and stability (adjusted Rand index across five deterministic 80% subsamples); tie breaks prefer silhouette, then fewer clusters. All transformations are refit within each subsample.

`cluster_default.json` saves the winning settings, exact input hash, assignments, medoids and metrics: education counts + stages + TF–IDF, average linkage, k=2, clusters of 29/79, silhouette 0.197 and mean stability 0.833. This is the best eligible result under these declared criteria, not externally validated country types. Ratios remain selectable; missing profiles and editorial family matching affect results, and 2025 profiles are not a synchronous 2024 textbook census. Silhouettes across feature spaces are only an exploratory comparison. Contemporary textbook features are disabled on historical survey waves.

Reproduce from the repository root:

```sh
python scripts/build-open-data.py
node scripts/select-cluster-default.cjs
python scripts/build-open-data.py
node tests/ratios.cjs
```

## Readability and future contributions

No readability score, text-length measure or edition identification is inferred from a country profile. A future readability corpus needs separately licensed text and verified edition/ISBN links. To propose a correction, provide area ID, source URL, PDF page, exact title, education setting and whether the text establishes use, publication, historical use or a quantified observation. Keep a record's source wording when changing a display label. Validate with `node tests/textbooks.cjs` and the other repository checks.

See `DATA_DICTIONARY.md`, `RIGHTS.md` and `CHANGELOG.md`. Source PDFs and textbook covers are not redistributed in this data package.

## Institution directory geography (map only)

`institution_geography.json` aggregates JF's public CSV export retrieved 9 October 2026: 19,330 unique directory IDs, of which 17,094 have a state/city value. These produce 478 nonempty location groups in 21 countries/areas after combining Taiwan and Taiwan2 as instructed on JF's search form. Original Japanese location labels are retained. The field mixes provinces, states, cities, regions and Russian federal districts; it is not a consistent administrative level or boundary dataset.

The map's expandable province/state panel follows the selected country and shows record counts, within-country shares and source coverage. It is available only alongside 2024 counts, hidden for the textbook layer and historical waves. The directory may contain separate institutional departments and only publishes consenting respondents; these counts must not replace survey institution totals. Taiwan has 808 directory records against 809 reported institutions; Vietnam has 482 against 490. Missing province fields elsewhere do not mean no local education. The aggregation does not enter clustering, textbook evidence or trend charts.

The public export also exposes institutional names, addresses, education stages, establishment type, teacher-training and online-teaching information. This release uses only aggregated country and state/city fields, with no contact details or individual record text redistributed. Source and CSV SHA-256 are included. It does not infer local learner counts, teaching quality, adoption or map boundaries. Use JF's official directory for institution-level details.
