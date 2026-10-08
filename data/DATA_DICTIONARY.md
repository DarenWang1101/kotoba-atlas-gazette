# Data dictionary

UTF-8 CSV uses a header row, RFC 4180 quoting and empty cells for null. Nested arrays/objects are JSON-encoded in CSV cells. JSON files are UTF-8. Percent values use 0–100, never 0–1.

## Textbook evidence

| Field | Meaning |
|---|---|
| evidence_id | Deterministic hash of area, source title, setting, pages and evidence type; corrected identifying fields create a new ID |
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
