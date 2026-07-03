# Spartan Judicial Campaign Data Lakehouse + Validation & Evaluation Platform

## Project Goal

Build a lightweight but production-minded data infrastructure prototype for Spartan Judicial that can collect, clean, validate, evaluate, and serve campaign data for judicial campaigns.

The prototype will focus on three priority campaigns:

- Joe Radler — 311th Family Court, challenger
- Paul Sullivan — 113th Civil Court, challenger
- Tami Pierce — 180th Criminal Court, incumbent

## Why This Matters

Spartan Judicial’s long-term value is not just collecting campaign data. The value is turning scattered public records and campaign information into a repeatable system that supports campaign strategy, voter education, fundraising, messaging, and coalition work.

This project creates the first version of that system.

## What the System Does

The system will:

1. Collect messy public and campaign data.
2. Store raw source files in a bronze layer.
3. Clean and standardize data into a silver layer.
4. Produce campaign-ready outputs in a gold layer.
5. Validate schemas and data quality.
6. Flag missing, stale, risky, or manually verified fields.
7. Serve outputs through candidate dossiers, readiness summaries, and dashboards.

## Prototype Scope

The first version will include:

- Candidate/campaign profile data
- Court-level OCA case/backlog metrics
- State Bar profile fields
- TEC campaign finance fields
- SCJC public sanctions check results
- Source links and confidence notes
- Data readiness/risk-signal evaluation
- A simple dashboard or profile view



## Final Deliverables

By early to mid-August, the project should include:

1. Working repo and project structure
2. Bronze/silver/gold data layers
3. Python ingestion and cleaning scripts
4. Pydantic schemas
5. Validation and data quality checks
6. Data readiness/risk-signal evaluation
7. Candidate dossier outputs for the three target campaigns
8. Streamlit or similar dashboard
9. Architecture documentation
10. Final evaluation report and handoff guide
