# Marketing A/B Testing: Conversion Lift & Ad-Timing Analysis

A reproducible Python portfolio project investigating whether commercial ads outperform a PSA control group, then testing weekday and hourly conversion patterns among users in the ad group.

> **Core principle:** a high rate is an observation; a statistically supported difference is an inference; a causal scheduling recommendation requires a credible experiment.

## Questions investigated

1. **Do ads outperform PSA?** Compare AD and PSA conversion proportions using a two-proportion Z-test, a Newcombe confidence interval, and Fisher's exact test as a robustness check.
2. **Does weekday matter?** Among AD users, test the association between conversion and the weekday on which each user saw the most ads. Follow up with all 21 weekday pairwise comparisons and Holm correction.
3. **Does hour matter?** Among AD users, test the association between conversion and the hour on which each user saw the most ads. Follow up with all 276 hourly pairwise comparisons and Holm correction.

## Key findings

| Analysis | Result | Statistical interpretation |
|---|---|---|
| AD vs. PSA | AD: **2.55%**; PSA: **1.79%** | Difference: **+0.77 percentage points**; 95% Newcombe CI: **+0.59 to +0.94 pp**; relative lift: **43.1%**; two-sided Z-test p = **1.71 × 10⁻¹³** |
| Weekday among AD users | Monday: **3.32%**; Tuesday: **3.04%** | Global chi-square p = **4.96 × 10⁻⁸⁶**; Monday vs. Tuesday remains significant after Holm correction across 21 pairs (adjusted p = **0.00937**) |
| Hour among AD users | 16:00: **3.09%**; 20:00: **3.03%** | Global chi-square p = **3.67 × 10⁻⁷¹**; the data does not establish that 16:00 outperforms 20:00 (Holm-adjusted p = **1.00** across 276 pairwise tests) |

The timing associations have small Cramér's V values (approximately **0.027**), so statistical significance should not be confused with large practical impact.

## What the project concludes—and does not conclude

- The AD group has a higher **observed** conversion rate than the PSA group in this extract.
- Weekday conversion rates differ; Monday leads the observed ranking and beats Tuesday after multiple-comparison correction.
- Hourly conversion rates differ; 16:00 leads descriptively, but the leading afternoon/evening hours are not cleanly separated statistically.
- The dataset's `most ads day` and `most ads hour` fields identify when a user saw the most ads. They are **not conversion timestamps** and do not prove that the advertiser deliberately scheduled exposure at those times.
- The extract does not document randomization, group-allocation planning, timezone, conversion attribution window, campaign cost, or revenue. Therefore, the analysis does not establish that ads or a particular timing window **caused** more conversions, nor whether the campaign was more profitable.

## Repository structure

```text
marketing-ab-testing/
├── marketing_ab_testing_analysis.ipynb
├── README.md
├── LINKEDIN_POST.md
├── requirements.txt
├── .gitignore
└── data/
    └── .gitkeep
```

## Run the notebook

### 1. Get the dataset

Download the CSV from [Kaggle — Marketing A/B Testing](https://www.kaggle.com/datasets/faviovaz/marketing-ab-testing/data).

Place it here:

```text
data/marketing_AB.csv
```

The dataset is intentionally not committed to this repository; retrieve it from the original source.

### 2. Create an environment and install dependencies

```bash
python -m venv .venv

# macOS / Linux
source .venv/bin/activate

# Windows PowerShell
.venv\Scripts\Activate.ps1

pip install -r requirements.txt
```

### 3. Launch Jupyter

```bash
jupyter lab
```

Open `marketing_ab_testing_analysis.ipynb` and run all cells from top to bottom.

## Methods

- **Primary group comparison:** two-sided two-proportion Z-test at α = 0.05.
- **Effect size:** absolute conversion-rate difference in percentage points, relative lift, and a 95% Newcombe confidence interval.
- **Robustness check:** two-sided Fisher exact test for the 2×2 conversion table.
- **Timing association:** Pearson chi-square tests of independence.
- **Post-hoc comparisons:** all pairwise two-proportion tests with Holm family-wise error correction (21 weekday pairs; 276 hour pairs).
- **Uncertainty in charts:** Wilson intervals for individual conversion proportions.

## Next experiment to run

Randomly assign comparable, eligible users to pre-defined advertising windows (for example, 16:00–18:00 and 20:00–21:00), keep creative, audience eligibility, bidding, frequency caps, and attribution window comparable, and pre-register the primary outcome and comparisons. Measure conversion alongside cost per conversion, revenue, and return on ad spend. Calculate sample size and power before launch.

## Tools

Python · pandas · NumPy · SciPy · Statsmodels · Matplotlib · Jupyter Notebook

## Source

Favio Vázquez, [Marketing A/B Testing dataset on Kaggle](https://www.kaggle.com/datasets/faviovaz/marketing-ab-testing/data).
