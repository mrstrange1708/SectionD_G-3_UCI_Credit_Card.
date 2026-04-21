# UCI Credit Card Data Dictionary

This document defines all variables used in the credit card default analysis and describes the cleaning and engineering transformations applied.

## Dataset Summary

| Item | Details |
|---|---|
| Dataset name | Default of Credit Card Clients Dataset |
| Source | UCI Machine Learning Repository |
| Raw file name | UCI_Credit_Card_raw.csv |
| Last updated | 2026-04-21 |
| Granularity | One row per customer record (30,000 observations) |

## Column Definitions

| Column Name | Data Type | Description | Example Value | Used In | Cleaning Notes |
|---|---|---|---|---|---|
| LIMIT_BAL | float | Amount of given credit (NT dollar). | 140000.0 | EDA / KPI | Strictly positive values. |
| SEX | int | Gender (1=male, 2=female). | 1 | EDA | Raw numerical format. |
| EDUCATION | float | Education level. | 2.0 | EDA | Nulls imputed with mode (2.0). |
| MARRIAGE | float | Marital status. | 1.0 | EDA | Nulls imputed with mode (2.0). |
| AGE | int | Age in years. | 35 | EDA | Range: 21 to 79. |
| PAY_0 to PAY_6 | int | History of past payment (delay in months). | 1 | EDA / KPI | Recoded -2 (no use) to -1 (paid duly). |
| BILL_AMT1 to BILL_AMT6| float | Amount of bill statement (NT dollar). | 17000.0 | EDA / KPI | Kept negatives (credit balances). |
| PAY_AMT1 to PAY_AMT6 | float | Amount of previous payment (NT dollar). | 2000.0 | EDA / KPI | Raw numerical format. |
| default_status | int | Default payment next month (1=yes, 0=no).| 0 | EDA / KPI | Renamed from default.payment.next.month. |

## Derived & Label Columns

| Column Name | Logic | Business Meaning |
|---|---|---|
| utilization_ratio| BILL_AMT1 / LIMIT_BAL | Meaures credit capacity usage. Capped handled for div by zero. |
| avg_delay | mean(PAY_0...PAY_6) | Captures long-term behavioral patterns of delay vs. promptness. |
| max_delay | max(PAY_0...PAY_6) | Captures the worst-case behavior (floor) of a customer. |
| risk_tier | Rule-based (delay + usage) | High: max_delay >= 2 & usage > 80%; Medium: max_delay >= 1 or usage > 60%; else Low. |
| sex_label | map(SEX) | Readable label: 'Male', 'Female'. |
| education_label | map(EDUCATION) | Readable label (Sanitized: 0,4,5,6 -> Others). |
| marriage_label | map(MARRIAGE) | Readable label (Sanitized: 0,3 -> Others). |
| default_label | map(default_status) | Readable label: 'No Default', 'Default'. |

## Data Quality Notes

- **Missing Values**: Handed via mode imputation for `EDUCATION` and `MARRIAGE`.
- **Categorical Sanitization**: Consolidated many undocumented/sparse labels in `EDUCATION` and `MARRIAGE` into 'Others'.
- **Behavioral Signal**: Preserved zero-balance months in `PAY_X` by recoding them to -1 (paid duly) instead of dropping or NaN.
