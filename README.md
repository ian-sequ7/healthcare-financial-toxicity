# Healthcare Financial Toxicity Prediction

PSTAT 231 Final Project — predicting catastrophic out-of-pocket healthcare burden on 20,962 MEPS 2022 respondents.

**Rendered write-up:** https://ian-sequ7.github.io/healthcare-financial-toxicity/final_project_resume.html

## Headline results

Best model (XGBoost) on held-out test set:

| Metric | Value | 95% CI |
|---|---|---|
| ROC-AUC | **0.874** | [0.849, 0.895] |
| PR-AUC | **0.331** | [0.264, 0.404] |
| Brier Skill Score | 0.163 | — |

## Methods

Six classifiers compared under 10-fold stratified cross-validation (2,020 individual model fits across 202 hyperparameter configurations):

- Logistic regression (baseline)
- Elastic net
- Random forest
- XGBoost
- RBF-SVM
- BART

Interpretability via odds ratios, impurity-based variable importance, and SHAP (`shapviz` + `kernelshap`). Fairness audited across racial/ethnic subgroups via `fairmodels` + DALEX.

## Data

Raw data: 2022 MEPS Full Year Consolidated file (HC-243), publicly available from AHRQ:

> Agency for Healthcare Research and Quality. (2024). *Medical Expenditure Panel Survey HC-243: 2022 Full Year Consolidated Data File*.
> https://meps.ahrq.gov/data_stats/download_data_files_detail.jsp?cboPufNumber=HC-243

Data is not bundled. Download `h243.dta` into `data/` to rebuild the analytic cohort.

## Reproduce

```bash
R -e 'renv::restore()'
quarto render final_project_resume.qmd
```

First render takes ~2 hours (2,020 fits); subsequent renders use cached .RData objects.

## Structure

- `final_project_resume.qmd` — Main write-up (source)
- `final_project_resume.html` — Rendered report (self-contained, 9MB)
- `codebook.md` — Variable definitions
- `h243cb.pdf` — Full MEPS HC-243 codebook
- `archive/` — Original class submission (Mar 2026)

## Author

Ian Sequeira — B.S. Statistics & Data Science, UC Santa Barbara (2027)
