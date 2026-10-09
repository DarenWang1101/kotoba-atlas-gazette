# Data dictionary

UTF-8 CSV uses a header row, RFC 4180 quoting and empty cells for null. Nested arrays/objects are JSON-encoded in CSV cells. JSON files are UTF-8. Percent values use 0–100, never 0–1.

## Textbook evidence

| Field | Meaning |
|---|---|
| evidence_id | Deterministic hash of area, display title, source title, setting, pages and evidence type; corrected identifying fields create a new ID |
| area_id | Foreign key to areas; atlas identifier |
| title_id | Foreign key to titles; hash of display label, not a bibliographic authority ID |
| display_title / title_family | Editorial search/display labels; family matching does not establish edition equivalence |
| source_title | Named title as transcribed from source, layout whitespace normalized |
| source_title_variants | Additional explicitly named variants represented by a grouped record |
| education_setting / setting_detail | Stages described by the source; compound values deliberately not split into unsupported claims |
| evidence_type | Source claim: reported_use, historical_use, mentioned, curriculum_material, approved_category, or another explicit source-specific classification retained in profile_reviews |
| material_type | Textbook, category, named_material, digital_resource or source-specific type; named_material is intentionally not an inferred physical format |
| source_id / source_url | Source registry key and official JF URL |
| profile_edition | Edition year of the profile, distinct from observation year |
| pdf_pages | One-based physical PDF pages, not browser line numbers |
| section | Section or evidence locator |
| note | Original editorial paraphrase explaining context and limits |
| adoption | Object retaining status, value/lowerBound, unit, scope, numerator, denominator and observationYear where reported; missing fields are unknown, not zero |
| reviewed_on | ISO date of this curation pass |

## Survey observations

Primary key: `(survey_year, area_id, measure)`. `value` is a reported count or null. `missing` is true only for null. `institutions` counts institutions; other measures count people. `source_row` retains the original area label. `school` is a combined primary + secondary count where provided, not a substitute for native separate categories. `multipleStage` is retained only where reported. Category definitions and breaks live in `survey_waves.json`.

## Coverage and provenance

`reviewed_named_materials`: named material records from the stated scope. `reviewed_no_named_titles`: no named titles found in that reviewed scope; not a claim of no textbook use. `source_unavailable`: source could not be retrieved for this audit; not evidence that the official source is permanently unavailable. `no_profile_link`: no profile linked in the atlas source index. Sources include edition year, URL, retrieval date and SHA-256 where the reviewed PDF was available. No synthetic hash is generated for unavailable files.

## Population and derived ratios

Population record key: `(canonical_iso2, year)`. `area_id` preserves the original atlas key (DY/HV aliases); `canonical_iso2` is the join key for published survey tables. `population` is a count of residents or null. `population_basis`, `source_id`, `source_url`, `source_area_code`, `status` and `notes` preserve provenance and comparability. `year` is the survey-wave join key. `reference_year` (generated site `populationYear`) is the population reference year where specified, otherwise null. JF 2024 denominators generally have unspecified reference dates; only Taiwan explicitly supplies December 2024. `pdf_page` and `table` identify the exact JF record. `population_reference_year` in ratio CSV exports preserves that distinction. Population is not the surveyed learner population.

Ratio record key: `(area_id, survey_year, measure)`. `value = numerator / denominator × multiplier`; multiplier is 100000 for population ratios, 1 otherwise. Missing/zero denominator produces null, never Infinity or an artificial zero. `survey_source` identifies the numerator's survey table; `population_source` is set only for population-derived measures. `learnerTeacher`, `learnerInstitution` and `teacherInstitution` respectively divide learners by teachers, learners by institutions and teachers by institutions. `learnersPopulation`, `teachersPopulation` and `institutionsPopulation` divide the named count by population and multiply by 100000.

## Clustering outputs

`cluster_default.json`: `options` records group keys, encoding, algorithm, k and minimum document frequency; `cohort` holds the ordered input area IDs; `dataSha256` hashes the exact serialized enriched input rows used by the selection script. `assignments` gives area → cluster and medoid; group numbers have no inherent ordinal meaning. `metrics` stores silhouette, mean subsample stability, selection score and group sizes.

`cluster_evaluation.json`: one entry per tested configuration, including eligibility, silhouette, all five adjusted Rand indices, mean stability and score. Higher is preferred; these are internal exploratory criteria, not prediction accuracy.

`textbook_features.json`: each `vectors[].weights` array aligns exactly with `vocabulary`. Vocabulary entries hold normalized key, display label, document frequency and IDF. Zero weight means absent from that area's curated reported-use set; it does not prove no real-world use. A null vector means unavailable. Displayed cluster feature weights average over nonmissing vectors; area counts retain the full cluster denominator.

## Institution geography

`institution_geography.json` describes the 2024 public-directory snapshot. `source_record_count` counts unique exported 機関ID values. Each `areas[]` entry contains atlas `area_id`, name, original `source_country_labels`, country `directory_records`, `unlocated_records` and `subdivisions[]`. Each subdivision has the verbatim JF `label`, its count of unique directory records and `source_filters` containing the country/region filter IDs observed in the official search form. No institution or contact records are included. Taiwan's two source groups are combined by exact subdivision label; IDs were unique across both. Counts and shares describe this export, not survey coverage or all campuses. Subdivisions with no exported records are absent, not asserted zero.
