# **Spartan Judicial Prototype Feasibility Memo**

## **Project purpose**

Spartan Judicial is a civic data engineering prototype testing whether fragmented public records can be collected, normalized, and connected into candidate-level judicial accountability dossiers. The goal of this phase was not to build a final public-facing product or complete every candidate record. The goal was to test whether the core data model is feasible, which sources can scale, and which fields require manual verification before broader expansion.

The current prototype focuses on Harris County judicial candidates in the 2026 general election and combines candidate election data, court-level backlog metrics, professional profile information, campaign finance records, and public sanctions checks.

## **What I built**

I built three working data assets and one prototype dossier workflow.

First, I created a structured candidate seed file from the Harris County 2026 general election candidate list. The current working file contains 99 candidate rows with fields such as candidate name, party, election date, race stage, court name, court number, court type, opponent names, source URL, and source confidence.

Second, I created a candidate-to-court metrics dataset by filtering the Texas Office of Court Administration FY 2025 Age of Cases data to Harris County and joining court-level backlog metrics to candidate court seats. This dataset connects candidates to court-level metrics such as case category, active pending case totals, long-pending case counts, long-pending thresholds, and long-pending percentages where a reliable court match was available.

Third, I built a mini-batch dossier prototype. The first complete dossier row was built for Tanya Garrison. Additional mini-batch rows were used to test repeatability across Gerald Fowler, Ronald Schramm, Jason Cox, Silky Joshi-Malik, and LaShawn Williams. The mini-batch tested whether State Bar, TEC campaign finance, OCA court metrics, incumbent status, and SCJC public sanctions fields could be populated in a structured way.

The prototype shows that a judicial candidate dossier is feasible, but it also shows that not all fields scale the same way. Candidate identity and OCA court metrics are the strongest scalable layers. State Bar, TEC, incumbent status, and SCJC fields require more manual verification and clearer matching rules before scaling to all candidates.

## **Sources used**

The candidate seed list was built from Ballotpedia’s Harris County 2026 election information. This provided the initial candidate names, parties, election date, race stage, court seats, court numbers, court types, and opponent pairings.

The court backlog layer used the Texas Office of Court Administration FY 2025 Age of Cases dataset. This source provided court-level aggregate data, including active pending cases, disposed cases, and case-age buckets. The data was filtered to Harris County and normalized so that usable court-level metrics could be joined to candidate court seats.

The State Bar of Texas attorney profile was used for professional identity verification. For Tanya Garrison, this source provided her State Bar profile URL, bar card number, Texas license date, eligibility status, and public disciplinary history field.

The Texas Ethics Commission campaign finance search was used to test whether candidate finance data could be matched to JCOH filer records. Clear TEC records were found for Tanya Garrison and Gerald Fowler. No clear TEC filer match was found for Ronald Schramm, Jason Cox, Silky Joshi-Malik, or LaShawn Williams using candidate-name search.

The State Commission on Judicial Conduct public sanctions archive was used to test the public sanctions layer. The SCJC check was performed by searching checked fiscal-year public sanction pages by candidate name. Results were recorded using careful wording such as “No matching public sanction found in checked SCJC public sanctions pages,” rather than “no sanctions.”

## **What worked**

The candidate seed layer worked well. Once candidate names, parties, court seats, and opponent pairings were structured, the candidate file became a usable backbone for the rest of the project.

The OCA court-metrics layer also worked well. The court metrics are not candidate-performance metrics, but they provide useful court-seat context. This means the dataset can show the backlog conditions attached to the court seat a candidate is running for. The candidate-to-court metrics join is one of the strongest parts of the prototype because it can be applied across many candidates at once after court names are cleaned and matched.

The Tanya Garrison dossier proved that a full candidate dossier row can be populated from public sources. Her row combines candidate identity, court seat, court type, State Bar profile information, public disciplinary history status, TEC campaign finance report information, OCA court-level backlog metrics, and SCJC public sanctions check results.

The mini-batch test also worked as a feasibility stress test. It showed that the dossier structure can handle both complete and incomplete records. Instead of treating blanks as failures, the project can use status values such as “Not checked,” “Unknown,” “No clear match found,” and “Needs verification” to make missingness explicit and auditable.

## **What required manual verification**

Several layers required manual verification and should not be treated as fully automated yet.

Incumbent status required checking the current holder of the court seat and comparing that person to the candidate running for that seat. This is a candidate-level field, but it is determined from court-seat-level information. The interpretation matters because OCA backlog metrics mean different things for incumbents, challengers, and open-seat candidates.

State Bar lookup required manual identity matching. A State Bar profile can provide bar number, license date, eligibility status, and public disciplinary history, but name matching can become unreliable for common names or candidates with incomplete identifying information. The project should not automatically assume that a same-name State Bar profile is the correct candidate without verification.

TEC campaign finance matching was only partially successful. Clear filer records were found for Tanya Garrison and Gerald Fowler, but no clear TEC filer match was found for Ronald Schramm, Jason Cox, Silky Joshi-Malik, or LaShawn Williams using the current candidate-name search method. This means TEC finance cannot yet be treated as a clean automated join. The difficult part is not extracting numbers once a filer record is found; the difficult part is confirming that the filer record belongs to the correct candidate.

SCJC public sanctions checks also required manual archive searching and careful wording. A “no match found” result should not be interpreted as a complete guarantee of no sanctions. It only means that no matching public sanction was found in the checked SCJC public sanctions pages.

## **What should be automated next**

The next automation should focus on the safest and most repeatable layers first.

The candidate roster and OCA court metrics should remain the core scalable foundation. These layers already support broad candidate-level coverage and should be cleaned, versioned, and preserved as the base dataset.

The OCA matching process should be strengthened next. Court-name normalization, court-number matching, and match-confidence flags should be improved so the project can clearly distinguish between reliable court matches, uncertain matches, and no-match cases.

Incumbent status should be scaled through a court-seat lookup workflow rather than individual candidate guessing. The recommended method is to identify the current judge for each court seat, then compare that current judge to the candidates running for that same seat. This can populate incumbent, challenger, open seat, or unknown status more consistently.

TEC automation should be limited for now. The project should not attempt full automatic candidate-to-filer matching yet. A safer next step is to manually verify filer IDs first, then automate extraction of report metadata and latest report-period contribution and expenditure totals after the filer ID is confirmed.

SCJC checks can be made more systematic through a standardized checklist. The project should record fiscal years checked, search terms used, match status, and wording rules. This would make the sanctions layer more consistent without overclaiming.

## **What should not be overclaimed**

The OCA metrics should not be described as individual candidate performance metrics. They are court-level aggregate metrics. The safest public-facing wording is “court-level backlog context for the seat this candidate is running for.”

For challengers, OCA metrics should not be used to imply responsibility for current court backlog. A challenger may be running for a court seat with existing backlog conditions, but that does not mean the challenger caused or managed those conditions.

For incumbents, OCA metrics still require caution. Even if an incumbent currently holds a court seat, court-level backlog may reflect many factors beyond one judge, including filing volume, staffing, case type, administrative practices, and broader court-system conditions.

State Bar disciplinary history should not be described as a complete background check. The safer wording is “public disciplinary history listed on the State Bar profile at time of access.”

SCJC results should not be written as “no sanctions” unless the exact search scope is defined and verified. The safer wording is “No matching public sanction found in checked SCJC public sanctions pages.”

TEC finance totals should not be treated as lifetime totals or full-cycle totals unless all reports, corrections, and reporting periods are reconciled. For the prototype, the safer approach is to record latest report-period totals and clearly state that they are not lifetime or full-cycle totals.

## **Current scaling assessment**

The candidate roster is scalable. The current working file provides a structured base for 99 candidate rows and can support additional enrichment.

The OCA court metrics layer is mostly scalable. It provides useful court-level context for many candidates, but unmatched or uncertain rows should be clearly labeled instead of left blank.

Incumbent status is semi-scalable. The method is clear, but the source lookup still requires verification. It should be scaled by court seat, not by searching each candidate independently.

State Bar profile enrichment is feasible but manual or semi-manual. It should be tested on more candidates before automation because identity matching can be ambiguous.

TEC campaign finance is semi-scalable but not ready for full automation. The mini-batch found clear records for Tanya Garrison and Gerald Fowler, but not for several other candidates. Future automation should extract report details only after a filer ID has been manually verified.

SCJC public sanctions checks are feasible but manual/search-based. The archive check should be standardized with consistent years checked, search terms, and result language.

## **Recommended next phase**

The next phase should not be manual completion of all 99 candidate dossiers. That would be inefficient and could introduce unreliable claims. The next phase should be a controlled scaling pass that turns the prototype into a repeatable data workflow.

First, clean the current working files by replacing blank cells with explicit status values such as “Not checked,” “Unknown,” “No clear match found,” “No OCA match found,” or “Needs verification.” This will make the datasets easier to interpret and present.

Second, create a data dictionary that explains each file’s purpose. The candidate file should be described as the master candidate roster. The candidate-court metrics file should be described as court-level OCA context joined to candidate seats. The mini-batch dossier should be described as a prototype dossier test, not a completed full-candidate database.

Third, create a scaling plan table. The scaling plan should classify each layer as scalable, semi-scalable, or manual. It should also identify the risk in each layer and the next action needed before scaling.

Fourth, expand the mini-batch cautiously. A good next batch would include candidates across different court types and match difficulty levels. The purpose should be to test matching rules, not to produce a final public dataset yet.

The main conclusion is that Spartan Judicial is feasible as a civic data engineering project, but the responsible path is source-by-source scaling rather than candidate-by-candidate manual completion. The strongest current deliverables are the structured candidate roster, the candidate-to-court OCA metrics dataset, the mini-batch dossier prototype, and the documented scaling assessment.

