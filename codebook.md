# Variable Codebook: Healthcare Financial Toxicity Project

**Data Source**: 2022 Medical Expenditure Panel Survey (MEPS) Full Year Consolidated Data File (HC-243)

**Citation**: Agency for Healthcare Research and Quality. (2024). *Medical Expenditure Panel Survey HC-243: 2022 Full Year Consolidated Data File*. Rockville, MD: AHRQ. Retrieved from <https://meps.ahrq.gov/data_stats/download_data_files_detail.jsp?cboPufNumber=HC-243>

**Full MEPS Codebook**: See `h243cb.pdf` in the project repository for documentation of all 1,420 variables in the HC-243 file.

---

## Selected MEPS Variables (37 total)

### Identifiers & Survey Design

| Variable | Description | Type | Role |
|----------|-------------|------|------|
| DUPERSID | Person-level identifier | ID | Excluded from models |
| PERWT22F | Person-level survey weight | Weight | Used for weighted descriptives only |
| VARSTR | Variance estimation stratum | Design | Used for survey design specification |
| VARPSU | Variance estimation PSU | Design | Used for survey design specification |

### Demographics

| Variable | Description | Type | Role |
|----------|-------------|------|------|
| AGE22X | Age as of 12/31/2022 | Continuous | Predictor |
| SEX | Sex (1=Male, 2=Female) | Binary | Recoded to `sex` |
| RACETHX | Race/ethnicity (1=Hispanic, 2=NH White, 3=NH Black, 4=NH Asian, 5=NH Other/Multiple) | Categorical | Recoded to `race_eth` |
| EDUCYR | Years of education | Continuous | Predictor |
| REGION22 | Census region (1=Northeast, 2=Midwest, 3=South, 4=West) | Categorical | Recoded to `region` |

### Economic

| Variable | Description | Type | Role |
|----------|-------------|------|------|
| FAMINC22 | Total family income ($) | Continuous | Outcome component (denominator); excluded from models |
| POVCAT22 | Poverty category (1=Poor/Negative, 2=Near Poor, 3=Low Income, 4=Middle Income, 5=High Income) | Ordinal | Recoded to `poverty`; Predictor |

### Insurance

| Variable | Description | Type | Role |
|----------|-------------|------|------|
| INSCOV22 | Insurance coverage (1=Private, 2=Public Only, 3=Uninsured) | Categorical | Recoded to `insurance`; Predictor |

### Health Status

| Variable | Description | Type | Role |
|----------|-------------|------|------|
| RTHLTH53 | Perceived physical health status (1=Excellent to 5=Poor) | Ordinal | Recoded to `health_phys`; Predictor |
| MNHLTH53 | Perceived mental health status (1=Excellent to 5=Poor) | Ordinal | Recoded to `health_ment`; Predictor |

### Chronic Conditions

| Variable | Description | Type | Role |
|----------|-------------|------|------|
| DIABDX_M18 | Ever diagnosed with diabetes (1=Yes, 2=No) | Binary | Recoded to `diabetes`; Predictor |
| HIBPDX | Ever diagnosed with high blood pressure (1=Yes, 2=No) | Binary | Recoded to `highbp`; Predictor |
| ASTHDX | Ever diagnosed with asthma (1=Yes, 2=No) | Binary | Recoded to `asthma`; Predictor |
| ARTHDX | Ever diagnosed with arthritis (1=Yes, 2=No) | Binary | Recoded to `arthritis`; Predictor |
| CANCERDX | Ever diagnosed with cancer (1=Yes, 2=No) | Binary | Recoded to `cancer`; Predictor |
| STRKDX | Ever diagnosed with stroke (1=Yes, 2=No) | Binary | Recoded to `stroke`; Predictor |

### Access to Care

| Variable | Description | Type | Role |
|----------|-------------|------|------|
| DLAYCA42 | Delayed medical care due to cost (1=Yes, 2=No) | Binary | Recoded to `delayed_care`; Predictor |
| AFRDCA42 | Unable to get necessary care due to cost (1=Yes, 2=No) | Binary | Recoded to `forgone_care`; Predictor |

### Healthcare Utilization

| Variable | Description | Type | Role |
|----------|-------------|------|------|
| OBTOTV22 | Total office-based visits | Count | Sensitivity analysis only (leakage risk) |
| ERTOT22 | Total ER visits | Count | Sensitivity analysis only (leakage risk) |
| IPDIS22 | Total inpatient discharges | Count | Sensitivity analysis only (leakage risk) |
| HHTOTD22 | Total home health days | Count | Sensitivity analysis only (leakage risk) |

### Expenditures

| Variable | Description | Type | Role |
|----------|-------------|------|------|
| TOTEXP22 | Total healthcare expenditures ($) | Continuous | Descriptive only |
| TOTSLF22 | Total out-of-pocket expenditures ($) | Continuous | Outcome component (numerator); excluded from models |
| TOTMCR22 | Total Medicare payments ($) | Continuous | Descriptive only |
| TOTMCD22 | Total Medicaid payments ($) | Continuous | Descriptive only |
| TOTPRV22 | Total private insurance payments ($) | Continuous | Descriptive only |
| TOTVA22 | Total VA payments ($) | Continuous | Descriptive only |
| TOTTRI22 | Total TRICARE payments ($) | Continuous | Descriptive only |
| TOTOFD22 | Total other federal payments ($) | Continuous | Descriptive only |
| TOTWCP22 | Total workers' compensation payments ($) | Continuous | Descriptive only |
| TOTOSR22 | Total other state/local payments ($) | Continuous | Descriptive only |
| TOTOTH22 | Total other payments ($) | Continuous | Descriptive only |

---

## Derived Variables

| Variable | Description | Type | Role | Derivation |
|----------|-------------|------|------|------------|
| burden_ratio | OOP-to-income ratio | Continuous | Intermediate | TOTSLF22 / FAMINC22 |
| high_burden | Financial burden indicator | Binary (No/Yes) | Outcome | 1 if burden_ratio > 0.10 |
| n_chronic | Count of chronic conditions (0--6); excluded from models due to multicollinearity with individual condition dummies | Count | Descriptive | Sum of DIABDX_M18, HIBPDX, ASTHDX, ARTHDX, CANCERDX, STRKDX (coded 1=Yes) |
| age_group | Binned age (6 levels) | Categorical | Descriptive | cut(AGE22X, breaks at 18, 25, 35, 45, 55, 65, Inf) |

---

## Missing Data Conventions

MEPS uses negative sentinel codes rather than standard NA values:
- **-1**: Inapplicable (e.g., condition questions not asked of children)
- **-7**: Refused
- **-8**: Don't know
- **-9**: Not ascertained

All negative sentinel codes are recoded to `NA` during data preparation. See `eda.qmd` for the recoding procedure.

## Cohort Restrictions

1. Positive survey weight (PERWT22F > 0)
2. Positive family income (FAMINC22 > 0)
3. Adults aged 18+ (AGE22X >= 18)

Final analytic cohort: **20,962 adults**
