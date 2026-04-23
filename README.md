# CiteGuard — Miscitation Reports Database

Public database of miscitation reports submitted via [CitéGuard](https://github.com/mink-veltman/citeguard). Each row is a report flagging a specific paper for misciting another work.

## Version history

Every submission or update to this database is a separate git commit. You can browse the full history at [github.com/mink-veltman/citeguard-data/commits](https://github.com/mink-veltman/citeguard-data/commits) to see exactly what was added or changed, and when.

## File

`miscitation_reports.csv` — UTF-8 encoded, comma-separated.

## Codebook

| Column | Type | Description |
|--------|------|-------------|
| `report_id` | string | Unique report identifier. Format: `YYYYMMDDHHMMSS_<10-char hash>`. |
| `submitted_at` | datetime | Timestamp of submission or last update (`YYYY-MM-DD HH:MM:SS`). |
| `reporter_role` | string | Reporter's relationship to the cited work. One of: `Author of the cited work`, `Co-author`, `Reader`, `Other`. |
| `reporter_orcid` | string | ORCID iD of the reporter, if provided (e.g. `0000-0001-2345-6789`). Empty if anonymous. |
| `own_work_doi` | string | DOI of the **cited** work — the paper being miscited. |
| `citing_work_doi` | string | DOI of the **citing** paper — the paper containing the miscitation. Empty if the report is a general warning not tied to a specific citing paper. |
| `target_author_last_names` | string | Last names of the authors of the cited work, semicolon-separated (derived from CrossRef metadata). |
| `target_lead_author` | string | Last name of the first author of the cited work. |
| `target_author_key` | string | Normalised author key used for matching (pipe-separated lowercased last names). |
| `target_publication_year` | string | Publication year of the cited work (derived from CrossRef metadata). |
| `target_title_clue` | string | First 80 characters of the cited work's title (derived from CrossRef metadata). |
| `target_title_key` | string | Normalised title key used for matching (lowercase significant words, pipe-separated). |
| `mistake_codes` | string | One or more miscitation type codes from the taxonomy below, semicolon-separated (e.g. `M1; M3`). |
| `mistake_titles` | string | Human-readable labels corresponding to `mistake_codes`, semicolon-separated. |
| `quoted_or_paraphrased_text` | string | The exact sentence(s) from the citing paper, or a paraphrase of the miscited claim, as provided by the reporter. |
| `why_incorrect` | string | The reporter's explanation of what is wrong and what the cited work actually shows. Multiple explanations from different reporters are separated by ` \|\| `. |

## Miscitation taxonomy

| Code | Type | Description |
|------|------|-------------|
| M1 | Direct contradiction of finding | The findings of the cited paper are directly contradicted in the citation. |
| M2 | Non-existent finding | Neither the cited finding nor the measures exist in the cited paper. |
| M3 | Non-significant finding | A non-significant finding is cited as though it were significant. |
| M4 | Overgeneralization of population | The cited finding is overgeneralized to a larger population without justification. |
| M5 | Misrepresentation of experimental conditions | The experimental conditions are misrepresented. |
| M6 | Causal relation distorted or confused | The causal claim is reversed or overstated. |
| M7 | Independent variable distortion | The cited independent variable is distorted. |
| M8 | Dependent variable distortion | The cited dependent variable is distorted. |
| M9 | Measure-to-construct inflation | A measure is inflated to a broader construct without justification. |
| M10 | Non-contextualized citation | Citation relevance is not apparent. |

## Notes on merging

If two reports target the same cited–citing DOI pair, they are merged into a single row: `mistake_codes` and `mistake_titles` are deduplicated and concatenated; `why_incorrect` and `quoted_or_paraphrased_text` are concatenated with ` || ` as a separator. The `submitted_at` field reflects the time of the most recent update.

## Anonymity and credibility

`reporter_orcid` is optional. Anonymous reports are accepted but flagged for extra scrutiny. Reports from verified authors of the cited work are given higher credibility.
