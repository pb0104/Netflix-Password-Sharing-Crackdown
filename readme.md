# Did Netflix's Password Sharing Crackdown Grow Its Subscriber Base?

**IDS 701 Final Project — Team 6**  
Pranshul Bhatnagar, Sejal Jagtap, Brynn Zhang, Xinhao Zhang  
May 2026



## Overview

This project applies causal inference methods to evaluate whether Netflix's 2022–2023 paid password-sharing crackdown led to meaningful subscriber growth. Netflix lost paid subscribers for the first time in over a decade in Q1 2022, with an estimated 100 million households accessing the platform through shared accounts. The crackdown required users outside a household to pay an additional fee or create their own account — a high-stakes policy with an theoretically ambiguous outcome.

We use five years of regional subscriber data and a comparison with Disney+ (which did not implement a similar policy) to isolate the effect of the crackdown from concurrent confounders such as the ad-supported tier launch, price increases, and post-COVID streaming normalization.



## Key Findings

- **Positive but inconclusive effect.** Every model estimated shows a positive effect on subscriber growth, but with wide uncertainty bands due to the limited number of regional data points.
- **Most reliable estimate: +0.30M subscribers/quarter** (Netflix-only DiD with region and time controls; p = 0.065).
- **No evidence of a mass subscriber exodus.** The lower bound of every confidence interval is small, ruling out the scenario Netflix feared most.
- **UCAN ARPU rose from $14.20 → $16.56** post-crackdown (+$2.36/user/month), suggesting meaningful full-price conversion in Netflix's most valuable market.
- **APAC diverges**: subscriber growth was positive, but ARPU fell by $1.55, driven by mobile-only plan expansion in India and Southeast Asia.



## Data Sources

| Source | Coverage | Variables |
|--------|----------|-----------|
| Netflix 10-Q / 8-K filings | Q1 2019 – Q1 2024 | Subscribers, revenue, ARPU by region (UCAN, EMEA, LATAM, APAC) |
| Disney+ regional reports | Q3 2022 – Q1 2024 | Subscribers by region (UCAN, EMEA, APAC) |

> **Note:** Disney+ does not report Latin America separately, so LATAM is excluded from the cross-platform DiD analysis.



## Methodology

### 1. Staggered Difference-in-Differences (Netflix Only)

Exploits variation in crackdown rollout timing across Netflix's four reporting regions:

- **LATAM:** Q3 2022 (first treated)
- **UCAN:** Q2 2023
- **EMEA & APAC:** Q3 2023

Region and quarter fixed effects control for time-invariant regional differences and common macro shocks. The parallel trends assumption is assessed via a pre-treatment event study.

### 2. Cross-Platform DiD (Netflix vs. Disney+)

Disney+ serves as an industry benchmark — any Netflix growth above and beyond Disney+'s trend during the same quarters is attributed to the crackdown. This removes streaming-industry-wide trends from the estimate.

### 3. ARPU Analysis

The same DiD framework is applied to Average Revenue Per User (ARPU) to assess monetization effects separately from subscriber volume.



## Regression Results

| Model | Coefficient | p-value | 95% CI |
|-------|------------|---------|--------|
| Main DiD (Netflix only) | +0.303M | 0.065* | [−0.02M, +0.63M] |
| Alt LATAM timing (Q4 2022) | +0.279M | 0.556 | [−0.31M, +0.87M] |
| Exclude LATAM | +0.773M | 0.897 | [−0.40M, +1.95M] |
| DiD + Platform Control (vs Disney+) | +0.780M | 0.693 | [−3.10M, +4.66M] |
| DiD + Platform Control + Policy Controls | +1.422M | 0.451 | [−2.27M, +5.12M] |
| ARPU: DiD + Platform Control | −$0.08 | 0.860 | [−$0.92, +$0.77] |

\* p < 0.10



## Limitations

- **Small sample (N = 4 regions):** Inference is underpowered even under ideal conditions. Confidence intervals are wide regardless of estimator.
- **Short post-treatment window:** UCAN, EMEA, and APAC each have only 3–4 post-treatment quarters, limiting detection of delayed or cumulative effects.
- **Concurrent confounders:** The ad-supported tier launch (Q4 2022) and price hikes (Q1 2023) overlap with the crackdown rollout.
- **Staggered treatment bias:** Two-way fixed effects estimators can produce biased estimates when treatment effects vary across cohorts; more robust estimators lack sufficient power here.
- **No Disney+ LATAM data:** Prevents inclusion of the earliest-treated region in the cross-platform analysis.



## Repo Structure

```
├── data/
│   ├── cleaned/                          # Cleaned intermediate data files
│   ├── disney_regional_panel.csv         # Disney+ regional subscriber panel
│   ├── disney_streaming_combined.csv     # Combined Disney+ streaming data
│   └── netflix_revenue_updated.csv       # Netflix revenue and ARPU by region
├── notebooks/
│   ├── 01_disney_eda.ipynb               # Exploratory analysis of Disney+ data
│   ├── 02_build_analysis_dataset.ipynb   # Panel construction and merging
│   └── 03_main_analysis.ipynb            # DiD regressions, event study, ARPU analysis
├── output/
│   ├── analysis_ready_panel.csv          # Final merged panel used for all models
│   └── staggered_did_event_study_ci.png  # Event study figure
├── report/
│   └── IDS701 Final Report Team 6.pdf
├── .gitignore
├── requirements.txt
└── README.md
```



## How to Reproduce

1. Clone the repository:
   ```bash
   git clone https://github.com/pb0104/Netflix-Password-Sharing-Crackdown.git
   cd Netflix-Password-Sharing-Crackdown
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Run notebooks in order (`01` → `05`) or execute the full pipeline:
   ```bash
   jupyter nbconvert --to notebook --execute notebooks/*.ipynb
   ```



## Target Audience

This analysis is written for the **VP of Product Strategy at Netflix** and addresses three strategic decisions:

1. Whether to maintain and tighten paid sharing enforcement in UCAN and EMEA.
2. Whether low-ARPU subscriber growth in APAC is the right long-run strategy.
3. Whether Netflix should invest in collecting country-level data for future policy evaluation.

