# Renal Transplant Graft Rejection: Risk Factor Analysis

> **Note:** `renal_transplant_mice_imputation.py` was originally written and run as a Google Colab notebook, then exported to a plain `.py` file for this repo. Because of that, some code may behave unexpectedly or throw an error if run outside Colab (e.g. Google Drive access, missing runtime state). For the full, working experience (including outputs and visualizations) please follow the notebook link below.
>
> 🔗 [Open the original Colab notebook](https://colab.research.google.com/drive/1gZbqxlJjXAbX4tBY1JgCAqFwS4NcxSth)

## What this is

A coursework assignment analyzing a cohort of 752 renal transplant patients to evaluate which risk factors (recipient age, delayed graft function, number of acute rejection treatments, six-week creatinine, blood pressure, panel reactive antibodies, proteinuria, and HLA cross-reactive groups) are associated with graft rejection risk, while accounting for the competing risk of patient death.

## What I did

- **Missing data imputation** — identified which variables had missing values (`dgf`, `creat`, `predias`, `uprotein`, `cregsh`), correctly typed each for imputation (numeric vs. categorical), and ran Multiple Imputation by Chained Equations (MICE) via `miceforest` (random-forest-based) to generate 5 complete versions of the dataset (`imputed_data_0.csv` – `imputed_data_4.csv`) using distinct random seeds, for downstream pooled analysis.
- **Full statistical analysis and results report** — (co-authored with Tyenca de Graf) `renal_transplant_graft_rejection_report.pdf` is the written results section covering:
  - A competing-risk check confirming no patients died with a functioning graft, justifying a standard Cox proportional hazards model over a full competing-risks approach for the primary analysis (with a Fine-Gray model run as a supplementary check).
  - Baseline characteristics stratified by outcome, and univariable Cox regression for each candidate risk factor.
  - Multivariable Cox regression with systematic assumption-checking: multicollinearity (VIF), linearity of continuous predictors (Martingale residuals plus a spline vs. linear likelihood ratio test), and the proportional hazards assumption (scaled Schoenfeld residuals).
  - A landmark Cox model with time-varying coefficients to address two predictors (age, high rejection-treatment count) whose effects were found to violate the proportional hazards assumption.
  - Model validation via concordance index (C-index) and Brier score at multiple follow-up time points, plus a calibration plot.

## Data

- `data/renaltx.csv` — raw cohort data (752 patients): graft-rejection status and follow-up time, patient death status and follow-up time, and the 8 candidate risk factors.
- `data/imputed_data_0.csv` – `imputed_data_4.csv` — 5 MICE-imputed complete versions of the dataset.

## Tools

Python · pandas · numpy · miceforest · pyreadstat

## Notes

This was a coursework exercise for a biostatistics module (MAM03), not a production project. It's shared here to demonstrate a full clinical time-to-event analysis: from handling missing data properly, through systematically checking every model assumption rather than assuming they hold, to validating the final model's real-world predictive performance rather than reporting hazard ratios in isolation.
