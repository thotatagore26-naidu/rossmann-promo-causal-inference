# Did the promotion actually cause a sales increase?
### A causal inference case study using Difference-in-Differences

## Overview
Businesses often assume that a sales increase after a promotion means the promotion worked — but
sales might have risen anyway due to seasonality or general demand trends. This project uses
**Difference-in-Differences (DiD)**, a standard causal inference method, to separate the true
causal effect of a promotion from background trends, using real retail data.

## Key finding
A naive before/after comparison suggested the promotion increased sales by ~227 units/week.
After properly accounting for the background trend using a control group, the true causal effect
was **not statistically significant** (DiD estimate ≈ −63, p = 0.84) — the naive comparison was
misleading. A placebo test (fake treatment date) confirms the method is not just picking up noise.

## Dataset
[Rossmann Store Sales](https://www.kaggle.com/competitions/rossmann-store-sales/data) — 1,115
European drugstores, ~1M daily sales records (2013–2015), including a `Promo2` field marking
which stores opted into a long-running promotional campaign and when.

- **Treatment group**: 32 stores that started `Promo2` in March 2014
- **Control group**: 544 stores that never adopted `Promo2`

## Method
1. Aggregate daily sales to weekly averages per group
2. Check the **parallel trends assumption** — confirm treatment and control moved together before
   the promotion started
3. Estimate the DiD regression: `Sales ~ Treatment + Post + Treatment×Post`
4. Compare against the naive (misleading) before/after estimate
5. Run a **placebo test** (fake earlier treatment date) as a robustness check

## Repo structure
```
├── README.md
├── analysis.ipynb        # full analysis, executed with outputs
├── data/                 # train.csv, store.csv (Rossmann Store Sales, Kaggle)
└── outputs/
    ├── parallel_trends.png
    └── weekly_agg.csv
```

## How to run
```bash
pip install pandas numpy matplotlib statsmodels jupyter
jupyter notebook analysis.ipynb
```

## Limitations / future work
- Result is specific to one adoption cohort (32 stores); testing other cohorts would strengthen
  or challenge the finding
- An event-study design (effect by week relative to adoption) would reveal any short-term effect
  that faded over time
- Store-level fixed effects and covariates (competition distance, store type) could tighten the
  estimate further

## Author
[Your name] — Data Science, Deggendorf Institute of Technology
