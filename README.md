# Football Match Analytics Data Warehouse (SSIS, SSRS)

A production-style **Data Warehouse** for football match analytics built from **Transfermarkt** CSV datasets.  
The solution consolidates heterogeneous sources into a **star schema** (match-grain fact + multiple dimensions), loads and cleans data with **SSIS** (fuzzy matching, standardization, key resolution), and delivers **SSRS** reports focused on decision-ready KPIs and trends.

---

## Project Overview

- **Objective**  
  Provide a reliable analytical layer for football (soccer) insights by transforming fragmented CSVs into a consistent, query-optimized data model.

- **Analytical Grain**  
  One row per **match** in the fact table (FactGame).

- **Business Value**  
  Consistent KPIs (goals, cards, attendance, outcomes), longitudinal analysis across seasons and competitions, and comparative insights for clubs, managers, referees, and stadiums.

---

## Data Sources

Structured CSV exports derived from Transfermarkt covering matches, competitions, clubs, seasons, referees, managers, stadiums, and player appearances.  
These serve as the operational layer feeding the DWH via ETL.

---

## Star Schema

- **FactGame** (grain: match)  
  Core measures: goals (home/away, total), cards (yellow/red), attendance, outcomes (home win / away win / draw), clean sheets, both teams to score, total cards/goals.

- **Dimensions**  
  - **DimDate** – calendar attributes (day, month, quarter, year, season alignment)  
  - **DimCompetition** – competition metadata  
  - **DimSeason** – normalized season and round info  
  - **DimStadium** – stadium details and capacity  
  - **DimReferee** – referee identity and attributes  
  - **DimClub** – club dimension (used in home/away roles)  
  - **DimManager** – manager identity and tenure attributes  

Surrogate keys are used across dimensions for stable joins and auditability.

---

## ETL (SSIS)

Robust ETL packages handle end-to-end ingestion:

- **Cleansing & Standardization**  
  Data Conversion, Derived Columns, Sorts, null handling (“Unknown”), format normalization.

- **Entity Resolution**  
  **Fuzzy Grouping** and **Fuzzy Lookup** to align near-duplicate names (managers, stadiums, clubs); **Lookup** tasks for key mapping.

- **Business Logic**  
  Season and round normalization, computation of composite measures (e.g., `game_total_cards`), and caching of auxiliary inputs (e.g., appearances) for rollups.

- **Load Strategy**  
  Staging → transformed sets → star schema (dimensions first, then fact with FK resolution).

---

## Reporting (SSRS)

Curated, parameterized reports aimed at practical decision-making:

- **Competitions – Entertainment Index** (average goals, matches by season)  
- **Referee Statistics** (cards per match, “strictness” indicators)  
- **Stadium Attendance** (Top-N, trends by season)  
- **Team Performance** (home/away results, win rates, goal difference)  
- **Manager Performance** (W/D/L, points per match, goal difference)  
- **Fair-Play Ranking** (lowest card index per season)

Each report is designed for drill-down exploration and cross-season comparison.

---

## Technologies

- **DWH & ETL:** Microsoft SQL Server, **SSIS**  
- **Reporting:** **SSRS**  
- **Modeling:** Star schema (Fact + Dimensions), surrogate keys  
- **Data Quality:** Fuzzy matching, normalization, deduplication  
- **Tooling:** Git, CSV pipelines, documentation assets
  
---

Complete project documentation is available in Serbian:  
**[Projektna_dokumentacija-Vojin_Ćetković_IT21_2021.pdf (PDF)]

