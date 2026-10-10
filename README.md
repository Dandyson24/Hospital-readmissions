<p align="center">
  <strong>HOSPITAL READMISSION RISK ANALYSIS</strong><br>
  Turning Hospital Encounter Data into Actionable Insights
</p>

# Hospital Readmission Risk Analysis

## Project Overview

Hospital readmissions can place additional pressure on clinical teams, consume limited bed capacity, increase care costs, and expose hospitals to financial penalties under applicable reimbursement programmes. Understanding which patient groups and care patterns are associated with readmission can help hospitals make better decisions about discharge planning and post-discharge support.

This project uses the **Diabetes 130-US Hospitals for Years 1999–2008** dataset to investigate readmission patterns and identify areas that may deserve closer clinical and operational review. Using SQL in MySQL Workbench, I examined patient encounters, diagnosis categories, prior hospital utilisation, discharge destinations, medication changes, and HbA1c testing.

The objective is to translate historical healthcare data into practical insights that can support resource prioritisation, care coordination, and quality-improvement discussions.

> **Important:** This is a retrospective analysis of a historical dataset. The findings describe associations in the analysed data; they do not establish causation or constitute a validated clinical prediction model.

## Business Problem

Hospitals need to understand why some patients return for inpatient care soon after discharge. Without a clear view of the patterns behind readmissions, teams may struggle to identify where additional discharge support, follow-up, or care coordination could be most useful.

This analysis addresses four practical questions:

- Which diagnosis categories show comparatively high readmission rates?
- Is previous inpatient utilisation associated with a greater likelihood of readmission?
- How do discharge destinations relate to readmission patterns?
- What can the data show about medication changes and HbA1c testing in relation to readmission?

The business goal is to help hospital leaders and care teams identify opportunities for targeted review—not to assume that every readmission is preventable.

## Dataset

- **Source:** Diabetes 130-US Hospitals for Years 1999–2008, available through the UCI Machine Learning Repository
- **Coverage:** Encounters from 130 U.S. hospitals over 1999–2008
- **Scale:** Approximately 100,000 patient encounters in the source dataset
- **Licence:** CC BY 4.0, as identified for the dataset
- **Analysis environment:** MySQL Workbench

The dataset contains information relating to patient demographics, admission and discharge details, diagnoses, medication use, prior utilisation, and diabetes-related testing.

## Tools and SQL Skills

- **MySQL Workbench** — querying and analysis
- **Common Table Expressions (CTEs)** — building reusable analytical cohorts
- **Window functions** — ranking and grouping patients for comparative analysis
- **`CASE` expressions** — translating values into interpretable risk categories
- **Correlated subqueries and `HAVING`** — identifying diagnosis groups with above-average readmission rates
- **Multi-table joins** — connecting reference tables to translate coded values into readable labels
- **Data cleaning and exploratory analysis** — preparing the data and establishing baseline comparisons

## Approach and Methodology

### 1. Data preparation

- Converted placeholder values representing missing information to `NULL` where appropriate.
- Removed a field with approximately 97% missing data from the analysis.
- Applied encounter-selection rules intended to reduce duplicate representation and bias.
- Excluded records for inactive patients or patients discharged to hospice where appropriate to the analysis scope.

**Interpretation note:** The source data contains multiple encounters for some patients. Any analysis that limits the data to one encounter per patient must define the selection rule clearly, because it can change the population being measured. Exclusions and deduplication should therefore be documented and checked against the question each query is intended to answer.

### 2. Exploratory analysis

Established baseline readmission rates and compared patterns across:

- Age groups
- Admission types
- Diagnosis categories
- Discharge destinations

This created a reference point for identifying groups with comparatively higher readmission rates.

### 3. Advanced SQL analysis

- Created a reusable high-risk analysis cohort using a CTE.
- Used window functions to rank patients by relevant utilisation or medication measures.
- Applied `NTILE(4)` to divide observations into quartiles for comparative analysis.
- Grouped encounters by medication use and number of diagnoses.
- Used correlated subqueries and `HAVING` conditions to identify diagnosis categories whose readmission rates exceeded the overall comparison rate.

These techniques made the analysis easier to organise, repeat, and extend.

### 4. Findings and interpretation

The analysis was distilled into four themes that connect the results to hospital operations and care planning.

## Key Findings

### 1. Circulatory and diabetes diagnoses warrant attention

Circulatory-related and diabetes diagnosis categories showed the highest readmission rates among the diagnosis groups examined, and both were above the overall average in this analysis.

**Business implication:** Hospitals could review discharge planning and follow-up arrangements for patients in these groups to determine whether additional support is needed. The observed pattern is a signal for further investigation, not proof that the diagnoses themselves caused readmission.

### 2. Previous inpatient utilisation is an important signal

Patients with more inpatient visits during the preceding year showed substantially higher readmission risk in the analysis.

**Business implication:** Prior utilisation may be a useful factor for identifying patients who could benefit from more coordinated discharge planning, timely follow-up, and closer review of care needs.

### 3. Discharge destination is associated with different outcomes

Readmission rates varied by discharge disposition. Patients discharged to some facility types had different observed rates from patients discharged home.

**Business implication:** Hospital teams should examine discharge pathways and transitions of care by destination. Comparisons should account for differences in patient condition and care needs before drawing conclusions about the performance of any destination.

### 4. Medication changes and HbA1c testing merit further review

The analysis examined medication changes at discharge and HbA1c testing during the hospital stay, two areas relevant to diabetes care and discharge processes.

**Business implication:** These measures can inform a deeper review of medication reconciliation, diabetes management, and continuity of care. Their relationship with readmission should be interpreted using the actual group-level results rather than assuming that a medication change or test automatically improves or worsens outcomes.

## Recommendations for Hospital Leaders

1. **Prioritise follow-up using multiple indicators.** Consider previous inpatient utilisation alongside diagnosis, clinical condition, discharge needs, and other locally validated factors.
2. **Review discharge pathways.** Investigate differences in readmission rates across discharge destinations while accounting for case mix and patient complexity.
3. **Strengthen transition-of-care processes.** Review medication reconciliation, patient education, appointment scheduling, and communication with community or post-acute care providers.
4. **Focus quality-improvement reviews on higher-rate diagnosis groups.** Work with clinical teams to identify practical gaps in care coordination and follow-up.
5. **Track outcomes over time.** Monitor readmission measures alongside relevant operational and quality indicators to assess whether interventions are associated with improvement.
6. **Validate before operational deployment.** Test whether the observed patterns persist in more recent, representative hospital data before using them to guide individual patient decisions.

## Key SQL Techniques Demonstrated

| Technique | How it was used | Business value |
|---|---|---|
| Common Table Expressions (CTEs) | Built a reusable analysis cohort | Improves readability and consistency |
| `RANK() OVER (PARTITION BY ...)` | Ranked records within relevant groups | Supports comparative prioritisation |
| `NTILE(4)` | Divided observations into four groups | Makes relative patterns easier to compare |
| `CASE` expressions | Created interpretable categories | Makes results easier for non-technical stakeholders to use |
| Correlated subqueries and `HAVING` | Flagged diagnosis categories above a comparison average | Highlights groups for further investigation |
| Multi-table joins | Mapped coded values to descriptive labels | Improves reporting clarity |

## Limitations and Responsible Interpretation

- The data covers **1999–2008** and may not reflect current clinical practice, technology, reimbursement rules, or patient populations.
- Findings are observational and should not be interpreted as evidence of causation.
- Readmission patterns may be influenced by severity of illness, comorbidities, access to care, social factors, and other variables not fully captured in the analysis.
- Excluding encounters or patient groups can affect readmission estimates. The cohort definition and denominator should be stated for each metric.
- A single encounter per patient is not necessarily appropriate for every readmission question. Readmission analysis generally requires a defined index encounter and a subsequent observation window.
- The findings are exploratory and do not represent a clinically validated patient-level risk score.
- No claim of financial savings or reduction in readmissions is made without an intervention and a suitable evaluation design.

## Project Deliverables

- SQL scripts for data preparation and analysis
- Exploratory readmission comparisons
- Reusable queries for patient grouping and ranking
- Diagnosis-category comparisons
- Business-focused findings and recommendations

## Conclusion

This project demonstrates how SQL can move healthcare analysis beyond simple counts to structured questions about utilisation, diagnosis, and discharge processes. By connecting readmission patterns to operational decisions, hospitals can identify where further investigation and quality-improvement efforts may be most valuable.

The central takeaway is that historical data can help teams ask better questions and direct attention where it may matter most—but responsible decisions require clinical context, current data, and ongoing evaluation.

## Repository Structure

```text
hospital-readmission-risk-analysis/
├── README.md
├── sql/
│   ├── data_cleaning.sql
│   ├── exploratory_analysis.sql
│   └── advanced_readmission_analysis.sql
└── results/
    └── charts-and-summary.md
```

*Adjust the file names and folder structure above to match the files actually included in this repository.*

## Author

**Healthcare Data Analytics Portfolio Project**

This project combines healthcare domain knowledge with SQL-based analysis to demonstrate how data can support operational insight, quality improvement, and evidence-informed decision-making.
