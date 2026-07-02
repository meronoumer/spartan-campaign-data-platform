# Architecture v0

## System Overview

The Spartan Judicial data platform follows a lightweight campaign data lakehouse pattern.

The system moves data through five stages:

1. Ingestion
2. Bronze/raw storage
3. Silver/cleaned data
4. Gold/campaign-ready outputs
5. Dashboard and evaluation reports

## Architecture Flow

Messy public/campaign sources  
↓  
Python ingestion scripts  
↓  
Bronze data layer  
↓  
Schema validation  
↓  
Silver cleaned tables  
↓  
Data quality checks  
↓  
Gold candidate/campaign outputs  
↓  
Streamlit dashboard + evaluation report

## Bronze Layer

The bronze layer stores raw or lightly modified source data.

Examples:

- Raw candidate roster
- Raw OCA metrics
- Raw target campaign dossier
- TEC report exports
- SCJC check results
- State Bar profile records

## Silver Layer

The silver layer stores cleaned, standardized data.

Examples:

- Clean candidate table
- Clean court metrics table
- Clean finance report table
- Clean State Bar profile table
- Clean SCJC/source check table

## Gold Layer

The gold layer stores campaign-ready outputs.

Examples:

- Candidate dossiers
- Campaign readiness table
- Data quality summary
- Source verification summary
- Data readiness/risk signals

## Validation Layer

The validation layer checks whether the data is complete, correctly typed, and safe to use.

Example checks:

- Candidate name is not null
- Filer ID is preserved as text
- Long-pending percentage is between 0 and 1
- SCJC wording uses approved language
- Required source links are present
- Missing fields are explicitly labeled

## Evaluation Layer

The evaluation layer summarizes whether each campaign profile is ready, incomplete, or needs manual review.

Example risk signals:

- Missing source link
- Unverified campaign finance match
- Court metric caveat required
- SCJC check incomplete
- High long-pending court context
- Manual verification required

## Serving Layer

The first serving layer will likely be a Streamlit dashboard with:

1. Candidate dossier view
2. Campaign readiness view
3. Data quality report
4. Source lineage view
5. Risk signal view