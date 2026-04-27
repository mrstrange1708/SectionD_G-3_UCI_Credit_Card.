# Credit Shield: Identifying and Mitigating Loan Default Risk

> **Newton School of Technology | Data Visualization & Analytics**
> A 2-week industry simulation capstone using Python, GitHub, and Tableau to convert raw data into actionable business intelligence.

---

### Quick Start

If you are working locally:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook
```

If you are working in Google Colab:

- Upload or sync the notebooks from `notebooks/`
- Keep the final `.ipynb` files committed to GitHub
- Export any cleaned datasets into `data/processed/`

---

## Project Overview

| Field | Details |
|---|---|
| **Project Title** | Credit Shield: Identifying and Mitigating Loan Default Risk |
| **Sector** | Finance |
| **Team ID** | G3 |
| **Section** | D |
| **Faculty Mentor** | Archit Raj & Satyaki Das |
| **Institute** | Newton School of Technology |
| **Submission Date** | 2026-04-29 |


### Team Members

| Role | Name | GitHub Username |
|---|---|---|
| Project Lead | Shaik Junaid Sami | `mrstrange1708` |
| Data Lead |  Shaik Junaid Sami  | `mrstrange1708` |
| ETL Lead |  Shaik Junaid Sami  | `mrstrange1708` |
| Analysis Lead | Kartik Mehra | `KartikMehra22` |
| Visualization Lead | Mansi Agarwal | `Aggarwalmansi` |
| Strategy Lead | Yogesh Modi | `YogeshModi24` |
| PPT and Quality Lead | Keshav Goyal | `keshavgoel2101` |

---

## Business Problem

Financial institutions face increasing loan defaults, leading to revenue loss and higher risk exposure. This project aims to analyze customer demographics, transaction behavior, and credit patterns to identify key factors driving loan defaults, segment high-risk customers, and recommend strategies to reduce default rates and improve risk management.

**Core Business Question**

> What are the primary demographic and behavioral drivers of credit default, and how can we segment customers to proactively mitigate risk?

**Decision Supported**

> Enabling credit risk officers to adjust credit limits, intensify monitoring for high-risk segments, and refine lending criteria to reduce overall loss.


---

## Dataset

| Attribute | Details |
|---|---|
| **Source Name** | UCI Machine Learning Repository |
| **Direct Access Link** | https://www.kaggle.com/datasets/uciml/default-of-credit-card-clients-dataset?select=UCI_Credit_Card.csv |
| **Row Count** | 30,000 |
| **Column Count** | 32 (After feature engineering) |
| **Time Period Covered** | April 2005 to September 2005 |
| **Format** | CSV |

**Key Columns Used**

| Column Name | Description | Role in Analysis |
|---|---|---|
| `LIMIT_BAL` | Amount of given credit (NT dollar) | Primary measure of financial capacity & exposure |
| `avg_delay` | Average payment delay across 6 months | Strongest behavioral risk predictor |
| `utilization_ratio` | Credit utilized vs limit | Indicator of financial distress / reliance |
| `default_status` | Default payment (1=yes, 0=no) | Primary target variable used for risk tracking |

For full column definitions, see [`docs/data_dictionary.md`](docs/data_dictionary.md).

---

## KPI Framework

| KPI | Definition | Formula / Computation |
|---|---|---|
| **Overall Default Rate (%)** | Percentage of the total portfolio that has defaulted | `Total Defaulters / Total Customers * 100` |
| **Risk Tier Exposure** | Average credit limit assigned to customers within specific risk segments | `mean(LIMIT_BAL)` grouped by `risk_tier` |
| **Behavioral Delay Index** | A blended average of a customer's payment latencies over 6 months | `mean(PAY_0` through `PAY_6)` |

Document KPI logic clearly in `notebooks/04_statistical_analysis.ipynb` and `notebooks/05_final_load_prep.ipynb`.

---

## Tableau Dashboard

| Item | Details |
|---|---|
| **Dashboard URL** | https://public.tableau.com/app/profile/mansi.agarwal6786/viz/creditCard_17770722137780/Dashboard5 |
| **Executive View** | _Describe the high-level KPI summary view_ |
| **Operational View** | _Describe the detailed drill-down view_ |
| **Main Filters** | _List the interactive filters used_ |

Store dashboard screenshots in [`tableau/screenshots/`](tableau/screenshots/) and document the public links in [`tableau/dashboard_links.md`](tableau/dashboard_links.md).

---

## Key Insights

_List 8-12 major findings from the analysis, written in decision language. Each insight should tell the reader what to think or act upon, not merely describe a chart._

1. **Financial Buffer Focus**: Defaulters consistently possess lower credit limits (NT$ 130K) compared to non-defaulters (NT$ 178K), indicating lower financial capacity.
2. **Behavioral Red Flags**: `avg_delay` is overwhelmingly the strongest predictor of default; every additional month of delay exponentially increases default odds.
3. **Demographic Stability**: Marital status and Education level mathematically act as risk differentiators, with Graduate School level clients displaying the lowest baseline risk profile.
4. **Noise Reduction via Engineered Features**: Payment delay months (`PAY_0` to `PAY_6`) exhibit very high multicollinearity (VIF > 10). Consolidating them into an `avg_delay` metric removes noise and sharpens dashboard analysis.
5. **Behavioral Significance**: Statistical correlation confirms that behavioral payment metrics vastly outweigh demographic data in signaling failure risk.

---

## Recommendations

_Provide 3-5 specific, actionable business recommendations, each linked directly to an insight above._

| # | Insight | Recommendation | Expected Impact |
|---|---|---|---|
| 1 | `avg_delay` is the top predictor | **Behavioral Early-Warning System**: Flag any customer with `avg_delay > 1.5` months for early outreach before they miss a third payment. | Catches the majority of defaulters in the 'High Risk' tier 2-3 months early. |
| 2 | Lower limits → higher default | **Dynamic Limit Adjustment**: Offer managed, proactive limit increases to good-standing 'Medium Risk' customers. | Reduces 'distress-driven' defaults by providing financial breathing room. |
| 3 | Education & Marriage dependencies | **Demographic-Tailored Products**: Design graduated onboarding limits with stricter initial monitoring for 'Others/High School' tiers. | Reduces initial bank exposure in statistically higher-risk segments. |

---

## Repository Structure

```text
SectionName_TeamID_ProjectName/
|
|-- README.md
|
|-- data/
|   |-- raw/                         # Original dataset (never edited)
|   `-- processed/                   # Cleaned output from ETL pipeline
|
|-- notebooks/
|   |-- 01_extraction.ipynb
|   |-- 02_cleaning.ipynb
|   |-- 03_eda.ipynb
|   |-- 04_statistical_analysis.ipynb
|   `-- 05_final_load_prep.ipynb
|
|-- scripts/
|   `-- etl_pipeline.py
|
|-- tableau/
|   |-- screenshots/
|   `-- dashboard_links.md
|
|-- reports/
|   |-- README.md
|   |-- project_report_template.md
|   `-- presentation_outline.md
|
|-- docs/
|   |-- data_dictionary.md
|   `-- notebook_pipeline_analysis.md
|
|-- DVA-oriented-Resume/
`-- DVA-focused-Portfolio/
```

---

## Analytical Pipeline

The project follows a structured 7-step workflow:

1. **Define** - Sector selected, problem statement scoped, mentor approval obtained.
2. **Extract** - Raw dataset sourced and committed to `data/raw/`; data dictionary drafted.
3. **Clean and Transform** - Cleaning pipeline built in `notebooks/02_cleaning.ipynb` and optionally `scripts/etl_pipeline.py`.
4. **Analyze** - EDA and statistical analysis performed in notebooks `03` and `04`.
5. **Visualize** - Interactive Tableau dashboard built and published on Tableau Public.
6. **Recommend** - 3-5 data-backed business recommendations delivered.
7. **Report** - Final project report and presentation deck completed and exported to PDF in `reports/`.

---

## Tech Stack

| Tool | Status | Purpose |
|---|---|---|
| Python + Jupyter Notebooks | Mandatory | ETL, cleaning, analysis, and KPI computation |
| Google Colab | Supported | Cloud notebook execution environment |
| Tableau Public | Mandatory | Dashboard design, publishing, and sharing |
| GitHub | Mandatory | Version control, collaboration, contribution audit |
| SQL | Optional | Initial data extraction only, if documented |

**Recommended Python libraries:** `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`, `statsmodels`

---

## Evaluation Rubric

| Area | Marks | Focus |
|---|---|---|
| Problem Framing | 10 | Is the business question clear and well-scoped? |
| Data Quality and ETL | 15 | Is the cleaning pipeline thorough and documented? |
| Analysis Depth | 25 | Are statistical methods applied correctly with insight? |
| Dashboard and Visualization | 20 | Is the Tableau dashboard interactive and decision-relevant? |
| Business Recommendations | 20 | Are insights actionable and well-reasoned? |
| Storytelling and Clarity | 10 | Is the presentation professional and coherent? |
| **Total** | **100** | |

> Marks are awarded for analytical thinking and decision relevance, not chart quantity, visual decoration, or code length.

---

## Submission Checklist

**GitHub Repository**

- [ ] Public repository created with the correct naming convention (`SectionName_TeamID_ProjectName`)
- [ ] All notebooks committed in `.ipynb` format
- [ ] `data/raw/` contains the original, unedited dataset
- [ ] `data/processed/` contains the cleaned pipeline output
- [ ] `tableau/screenshots/` contains dashboard screenshots
- [ ] `tableau/dashboard_links.md` contains the Tableau Public URL
- [ ] `docs/data_dictionary.md` is complete
- [ ] `README.md` explains the project, dataset, and team
- [ ] All members have visible commits and pull requests

**Tableau Dashboard**

- [ ] Published on Tableau Public and accessible via public URL
- [ ] At least one interactive filter included
- [ ] Dashboard directly addresses the business problem

**Project Report**

- [ ] Final report exported as PDF into `reports/`
- [ ] Cover page, executive summary, sector context, problem statement
- [ ] Data description, cleaning methodology, KPI framework
- [ ] EDA with written insights, statistical analysis results
- [ ] Dashboard screenshots and explanation
- [ ] 8-12 key insights in decision language
- [ ] 3-5 actionable recommendations with impact estimates
- [ ] Contribution matrix matches GitHub history

**Presentation Deck**

- [ ] Final presentation exported as PDF into `reports/`
- [ ] Title slide through recommendations, impact, limitations, and next steps

**Individual Assets**

- [ ] DVA-oriented resume updated to include this capstone
- [ ] Portfolio link or project case study added

---

## Contribution Matrix

This table must match evidence in GitHub Insights, PR history, and committed files.

| Team Member | Dataset and Sourcing | ETL and Cleaning | EDA and Analysis | Statistical Analysis | Tableau Dashboard | Report Writing | PPT and Viva |
|---|---|---|---|---|---|---|---|
| _Member 1_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ |
| _Member 2_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ |
| _Member 3_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ |
| _Member 4_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ |
| _Member 5_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ |
| _Member 6_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ | _Owner / support_ |

_Declaration: We confirm that the above contribution details are accurate and verifiable through GitHub Insights, PR history, and submitted artifacts._

**Team Lead Name:** _____________________________

**Date:** _______________

---

## Academic Integrity

All analysis, code, and recommendations in this repository must be the original work of the team listed above. Free-riding is tracked via GitHub Insights and pull request history. Any mismatch between the contribution matrix and actual commit history may result in individual grade adjustments.

---

*Newton School of Technology - Data Visualization & Analytics | Capstone 2*
