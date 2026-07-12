# Clinical Trial Safety Analysis

Survival analysis and safety reporting on a public clinical trial dataset using CDISC SDTM/ADaM standards.

## Overview

This project analyzes the **CDISC Pilot Study** (CDISCPILOT01), a publicly released, CDISC-standardized dataset built to mirror real clinical trial data structures. The (fictional) trial randomized 254 patients with mild-to-moderate Alzheimer's disease to Placebo, Xanomeline Low Dose, or Xanomeline High Dose (transdermal patch).

The analysis follows a real pharmaceutical statistical programming/biostatistics workflow: baseline demographics, adverse event summarization, and time-to-event survival analysis, using ADaM analysis-ready datasets derived from SDTM source data.

**Data source:** [CDISC/PHUSE SDTM-ADaM Pilot Project](https://github.com/cdisc-org/sdtm-adam-pilot-project) (public, synthetic data, no real patient information)

## Key Findings

- **Adverse events** occurred in 80.2% of Placebo patients, vs. 91.7% (Low Dose) and 94.0% (High Dose): a clear dose-response pattern.
- The AEs driving this were predominantly dermatologic (pruritus, application-site erythema, rash), consistent with a transdermal patch delivery mechanism, though not exclusively: Dizziness remained significant after formal testing, pointing to some systemic effect as well.
- **Time-to-first-dermatologic-event** analysis (Kaplan-Meier, log-rank p < 0.0001) showed both dose arms had a significantly higher hazard than Placebo: **HR = 4.15 (Low Dose)** and **HR = 5.03 (High Dose)**, 95% CIs entirely above 1.
- The proportional hazards assumption held (`cox.zph` p = 0.59), validating the Cox model.
- An age-adjusted sensitivity analysis showed hazard ratios were essentially unchanged (4.22 / 5.08), and age was not a significant predictor (p = 0.133): the dose-response signal is not confounded by age.

## Repo Structure

```
├── data/                  # SDTM/ADaM .xpt files
├── pipeline.Rmd           # Full exploratory analysis: every step, check, and sanity test
├── report.Rmd             # Report write-up: narrative and key results
└── README.md
```

- **`pipeline.Rmd`** is the working analysis: every data check, intermediate step, and model diagnostic, with all code visible. This is the "show your work" version.
- **`report.Rmd`** is the polished write-up: narrative interpretation alongside the key tables, plots, and models, with routine data-wrangling code hidden and analysis code visible.

## Methods

| Section | Approach |
|---|---|
| Baseline Demographics | Summary table (N, age, % female, % white) by treatment arm, from ADSL |
| Adverse Event Summary | Overall AE incidence by arm; top adverse events by MedDRA preferred term (ADAE) |
| Time-to-Event Analysis | Kaplan-Meier curves, log-rank test, Cox proportional hazards model, proportional hazards diagnostic, age-adjusted sensitivity analysis (ADTTE) |

## Tools

R · `haven` · `dplyr` / `tidyr` (data wrangling) · `survival` (Kaplan-Meier, Cox PH) · `survminer` (survival plots)

## Reproducing

1. Download the three ADaM datasets from the [CDISC pilot repo](https://github.com/cdisc-org/sdtm-adam-pilot-project) (`adsl.xpt`, `adae.xpt`, `adtte.xpt`) into a local `data/` folder.
2. Install required packages: `install.packages(c("haven", "dplyr", "tidyr", "survival", "survminer"))`
3. Knit either `.Rmd` file in RStudio.

## Note on data

All data used is public, synthetic, and released by CDISC specifically for training/demonstration purposes. No real patient data is involved.
