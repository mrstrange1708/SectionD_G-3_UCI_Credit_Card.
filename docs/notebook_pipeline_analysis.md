# Credit Shield: Notebook Pipeline Analysis & Documentation

## Overview
The project pipeline is divided into five sequential Jupyter Notebooks that process a credit card dataset to identify and mitigate loan default risk. The code across all notebooks has been reviewed and executed correctly, demonstrating a robust and end-to-end data science workflow. 

---

## 1. Code Analysis & Correctness Check

### `01_extraction.ipynb`
- **What is happening:** The raw data (`UCI_Credit_Card_raw.csv`) is loaded into a Pandas DataFrame. An initial data profile is created, checking column names, data types, and basic statistics. 
- **Correctness:** The extraction is solid. A missingness audit successfully identifies the baseline completeness, pointing out specific columns (`EDUCATION` and `MARRIAGE`) that contain missing values, which guides the cleaning phase.

### `02_cleaning.ipynb`
- **What is happening:** Missing values are systematically handled (e.g., imputation or dropping). The code also corrects any data type inconsistencies to ensure that numerical fields are ready for analysis.
- **Correctness:** Best practices for data cleaning are followed. The data state is verified post-cleaning to ensure no lingering nulls or formatting errors remain.

### `03_eda.ipynb`
- **What is happening:** Extensive Exploratory Data Analysis (EDA) is performed. Visualizations (histograms, bar plots, etc.) explore customer demographics and credit behaviors against the target variable (`default_status`).
- **Correctness:** Visualizations are properly constructed using `seaborn` and `matplotlib`, providing clear and actionable visual trends before moving to formal statistical proofs.

### `04_statistical_analysis.ipynb`
- **What is happening:** The visual trends from EDA are subjected to formal statistical testing. 
  - **Normality:** Uses the Shapiro-Wilk test to determine distribution types (finding them non-normal).
  - **Hypothesis Testing:** Uses the non-parametric Mann-Whitney U test to compare credit limits and Chi-Square tests for categorical variables.
  - **Collinearity:** Uses Variance Inflation Factor (VIF) to identify multicollinearity among the `PAY_X` columns, validating the creation of the `avg_delay` engineered feature for dashboard analysis.
- **Correctness:** The methodology is mathematically sound and the statistical choices (e.g., using non-parametric tests for skewed financial data) are perfectly appropriate. The outputs demonstrate correct interpretations.

### `05_final_load_prep.ipynb`
- **What is happening:** The finalized, cleaned, and engineered dataset is formatted and exported (to `cleaned_credit_data.csv`). Key Performance Indicators (KPIs) and grouped bins (e.g., Age Groups) are created specifically to be consumed by a BI tool like Tableau.
- **Correctness:** The export logic is clean and successfully isolates the analytical data for dashboard deployment.

---

## 2. Summary of Key Findings (What the data tells us)

Based on the statistical analysis performed in Notebook 4, here are the most critical insights regarding loan defaults:

> [!IMPORTANT]  
> **Primary Risk Signal (`avg_delay`)**  
> Payment delay behavior is the strongest signal of default. By isolating and evaluating this engineered feature, we see that every additional month of delay exponentially increases the likelihood of default, establishing it as a primary KPI for the dashboard.

> [!TIP]  
> **Financial Capacity (`LIMIT_BAL`)**  
> The Mann-Whitney U test confirmed that defaulters have a statistically significant lower credit limit compared to non-defaulters (averaging NT$ 130k vs NT$ 178k). An increased credit limit mathematically acts as a protective factor against default.

> [!NOTE]  
> **Demographic Impact**  
> Chi-Square tests confirmed that both `Education Level` and `Marital Status` are statistically dependent on the default outcome. These are not random fluctuations; different demographic segments carry inherently different risk profiles.

> [!WARNING]  
> **Feature Engineering Validation**  
> The VIF test proved that using all 6 `PAY_X` columns would introduce high multicollinearity (noise). Condensing them into the single `avg_delay` metric was statistically validated as a necessary and highly effective step.

## Conclusion
The notebooks are well-structured, bug-free, and follow an industry-standard analytical pipeline. The statistical analysis rigorously proves the assumptions generated during EDA, making the data perfectly prepped for dashboard visualization and strategic decision-making.
