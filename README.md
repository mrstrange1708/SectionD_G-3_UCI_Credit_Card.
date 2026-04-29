# Credit Shield: Identifying and Mitigating Loan Default Risk
 
> **Newton School of Technology | Data Visualization & Analytics Capstone 2**  
> An industry-standard data analytics project using Python, Tableau, and GitHub to convert raw financial data into actionable risk intelligence.
 
---
 
## Project Overview
 
| Field | Details |
|---|---|
| **Project Title** | Credit Shield: Identifying and Mitigating Loan Default Risk |
| **Sector** | Finance |
| **Team** | Section D, Group 3 |
| **Faculty Mentors** | Archit Raj & Satyaki Das |
| **Institute** | Newton School of Technology |
| **Submission Date** | April 29, 2026 |
 
---
 
## Team Members
 
| Role | Name | GitHub Username |
|---|---|---|
| Project Lead | Shaik Junaid Sami | [`mrstrange1708`](https://github.com/mrstrange1708) |
| Data Lead | Shaik Junaid Sami | [`mrstrange1708`](https://github.com/mrstrange1708) |
| ETL Lead | Shaik Junaid Sami | [`mrstrange1708`](https://github.com/mrstrange1708) |
| Analysis Lead | Kartik Mehra | [`KartikMehra22`](https://github.com/KartikMehra22) |
| Visualization Lead | Mansi Agarwal | [`Aggarwalmansi`](https://github.com/Aggarwalmansi) |
| Strategy Lead | Yogesh Modi | [`YogeshModi24`](https://github.com/YogeshModi24) |
| PPT & Quality Lead | Keshav Goyal | [`keshavgoel2101`](https://github.com/keshavgoel2101) |
 
---
 
## Business Problem
 
Financial institutions face increasing loan defaults, leading to revenue loss and higher risk exposure. This project analyzes customer demographics, transaction behavior, and credit patterns to identify key factors driving loan defaults, segment high-risk customers, and recommend strategies to reduce default rates and improve risk management.
 
**Core Business Question:**
 
> What are the primary demographic and behavioral drivers of credit default, and how can we segment customers to proactively mitigate risk?
 
**Decision Supported:**
 
> Enable credit risk officers to adjust credit limits, intensify monitoring for high-risk segments, and refine lending criteria to reduce overall portfolio loss.
 
---
 
## Dataset
 
| Attribute | Details |
|---|---|
| **Source** | UCI Machine Learning Repository |
| **Kaggle Link** | [Default of Credit Card Clients Dataset](https://www.kaggle.com/datasets/uciml/default-of-credit-card-clients-dataset) |
| **Rows** | 30,000 |
| **Columns** | 25 (original) → 32 (after cleaning and feature engineering) |
| **Time Period** | April 2005 – September 2005 (Taiwan) |
| **Format** | CSV |
 
**Key Variables:**
 
| Variable | Description | Role |
|---|---|---|
| `LIMIT_BAL` | Credit limit (NT$) | Primary financial capacity measure |
| `avg_delay` | Average payment delay (6 months) | Strongest behavioral predictor |
| `utilization_ratio` | Bill amount / Credit limit | Financial stress indicator |
| `default_status` | Default next month (1=yes, 0=no) | Target variable |
 
Full column definitions: [`docs/data_dictionary.md`](docs/data_dictionary.md)
 
---
 
## Key Findings
 
### 1. Payment Delay is the Primary Default Predictor
`avg_delay` shows the strongest positive correlation with default across all features tested. Defaulters' average repayment delay increased from **0.26 months in April to 0.72 months in September** — a **177% deterioration** over the 6-month observation window.
 
### 2. Financial Capacity Separates Risk Segments
Mann-Whitney U test confirmed (p = 6.13e-190) that defaulters hold significantly lower credit limits:
- **Defaulters:** NT$ 130,110
- **Non-defaulters:** NT$ 178,100
- **Difference:** NT$ 47,990 (27% lower)
### 3. Demographics Carry Statistically Proven Risk Differentials
Chi-Square tests confirmed both Education Level (p = 1.55e-23) and Marital Status (p = 1.51e-07) are statistically dependent on default outcome. High School graduates and customers in the "Others" marital category show elevated default rates.
 
### 4. High Risk Segment Drives Disproportionate Defaults
**2,868 customers (9.56%)** classified as High Risk under rule-based segmentation show a default rate of approximately **42%** — nearly **double the portfolio average of 22.12%**.
 
### 5. Critical Risk Zone Identified
Customers with `utilization_ratio > 0.8` AND `max_delay ≥ 2` show catastrophic default rates. The Financial Stress scatter plot confirms a critical risk zone where both financial strain and behavioral deterioration converge.
 
---
 
## Business Recommendations
 
| # | Recommendation | Expected Impact |
|---|---|---|
| 1 | **Early Warning System:** Flag customers when `avg_delay` crosses 1.0 for immediate outreach | Catch majority of High Risk defaults 2-3 months early |
| 2 | **Dynamic Credit Limit Review:** Customers with `LIMIT_BAL < NT$ 100,000` AND `utilization_ratio > 0.8` should be reviewed for limit adjustment or payment plan restructuring | Reduce distress-driven defaults by providing financial breathing room |
| 3 | **Demographic-Targeted Financial Education:** Targeted financial literacy programs for High School graduate segment | Reduce default exposure at acquisition stage for highest-risk education tier |
 
---
 
## Repository Structure
 
```
SectionD_G-3_UCI_Credit_Card/
│
├── README.md                           # This file
│
├── data/
│   ├── raw/
│   │   └── UCI_Credit_Card_raw.csv     # Original dataset (never edited)
│   └── processed/
│       ├── cleaned_credit_data.csv     # Output from notebook 02 (30,000 rows)
│       └── tableau_ready_dataset.csv   # Output from notebook 05 (30,000 rows)
│
├── notebooks/
│   ├── 01_extraction.ipynb             # Data loading and baseline profiling
│   ├── 02_cleaning.ipynb               # ETL pipeline with feature engineering
│   ├── 03_eda.ipynb                    # Exploratory data analysis
│   ├── 04_statistical_analysis.ipynb   # Hypothesis testing and formal proofs
│   └── 05_final_load_prep.ipynb        # Dashboard-ready export
│
├── scripts/
│   └── etl_pipeline.py                 # Modular ETL script (optional)
│
├── tableau/
│   ├── screenshots/                    # Dashboard PNG exports
│   │   ├── Behavioral___Segment_Analysis.png
│   │   ├── Credit_Risk_Overview.png
│   │   ├── Customer_Risk_by_Demographics.png
│   │   ├── Financial_Behavior___Stress_Analysis.png
│   │   └── Repayment_Behavior_Analysis.png
│   └── dashboard_links.md              # Tableau Public URLs
│
├── reports/
│   ├── notebook_pipeline_analysis.md   # Complete pipeline documentation
│   └── Shaik_Mohammed_Junaid_Sami_DVA_Resume.pdf
│
├── docs/
│   └── data_dictionary.md              # Column definitions and transformations
│
└── requirements.txt                    # Python dependencies
```
 
---
 
## Tech Stack
 
| Tool | Version | Purpose |
|---|---|---|
| Python | 3.12 | ETL, cleaning, analysis, KPI computation |
| Jupyter Notebooks | Latest | Interactive analysis environment |
| Pandas | Latest | Data manipulation and transformation |
| NumPy | Latest | Numerical operations |
| SciPy | Latest | Statistical testing |
| Seaborn / Matplotlib | Latest | Visualization |
| Statsmodels | Latest | VIF and advanced statistics |
| Tableau Public | Latest | Interactive dashboard design and publishing |
| GitHub | Latest | Version control and collaboration |
 
---
 
## Getting Started
 
### 1. Clone the Repository
 
```bash
git clone https://github.com/mrstrange1708/SectionD_G-3_UCI_Credit_Card.git
cd SectionD_G-3_UCI_Credit_Card
```
 
### 2. Set Up Python Environment
 
**Option A: Using venv**
```bash
python3.12 -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
pip install -r requirements.txt
```
 
**Option B: Using Conda**
```bash
conda create -n dva-capstone python=3.12
conda activate dva-capstone
pip install -r requirements.txt
```
 
### 3. Run the Notebooks
 
```bash
jupyter notebook
```
 
Open notebooks in order: `01_extraction.ipynb` → `02_cleaning.ipynb` → `03_eda.ipynb` → `04_statistical_analysis.ipynb` → `05_final_load_prep.ipynb`
 
### 4. (Optional) Run the ETL Pipeline Script
 
To regenerate the cleaned dataset from scratch:
 
```bash
python scripts/etl_pipeline.py \
  --input data/raw/UCI_Credit_Card_raw.csv \
  --output data/processed/cleaned_credit_data.csv
```
 
---
 
## Tableau Dashboard
 
**Live Dashboard:** [Credit Shield - Risk Intelligence Dashboard](https://public.tableau.com/app/profile/mansi.agarwal6786/viz/creditCard_17770722137780/Dashboard5?publish=yes)
 
### Dashboard Views:
 
1. **Credit Risk Overview** — Portfolio-level KPIs and default distribution
2. **Customer Risk by Demographics** — Default rates segmented by education, marriage, gender, and age
3. **Behavioral & Segment Analysis** — Risk tier distribution and delay behavior patterns
4. **Financial Behavior & Stress Analysis** — Utilization vs payment gap scatter and bill/payment comparison
5. **Repayment Behavior Analysis** — Payment trend over 6 months and delay score distribution
**All dashboards consistently show 30,000 total customers** — verified for data integrity.
 
---
 
## Analytical Pipeline
 
The project follows a structured 5-notebook workflow:
 
### Notebook 01: Extraction
- Load raw CSV (`UCI_Credit_Card_raw.csv`)
- Baseline data profiling
- Identify implicit missing values (EDUCATION: 0,5,6 | MARRIAGE: 0)
- Document class imbalance (77.88% non-default vs 22.12% default)
### Notebook 02: Cleaning & Feature Engineering
**Transformations:**
- Drop `ID` column
- Rename `default.payment.next.month` → `default_status`
- Impute EDUCATION and MARRIAGE missing values with mode
- Recode PAY columns: `-2` → `-1` (preserve behavioral signal)
- **Outlier audit:** Extreme values identified but **retained** (valid financial profiles)
- Engineer 4 features: `utilization_ratio`, `avg_delay`, `max_delay`, `risk_tier`
- Create 4 label columns for Tableau: `sex_label`, `education_label`, `marriage_label`, `default_label`
**Output:** `cleaned_credit_data.csv` — **30,000 rows × 32 columns**
 
### Notebook 03: Exploratory Data Analysis
- Univariate distributions (LIMIT_BAL, AGE, default_status)
- Bivariate analysis: demographics vs default, payment behavior vs default
- Correlation heatmap
- Visual validation of engineered features
### Notebook 04: Statistical Analysis
**Tests performed (α = 0.05):**
1. **Shapiro-Wilk:** Normality testing → both LIMIT_BAL and AGE non-normal → use non-parametric tests
2. **Mann-Whitney U:** Credit limit vs default → p = 6.13e-190 → H₀ rejected → defaulters have significantly lower limits
3. **Chi-Square:** Education and Marriage vs default → both p < 0.05 → statistically dependent
4. **VIF:** PAY columns multicollinearity check → all VIF < 5 → acceptable individually, but temporal redundancy justifies `avg_delay` consolidation
**Output:** Formal statistical proof of all EDA observations
 
### Notebook 05: Final Load Prep
- Add binned groupings: `age_group`, `utilization_category`, `delay_category`
- Export Tableau-ready dataset: `tableau_ready_dataset.csv` — **30,000 rows × 36 columns**
---
 
## Key Statistical Results
 
| Test | Variable(s) | Result | Interpretation |
|---|---|---|---|
| **Shapiro-Wilk** | LIMIT_BAL, AGE | p < 0.05 | Non-normal → use non-parametric tests |
| **Mann-Whitney U** | LIMIT_BAL vs default | p = 6.13e-190, r = 0.2356 | Defaulters have significantly lower credit limits |
| **Chi-Square** | EDUCATION vs default | χ² = 109.30, p = 1.55e-23 | Education level statistically dependent on default |
| **Chi-Square** | MARRIAGE vs default | χ² = 31.41, p = 1.51e-07 | Marital status statistically dependent on default |
| **VIF** | PAY_0 to PAY_6 | All VIF < 5 | Acceptable individually, avg_delay engineered for temporal redundancy reduction |
 
Full analysis: [`notebooks/04_statistical_analysis.ipynb`](notebooks/04_statistical_analysis.ipynb)
 
---
 
## Data Quality & Integrity
 
### Cleaning Decisions Documented:
 
| Issue | Decision | Rationale |
|---|---|---|
| **EDUCATION values 0,5,6** | Treated as NaN → mode imputation | Undocumented in UCI dictionary → implicit missing values |
| **MARRIAGE value 0** | Treated as NaN → mode imputation | Undocumented in UCI dictionary → implicit missing values |
| **PAY column value -2** | Recoded to -1 | Preserve behavioral signal of zero-balance months |
| **Outliers in LIMIT_BAL, BILL_AMT1, PAY_AMT1** | **Retained all rows** | Extreme values represent valid high-credit-limit and high-bill customers — removing would introduce selection bias |
| **Negative BILL_AMT values** | Retained | Represent credit balances (customer overpaid) — valid financial state |
 
**Final dataset integrity:**
- **30,000 rows retained** (zero rows dropped)
- **Zero missing values** in final cleaned dataset
- **Consistent across all notebooks and Tableau dashboards**
Full documentation: [`docs/data_dictionary.md`](docs/data_dictionary.md) | [`reports/notebook_pipeline_analysis.md`](reports/notebook_pipeline_analysis.md)
 
---
 
## Project Deliverables
 
- ✅ **5 Jupyter Notebooks** — Complete ETL and analysis pipeline
- ✅ **Tableau Dashboard** — 5 interactive views published on Tableau Public
- ✅ **Statistical Analysis** — Formal hypothesis testing with p-values
- ✅ **Data Dictionary** — All variables documented
- ✅ **Pipeline Documentation** — Complete notebook-by-notebook analysis
- ✅ **Business Recommendations** — 3 actionable strategies
- ✅ **Project Report** — Completed ([Final_Report.pdf](reports/Final_Report.pdf))
- ✅ **Presentation Deck** — Completed ([Presentation_Slides.pptx](reports/Presentation_Slides.pdf))

 
## Academic Integrity
 
All analysis, code, and recommendations in this repository are the original work of the team listed above. Contribution tracking is verified through GitHub Insights, commit history, and pull request logs.
 
**Team Lead:** Shaik Junaid Sami  
**Date:** April 28, 2026
 
---
 
## License
 
This project is submitted as part of the Newton School of Technology Data Visualization & Analytics Capstone 2 curriculum. All rights reserved.
 
---
 
## Contact
 
For questions or collaboration:
- **Project Lead:** Shaik Junaid Sami — [GitHub](https://github.com/mrstrange1708)
- **Visualization Lead:** Mansi Agarwal — [GitHub](https://github.com/Aggarwalmansi)
---
 
*Newton School of Technology — Data Visualization & Analytics | Capstone 2 | Section D, Group 3*
