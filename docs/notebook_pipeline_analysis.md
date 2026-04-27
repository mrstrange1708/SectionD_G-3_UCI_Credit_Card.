# Credit Shield: Identifying and Mitigating Loan Default Risk
## Notebook Pipeline Analysis & Documentation
 
**Project:** Credit Shield — Default Risk Intelligence  
**Dataset:** UCI Default of Credit Card Clients (Taiwan, April–September 2005)  
**Team:** Section D, Group 3 | Newton School of Technology | DVA Capstone 2  
**Problem Statement:** Financial institutions face increasing loan defaults, leading to revenue loss and higher risk exposure. This project analyses customer demographics, transaction behavior, and credit patterns to identify key factors driving loan defaults, segment high-risk customers, and recommend strategies to reduce default rates and improve risk management.
 
---
 
## Overview
 
The project pipeline is divided into five sequential Jupyter Notebooks that process a 30,000-record credit card dataset end-to-end — from raw extraction through statistical validation to Tableau-ready export. Every notebook is designed to be reproducible, documented, and directly traceable to the final dashboard and business recommendations.
 
---
 
## 1. Notebook-by-Notebook Analysis
 
---
 
### `01_extraction.ipynb` — Data Intake & Baseline Profiling
 
**What is happening:**  
The raw dataset (`UCI_Credit_Card_raw.csv`) is loaded into a Pandas DataFrame without any modification. The notebook establishes a complete baseline profile of the data before any cleaning occurs.
 
**Steps performed:**
- Loaded dataset and confirmed shape: **30,000 rows × 25 columns**
- Printed all column names and data types — verified no unexpected object-type numerical columns
- Ran `df.head()` and `df.tail()` to visually inspect data integrity end-to-end
- Ran `df.describe()` to capture statistical summary — identified right-skew in LIMIT_BAL and BILL_AMT columns
- Ran `df.isnull().sum()` — confirmed **zero explicit null values** in the raw file
- Ran `df.duplicated().sum()` — confirmed no duplicate rows
- Ran `value_counts()` on `EDUCATION`, `MARRIAGE`, `SEX`, and `default.payment.next.month`
**Key finding at extraction stage:**  
While the dataset contains zero explicit nulls, value_counts revealed undocumented categorical encodings:
- `EDUCATION`: Values 0, 5, 6 are not defined in the UCI data dictionary (Yeh & Lien, 2009)
- `MARRIAGE`: Value 0 is not defined in the original documentation
These were flagged as **implicit missing values** — to be treated in notebook 02.
 
**Class imbalance documented:**  
- Non-default (0): ~77.88% (23,364 customers)  
- Default (1): ~22.12% (6,636 customers)  
This imbalance is documented as a data characteristic and acknowledged in the project limitations.
**Correctness:** Solid. The extraction notebook correctly establishes a read-only baseline. No transformations are applied here — consistent with the principle that `data/raw/` files are never edited.
 
---
 
### `02_cleaning.ipynb` — ETL Pipeline & Feature Engineering
 
**What is happening:**  
All identified data quality issues are systematically resolved. Every transformation is logged with a written justification. The cleaned, enriched dataset is exported to `data/processed/`.
 
**Transformations applied (in order):**
 
**1. Drop ID column**  
The `ID` column is a row identifier with no analytical value. Retaining it would introduce false correlations in any downstream statistical test.
 
**2. Rename target column**  
`default.payment.next.month` → `default_status`  
Dots in column names break Python's attribute access syntax and cause silent errors in downstream notebooks.
 
**3. EDUCATION — Implicit missing value treatment**  
Values 0, 5, 6 are undocumented in the original UCI data dictionary. These are not "Others" — they are genuinely unknown. Converting them to NaN and imputing with mode is the academically honest approach.  
- Values 0, 5, 6 → converted to `NaN`  
- Approximately 350 rows affected  
- Imputed using **mode (value = 2, University)** — correct method for nominal categorical variables where mean and median are mathematically meaningless
**4. MARRIAGE — Implicit missing value treatment**  
Value 0 is undocumented. Same rationale as EDUCATION.  
- Value 0 → converted to `NaN`  
- Approximately 55 rows affected  
- Imputed using **mode (value = 2, Single)**
**5. PAY_0 to PAY_6 — Recode value -2**  
Value -2 is undocumented but academically interpreted as "no consumption that month." Converting to NaN would destroy 6 months of behavioral history. Recoded to -1 (pay duly) as the most conservative and defensible interpretation.
 
**6. Outlier Audit — IQR Method (No rows removed)**  
Extreme values were identified in `LIMIT_BAL`, `BILL_AMT1`, and `PAY_AMT1` using the IQR method. A deliberate decision was made to **retain all outliers** for the following reasons:
- Extreme credit limits represent genuine high-net-worth customer profiles — not data errors
- High bill amounts are analytically significant for default risk — removing them would hide the most financially stressed customers
- Removing outliers would introduce selection bias by eliminating the highest and lowest financial capacity segments
- **Total rows retained: 30,000** — consistent with all Tableau dashboards
**7. BILL_AMT negative values — Retained**  
Negative bill amounts represent credit balances (customer overpaid). These are valid financial states and are retained. Documented as a data characteristic.
 
**8. Feature Engineering — 4 derived columns**
 
| Feature | Formula | Business Meaning |
|---|---|---|
| `utilization_ratio` | `BILL_AMT1 / LIMIT_BAL` | Proportion of credit limit currently consumed. Higher = financially stretched. Zero-division handled. |
| `avg_delay` | `mean(PAY_0 to PAY_6)` | Average payment behavior across 6 months. Captures patterns, not anomalies. |
| `max_delay` | `max(PAY_0 to PAY_6)` | Worst single month of delay. Captures the behavioral floor of a customer. |
| `risk_tier` | Rule-based segmentation | High: max_delay ≥ 2 AND utilization > 0.8 / Medium: max_delay ≥ 1 OR utilization > 0.6 / Low: all others |
 
**9. Label Mapping — 4 readable columns**  
Presentation-ready label columns created for Tableau:  
`sex_label`, `education_label`, `marriage_label`, `default_label`  
Numeric columns retained alongside labels for statistical analysis compatibility.
 
**Final state:**
- **Rows: 30,000** (zero rows dropped)
- **Columns: 32** (25 original − 1 ID + 4 derived + 4 labels)
- **Missing values: 0**
- Output: `data/processed/cleaned_credit_data.csv`
**Correctness:** All cleaning decisions are documented with written justification. The outlier decision (retain vs remove) is correctly aligned with DVA project scope — extreme values are business signal, not noise.
 
---
 
### `03_eda.ipynb` — Exploratory Data Analysis
 
**What is happening:**  
Visual exploration of the cleaned dataset. Every chart is accompanied by a business-language insight — not just a description of what the chart shows.
 
**Analysis structure:**
 
**Block 1 — Univariate Analysis**
- Distribution of `LIMIT_BAL` — right-skewed; most customers hold lower credit limits
- Distribution of `AGE` — majority of customers in 25–40 age range
- `default_status` distribution — 22.12% default rate confirmed visually
**Block 2 — Bivariate: Demographics vs Default**
- Default rate by `education_label` — High School graduates show highest default rate
- Default rate by `marriage_label` — "Others" category shows elevated default risk
- Default rate by `sex_label` — Male customers show marginally higher default rate
- Default rate by age group (18–25, 25–35, 35–50, 50+) — 25–35 age group is largest high-risk cohort
- Default rate by `LIMIT_BAL` tier — Lower credit limit customers default at higher rates
**Block 3 — Payment Behavior Analysis**
- `utilization_ratio` distribution by default group — defaulters cluster above 0.8
- `avg_delay` distribution — defaulters show consistently higher average delay
- `risk_tier` vs `default_status` — High Risk tier validates rule-based segmentation logic
- Delay category vs default — High Delay customers default at ~40% rate vs ~5% for No Delay
**Block 4 — Correlation Analysis**
- Heatmap of all numerical features vs `default_status`
- `avg_delay` and `max_delay` show strongest positive correlation with default
- `LIMIT_BAL` shows negative correlation — higher limit = lower default risk
**Correctness:** All visualisations use `seaborn` and `matplotlib`. Every chart has a written business insight. EDA findings are directly traceable to statistical tests in notebook 04 and dashboard design in Tableau.
 
---
 
### `04_statistical_analysis.ipynb` — Formal Statistical Testing
 
**What is happening:**  
Visual trends from EDA are subjected to rigorous statistical proof. All tests use α = 0.05 (95% confidence interval).
 
**Section 1 — Normality Testing (Shapiro-Wilk)**  
Before selecting statistical tests, normality of key variables was verified.
- `LIMIT_BAL`: p = 0.000000 → NOT normally distributed
- `AGE`: p = 0.000000 → NOT normally distributed  
- **Decision:** Proceed with non-parametric tests for all numerical comparisons.  
- **Why this matters:** Financial data is inherently right-skewed. Using a t-test on non-normal data would produce invalid p-values. Shapiro-Wilk first is the correct methodology chain.
**Section 2 — Mann-Whitney U Test: Credit Limit vs Default**  
- H₀: Defaulters and non-defaulters have the same credit limit distribution  
- H₁: Defaulters have a significantly lower credit limit  
- Result: U = 43,308,360 | p = 1.92e-150 | Effect size (r) = 0.2244  
- Mean LIMIT_BAL — Defaulters: NT$ 114,141 | Non-Defaulters: NT$ 154,342  
- **Conclusion:** H₀ rejected. Defaulters hold significantly lower credit limits. Financial capacity is a statistically proven primary risk signal.
**Section 3 — Chi-Square Tests: Categorical Demographics**  
Testing whether Education and Marriage are statistically dependent on Default outcome.
- `education_label`: χ² = 82.13 | p = 1.07e-17 | Cramér's V = 0.0575 → **SIGNIFICANT**
- `marriage_label`: χ² = 27.74 | p = 9.46e-07 | Cramér's V = 0.0334 → **SIGNIFICANT**  
- **Conclusion:** Both demographics are statistically dependent on default status. This mathematically justifies demographic segmentation in Tableau dashboards.
**Section 4 — VIF Analysis: PAY Column Redundancy**  
- All VIF values across PAY_0 to PAY_6 fall **below 5** (acceptable threshold)  
- No severe multicollinearity detected in isolation  
- **Engineering rationale:** All 6 PAY columns measure the same underlying behavioral concept — payment delay — across consecutive months. They carry temporal redundancy even without high VIF. `avg_delay` was engineered as a dimensionality reduction measure, consolidating 6 monthly columns into one robust behavioral signal that is cleaner for dashboard use and easier to interpret in business context.
**Section 5 — Point-Biserial Correlation: avg_delay vs Default**  
- Measures linear relationship between the engineered `avg_delay` feature and binary `default_status`  
- Result: Positive, statistically significant correlation (p < 0.05)  
- **Conclusion:** Higher average payment delay = higher default probability. Validates `avg_delay` as the primary behavioral KPI for the dashboard.
**Section 6 — Spearman Correlation: Feature Ranking**  
- All key features ranked by correlation strength with `default_status`  
- `avg_delay` and `max_delay` rank highest — confirming payment behavior as the dominant predictor  
- `LIMIT_BAL` shows negative correlation — higher credit limit acts as a protective factor  
- `AGE`, `BILL_AMT1`, `PAY_AMT1` show weaker but statistically significant correlations
**Section 7 — Consolidated Statistical Summary**
 
| Factor | Test | Result | Key Finding |
|---|---|---|---|
| Credit Limit | Mann-Whitney U | Yes ✓ | Defaulters have significantly lower limits (p=1.92e-150) |
| Education Level | Chi-Square | Yes ✓ | Education level and default are statistically dependent |
| Marital Status | Chi-Square | Yes ✓ | Marital status and default are statistically dependent |
| PAY Columns | VIF | Acceptable ✓ | All VIF < 5 — avg_delay engineered to reduce temporal redundancy |
| avg_delay | Point-Biserial | Yes ✓ | Primary behavioral predictor of default |
 
**Correctness:** Methodology chain is mathematically sound throughout — Shapiro-Wilk → non-parametric selection → Mann-Whitney U → Chi-Square → VIF → Point-Biserial → Spearman. Statistical choices are appropriate for skewed financial data.
 
---
 
### `05_final_load_prep.ipynb` — Dashboard-Ready Export
 
**What is happening:**  
The cleaned, engineered dataset is formatted specifically for Tableau consumption. Additional KPI columns and binned groupings are created.
 
**Steps performed:**
- Loaded `cleaned_credit_data.csv` from `data/processed/`
- Created `age_group` bins: 18–25, 25–35, 35–50, 50+
- Created `utilization_category`: High (>0.8), Medium (0.4–0.8), Low (<0.4)
- Created `delay_category`: High Delay (avg_delay > 1), Moderate Delay (0–1), No Delay (≤0)
- Verified all label columns are Tableau-ready (no numeric codes)
- Final null check — confirmed 0 missing values
- Exported to `data/processed/tableau_ready_dataset.csv`
- Final shape confirmed: **30,000 rows × 36 columns**
**Correctness:** Export logic is clean. All KPI groupings are explicitly defined and documented. Dashboard row count (30,000) is consistent with all 5 Tableau dashboard views.
 
---
 
## 2. Summary of Key Findings
 
**Finding 1 — Payment Delay is the Primary Default Predictor**  
avg_delay shows the strongest positive correlation with default_status across all features tested. Defaulters' average repayment delay increased from 0.26 months in April to 0.72 months in September — a 177% deterioration over the 6-month observation window. Every additional month of delay measurably increases default probability.
 
**Finding 2 — Financial Capacity Separates Risk Segments**  
Mann-Whitney U confirmed (p = 1.92e-150) that defaulters hold significantly lower credit limits — NT$ 114,141 vs NT$ 154,342 for non-defaulters. Credit limit acts as a statistically proven protective factor. Lower limit = tighter financial margin = higher default exposure.
 
**Finding 3 — Demographics Carry Statistically Proven Risk Differentials**  
Chi-Square tests confirmed that both Education Level (p = 1.07e-17) and Marital Status (p = 9.46e-07) are statistically dependent on default outcome. High School graduates and customers in the "Others" marital category show elevated default rates. These are not random fluctuations — they are structural risk patterns.
 
**Finding 4 — 9.56% of Customers Are High Risk But Drive Disproportionate Default Volume**  
2,868 customers (9.56%) classified as High Risk under the rule-based segmentation. The High Risk tier shows a default rate of approximately 42% — nearly double the portfolio average of 22.12%. Targeting this segment for proactive intervention represents the highest-impact risk management action.
 
**Finding 5 — Utilization and Delay Together Define the Risk Boundary**  
Customers with utilization ratio above 0.8 AND max_delay above 2 months show catastrophic default rates. The Financial Stress scatter plot confirms a critical risk zone where both financial strain and behavioral deterioration converge.
 
---
 
## 3. Business Recommendations
 
**Recommendation 1 — Early Warning Trigger on avg_delay**  
Flag any customer whose avg_delay crosses 1.0 for immediate outreach. The data shows this threshold as the inflection point between Moderate Risk and High Risk tier default rates.
 
**Recommendation 2 — Dynamic Credit Limit Review**  
Customers with LIMIT_BAL below NT$ 100,000 AND utilization_ratio above 0.8 should be reviewed for limit adjustment or payment plan restructuring before delay escalates.
 
**Recommendation 3 — Demographic-Targeted Financial Education**  
High School graduate customers show the highest default rate by education segment. Targeted financial literacy programs for this cohort could reduce default exposure at the acquisition stage.
 
---
 
## 4. Pipeline Integrity Confirmation
 
| Check | Status |
|---|---|
| Raw data untouched in `data/raw/` | ✅ |
| Row count consistent across all notebooks | ✅ 30,000 |
| Row count consistent with Tableau dashboards | ✅ 30,000 |
| Zero missing values in final cleaned dataset | ✅ |
| All statistical test choices justified | ✅ |
| Outlier decision documented and defensible | ✅ Retained with rationale |
| VIF conclusions match actual output values | ✅ Fixed |
| Feature engineering tied to business logic | ✅ |
| All notebooks re-runnable top to bottom | ✅ |