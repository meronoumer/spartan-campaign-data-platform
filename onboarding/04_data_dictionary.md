## **Overview**

This data package contains three working CSV files created during the Spartan Judicial feasibility prototype. Together, these files test whether public election, court, professional, campaign finance, and sanctions records can be structured into candidate-level judicial accountability dossiers.

These files are not a final public-facing product. They are feasibility-stage deliverables showing which data layers are already scalable, which layers require manual verification, and which fields should be treated cautiously.

The three CSV files are:

1. `candidates.csv`  
2. `candidate_court_metrics_fy2025.csv`  
3. `prototype_dossier.csv`

---

# **1\. `candidates.csv`**

## **Purpose**

`candidates.csv` is the master candidate seed list for Harris County judicial races.

One row represents one candidate running in the 2026 general election candidate list captured for this prototype.

This file is the base roster used to identify who is running, what party they are running under, which court seat they are running for, and who their listed opponent is.

## **Important caveat**

This file is a candidate roster, not a performance dataset. It should not be used to make claims about judicial performance, campaign finance, disciplinary history, or sanctions by itself.

## **Column dictionary**

| Column name | Meaning |
| ----- | ----- |
| `candidate_name` | Name of the judicial candidate. |
| `party` | Candidate party label as listed in the source, such as `D` or `R`. |
| `election_date` | Date of the election associated with the candidate row. In this prototype, general-election candidates use `2026-11-03`. |
| `race_stage` | Election stage, such as `General`. |
| `court_name` | Name of the court seat the candidate is running for. |
| `court_number` | Numeric court identifier where available, such as `157` for the 157th District Court or `1` for County Civil Court at Law No. 1\. |
| `court_type` | Normalized category of court, such as `District Court`, `County Civil Court at Law`, or `Probate Court`. |
| `opponent_names` | Listed opponent or opposing candidate for the same court race, where available. |
| `source_name` | Name of the source used to build the candidate row. In this prototype, this is primarily Ballotpedia. |
| `source_url` | URL for the source used to build the candidate row. |
| `date_accessed` | Date when the source was accessed or recorded. |
| `source_confidence` | Confidence rating for the candidate seed information from the source. |
| `incumbent_status` | Candidate’s relationship to the current court seat holder, if checked. Controlled values should include `Incumbent`, `Challenger`, `Open seat`, `Unknown`, or `Needs verification`. |
| `incumbent_source` | Source used to determine incumbent status, such as an official court page, State Bar profile, or other verification source. |
| `incumbent_confidence` | Confidence rating for incumbent status, such as `High`, `Medium`, or `Low`. |
| `notes` | Free-text notes about the candidate row, source limitations, manual checks, or unresolved issues. |

---

# **2\. `candidate_court_metrics_fy2025.csv`**

## **Purpose**

`candidate_court_metrics_fy2025.csv` connects candidates to court-level Texas Office of Court Administration age-of-cases metrics for FY 2025 where a court match was available.

One row represents one candidate joined to court-level FY 2025 OCA backlog context for the court seat they are running for.

This file is used to test whether candidate court seats can be connected to public court backlog data at scale.

## **Important caveat**

These are court-level metrics, not individual candidate performance metrics.

For incumbents, the data may describe the court they currently hold, but the metrics still reflect court-level conditions and should not be treated as direct proof of individual responsibility.

For challengers, the metrics describe the court seat they are running for, not the candidate’s personal record.

Safe wording:

“Court-level backlog context for the seat this candidate is running for.”

Avoid wording such as:

“Candidate backlog,” “candidate-caused delay,” or “candidate performance metric.”

## **Column dictionary**

| Column name | Meaning |
| ----- | ----- |
| `candidate_name` | Name of the judicial candidate. |
| `party` | Candidate party label as listed in the source. |
| `election_date` | Date of the election associated with the candidate row. |
| `race_stage` | Election stage, such as `General`. |
| `court_name_candidate` | Court name from the candidate roster. This is the court seat the candidate is running for. |
| `court_number` | Numeric court identifier where available. |
| `court_type` | Normalized court type from the candidate roster. |
| `opponent_names` | Listed opponent or opposing candidate for the same court race, where available. |
| `source_name` | Name of the candidate-list source. |
| `source_url` | URL for the candidate-list source. |
| `date_accessed` | Date when the candidate source was accessed or recorded. |
| `source_confidence` | Confidence rating for the candidate seed information. |
| `notes` | Notes from the candidate roster or merge process. |
| `court_name_clean` | Cleaned/normalized court name used for matching candidate court names to OCA court names. |
| `county_name` | County associated with the OCA row. For this prototype, this should generally be Harris County where matched. |
| `court_name_oca` | Court name as represented in the OCA dataset after filtering/matching. |
| `case_category` | Case category selected from the OCA data, such as `civil`, `family`, `felony`, `misdemeanor`, or `juvenile`. This reflects the usable case category for that court row. |
| `active_pending_total` | Total active pending cases for the selected case category at the end of the reporting period. |
| `long_pending_count` | Number of active pending cases older than the selected long-pending threshold. |
| `long_pending_threshold` | Age threshold used to define long-pending active cases, such as `over_18_months`. |
| `long_pending_pct` | Share of active pending cases that are long-pending. Calculated as `long_pending_count / active_pending_total` where available. |
| `disposed_total` | Total disposed cases for the selected case category, where available. |
| `long_disposed_count` | Number of disposed cases older than the selected long-disposed threshold, where available. |
| `long_disposed_threshold` | Age threshold used to define long-disposed cases, such as `over_18_months`. |
| `long_disposed_pct` | Share of disposed cases that were long-disposed. Calculated as `long_disposed_count / disposed_total`where available. |
| `metric_confidence` | Confidence or usability note for the OCA metric, such as `Usable court-level aggregate`, `No OCA match found`, or `Needs review`. |
| `incumbent_status` | Candidate’s relationship to the current court seat holder, if checked. This changes how the court-level OCA data should be interpreted. |
| `prototype_candidate_flag` | Indicates whether the candidate was included in the prototype or mini-batch dossier test. Suggested values: `Yes` or `No`. |

## **Data cleaning note**

The current working file may contain a column named `prototype_candidate_flag\n`. This appears to include an accidental newline character. It should be renamed to:

prototype\_candidate\_flag

---

# **3\. `prototype_dossier.csv`**

## **Purpose**

`prototype_dossier.csv` is the mini-batch candidate dossier prototype.

One row represents one candidate included in the prototype batch. The prototype batch was used to test whether a full candidate dossier could combine candidate identity, court seat, incumbent status, State Bar data, campaign finance data, OCA court metrics, and SCJC public sanctions checks.

This file is not intended to be a complete full-candidate database. It is a feasibility artifact showing what a candidate dossier row can look like and where public-source data becomes difficult or incomplete.

## **Important caveat**

Blank, `Unknown`, `Not checked`, or `No clear match found` fields represent missing or unverified public-source data. They should not be interpreted as negative findings.

For example:

* `No clear match found` in TEC data does **not** mean the candidate has no campaign finance activity.  
* `No matching public sanction found in checked SCJC public sanctions pages` does **not** mean “no sanctions ever.”  
* Empty State Bar fields may mean the lookup was not completed or the identity match was not verified.

## **Column dictionary**

| Column name | Meaning |
| ----- | ----- |
| `candidate_name` | Name of the candidate included in the prototype dossier batch. |
| `party` | Candidate party label. |
| `court_name` | Court seat the candidate is running for. |
| `court_type` | Normalized court category, such as `District Court`, `County Civil Court at Law`, or `Probate Court`. |
| `incumbent_status` | Candidate’s relationship to the current court seat holder. Suggested values: `Incumbent`, `Challenger`, `Open seat`, `Unknown`, or `Needs verification`. |
| `state_bar_profile_url` | URL for the candidate’s State Bar of Texas profile, if a profile was found and identity was verified. If not checked, use `Not checked`. If no clear match was found, use `No clear match found`. |
| `bar_number` | State Bar of Texas bar card number, where verified. |
| `bar_status` | State Bar eligibility or license status, such as `Eligible to Practice in Texas`, where verified. |
| `licensed_since` | Texas license date listed by the State Bar profile, where verified. |
| `public_discipline_flag` | Whether the State Bar profile lists public disciplinary history. Suggested values: `Yes`, `No`, `Unknown`, `Not checked`, or `Needs verification`. |
| `campaign_finance_source` | Source used for campaign finance lookup, such as `Texas Ethics Commission — JCOH campaign finance reports`. |
| `campaign_finance_url` | URL or source description for the campaign finance report or filer search used. |
| `filer_id` | TEC filer ID where a clear campaign finance filer match was found. If no clear match was found, use `Not found`. |
| `total_contributions` | Total political contributions from the selected TEC report period. These should not be treated as lifetime or full-cycle totals unless explicitly reconciled. |
| `total_expenditures` | Total expenditures from the selected TEC report period. These should not be treated as lifetime or full-cycle totals unless explicitly reconciled. |
| `oca_case_category` | OCA case category used for the candidate’s court seat, such as `civil`, `family`, `felony`, `misdemeanor`, or `juvenile`. |
| `active_pending_total` | Court-level active pending case total for the selected OCA case category. |
| `long_pending_count` | Court-level count of active pending cases older than the selected long-pending threshold. |
| `long_pending_threshold` | Threshold used to classify active pending cases as long-pending, such as `over_18_months`. |
| `long_pending_pct` | Share of active pending cases that are long-pending. Calculated as `long_pending_count / active_pending_total` where available. |
| `scjc_checked` | Whether the SCJC public sanctions archive was checked. Suggested values: `Yes`, `Partial`, `No`, or `Not checked`. |
| `scjc_result` | Result of the SCJC public sanctions archive check. Safe wording example: `No matching public sanction found in checked SCJC public sanctions pages`. |
| `overall_confidence` | Overall confidence rating for the dossier row, based on source completeness and verification. Suggested values: `High`, `Medium`, `Low`, or `Needs review`. |
| `missing_fields` | List of fields that remain missing, unverified, or not checked for that candidate. |
| `notes` | Free-text notes explaining source choices, limitations, report-period caveats, manual search results, or interpretation warnings. |

---

# **Recommended controlled values**

To keep the datasets clean, avoid leaving cells blank when possible. Use explicit status values.

## **General missingness values**

| Value | Meaning |
| ----- | ----- |
| `Not checked` | The source or field has not been checked yet. |
| `Unknown` | The answer is unknown based on currently available information. |
| `Needs verification` | A possible value exists, but it has not been confidently verified. |
| `No clear match found` | A source was searched, but no reliable match was found. |
| `Not applicable` | The field does not apply to this row. |

## **Incumbent status values**

| Value | Meaning |
| ----- | ----- |
| `Incumbent` | Candidate appears to currently hold the court seat they are running for. |
| `Challenger` | Candidate is running for a seat currently held by someone else. |
| `Open seat` | Current holder is not running or the seat appears open. |
| `Unknown` | Current holder or candidate relationship could not be determined. |
| `Needs verification` | Incumbent relationship needs further source confirmation. |

## **Campaign finance match values**

| Value | Meaning |
| ----- | ----- |
| `Matched` | A clear TEC filer/report match was found. |
| `No clear match found` | TEC was searched, but no reliable candidate-filer match was found. |
| `Multiple possible matches` | More than one possible TEC match exists and manual review is needed. |
| `Needs verification` | A possible filer match was found but is not yet confirmed. |
| `Not checked` | TEC lookup has not been performed. |

## **SCJC result wording**

Use careful source-scoped wording.

Preferred:

No matching public sanction found in checked SCJC public sanctions pages

Avoid:

No sanctions

The preferred wording is safer because it describes the search result without overclaiming that no sanction has ever existed.

---

# **File relationship summary**

| File | Role in project | Row meaning | Best use |
| ----- | ----- | ----- | ----- |
| `candidates.csv` | Master candidate roster | One row \= one candidate | Identify who is running and for which court seat. |
| `candidate_court_metrics_fy2025.csv` | Candidate-to-court OCA context | One row \= one candidate joined to court-level OCA metrics where available | Analyze court-seat backlog context, not individual candidate performance. |
| `prototype_dossier.csv` | Mini-batch dossier prototype | One row \= one prototype-batch candidate | Test whether full candidate dossiers can be populated from public sources. |


---

# 4. `target_3_campaign_dossier.csv`

## Purpose

`target_3_campaign_dossier.csv` is the campaign-focused dossier file for the three priority Spartan Judicial races identified by Chris:

- Joe Radler — 311th Family Court
- Paul Sullivan — 113th Civil Court
- Tami Pierce — 180th Criminal Court

One row represents one target campaign candidate.

This file is more polished and campaign-specific than the earlier `prototype_dossier.csv` because it focuses on the actual campaign types Spartan Judicial is supporting: two challengers and one incumbent across family, civil, and criminal court races.

## Important caveat

This file is still a prototype data product, not a final public-facing voter guide.

OCA metrics are court-level context for the court seat, not individual candidate performance metrics. For challengers, the court metrics describe the seat they are running for, not their personal record. For the incumbent, the court metrics may be more directly relevant, but they should still be described as court-level context rather than proof of individual responsibility.

TEC campaign finance values are latest report-period totals, not lifetime or full-cycle totals unless separately reconciled.

SCJC results should be read as source-scoped archive search results. The safe wording is:

`No matching public sanction found in checked SCJC public sanctions pages`

not:

`No sanctions`

## Column dictionary

| Column name | Meaning |
|---|---|
| `candidate_name` | Name of the target campaign candidate. |
| `party` | Candidate party label. |
| `court_name` | Court seat the candidate is running for. |
| `court_type` | Normalized court category, such as `Family District Court`, `District Civil Court`, or `Criminal District Court`. |
| `incumbent_status` | Candidate’s relationship to the current court seat holder. Suggested values: `Incumbent`, `Challenger`, `Open seat`, `Unknown`, or `Needs verification`. |
| `state_bar_profile_url` | URL for the candidate’s State Bar of Texas profile, if a profile was found and identity was verified. |
| `bar_number` | State Bar of Texas bar card number, where verified. |
| `bar_status` | State Bar eligibility or license status, such as `Eligible to Practice in Texas`. |
| `licensed_since` | Texas license date listed on the State Bar profile. |
| `public_discipline_flag` | Whether the State Bar profile lists public disciplinary history. Suggested values: `Yes`, `No`, `Unknown`, `Not checked`, or `Needs verification`. |
| `campaign_finance_source` | Source used for campaign finance lookup, such as `Texas Ethics Commission — JCOH campaign finance reports`. |
| `campaign_finance_url` | URL or source description for the campaign finance report or filer search used. |
| `filer_id` | TEC filer ID where a clear campaign finance filer match was found. This should be stored as text to preserve leading zeros. |
| `total_contributions` | Total political contributions from the selected TEC report period. This should not be treated as a lifetime or full-cycle total unless all reports are reconciled. |
| `total_expenditures` | Total expenditures from the selected TEC report period. This should not be treated as a lifetime or full-cycle total unless all reports are reconciled. |
| `oca_case_category` | OCA case category used for the candidate’s court seat, such as `family`, `civil`, or `felony`. |
| `active_pending_total` | Court-level active pending case total for the selected OCA case category. |
| `long_pending_count` | Court-level count of active pending cases older than the selected long-pending threshold. |
| `long_pending_threshold` | Threshold used to classify active pending cases as long-pending, such as `over_18_months` or `over_365_days`. |
| `long_pending_pct` | Share of active pending cases that are long-pending. Calculated as `long_pending_count / active_pending_total` where available. |
| `scjc_checked` | Whether the SCJC public sanctions archive was checked. Suggested values: `Yes`, `Partial`, `No`, or `Not checked`. |
| `scjc_result` | Result of the SCJC public sanctions archive check. Use careful wording that describes the checked source scope. |
| `overall_confidence` | Overall confidence rating for the dossier row, based on source completeness and verification. Suggested values: `High`, `Medium`, `Low`, or `Needs review`. |
| `missing_fields` | List of fields that remain missing, unverified, or not checked for that candidate. |
| `notes` | Free-text notes explaining source choices, limitations, report-period caveats, manual search results, or interpretation warnings. |

## Best use

Use this file as the first campaign-specific gold-output prototype. It shows what a polished candidate dossier row could look like once multiple public sources are connected into one structured record.

This file should be used for:

- testing candidate dossier structure,
- identifying source verification needs,
- generating dashboard-ready candidate profile views,
- and demonstrating how Spartan Judicial could support the three priority campaigns.

It should not be used to rank candidates, assign blame, or make final public claims without additional review.
---

# **Interpretation warning**

The datasets should be interpreted as feasibility-stage research outputs. They are designed to test data availability, matching reliability, and responsible wording. They should not be used to rank candidates, assign blame, or make final claims about judicial performance without additional verification and review.

