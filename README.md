# Telco Customer Churn Analysis & Prediction

A end-to-end data analysis project combining SQL-based exploration, machine learning, and Power BI dashboarding to understand and predict customer churn in a telecommunications company.

---

## Project Overview

Customer churn is one of the most critical business problems in subscription-based industries. This project analyzes churn behavior using the IBM Telco Customer Churn dataset (7,043 customers), identifies high-risk segments through SQL analysis, and builds a predictive model using Python.

**Key finding:** A specific high-risk segment (month-to-month contract + Fiber optic + no TechSupport + Electronic check payment) shows a churn rate of **62.92%**, more than 2.4x the overall average of 26.54%.

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| Python (Google Colab) | Data cleaning, feature engineering, machine learning |
| SQLite + pandas | Relational database creation and SQL analysis |
| Power BI | Interactive dashboard |
| GitHub | Version control and portfolio |

---

## Dataset

- **Source:** [IBM Telco Customer Churn Dataset](https://github.com/IBM/telco-customer-churn-on-icp4d)
- **Size:** 7,043 customers, 21 columns
- **Target variable:** `Churn` (Yes/No)
- **Features:** Demographics, subscribed services, contract type, billing information

---

## Project Structure

```
telco-customer-churn/
│
├── data/
│   ├── telco_churn.csv                  # Raw dataset
│   └── telco_churn_cleaned.csv          # Cleaned dataset
│
├── notebooks/
│   └── telco_churn_analysis.ipynb       # Full analysis notebook
│
├── outputs/
│   ├── churn_by_contract.csv
│   ├── churn_by_tenure.csv
│   ├── feature_importance.csv
│   ├── model_predictions.csv
│   ├── kpi_summary.csv
│   ├── confusion_matrix.png
│   ├── feature_importance.png
│   ├── dashboard_page1.png
│   └── dashboard_page2.png
│
├── dashboard/
│   └── telco_churn_dashboard.pbix       # Power BI dashboard
│
└── README.md
```

---

## Dashboard

### Page 1 — Churn Overview
![Churn Overview](https://raw.githubusercontent.com/Rominabh/telco-customer-churn/main/dashboard_page1.png)

### Page 2 — Model Insights
![Model Insights](https://raw.githubusercontent.com/Rominabh/telco-customer-churn/main/dashboard_page2.png)

---

## Part 1: SQL Analysis

The cleaned dataset was loaded into a **SQLite database** and split into three relational tables:

- `customers` — demographic information
- `services` — subscribed services
- `billing` — contract, payment, and churn status

### Key SQL Techniques Used

| Technique | Query |
|-----------|-------|
| JOIN + CASE WHEN | Churn rate by contract type |
| GROUP BY + AVG | Churn rate by payment method |
| CASE WHEN cohort | Churn rate by tenure group |
| CTE + NTILE() | Customer value quartile analysis |
| Three-table JOIN | High-risk segment profiling |
| UNION ALL | Segment vs overall comparison |

### Key SQL Findings

**Churn Rate by Contract Type**
| Contract | Customers | Churned | Churn Rate |
|----------|-----------|---------|------------|
| Month-to-month | 3,875 | 1,655 | 42.71% |
| One year | 1,473 | 166 | 11.27% |
| Two year | 1,695 | 48 | 2.83% |

**Churn Rate by Payment Method**
| Payment Method | Churn Rate | Avg Monthly Charge |
|---------------|------------|-------------------|
| Electronic check | 45.29% | $76.26 |
| Mailed check | 19.11% | $43.92 |
| Bank transfer (automatic) | 16.71% | $67.19 |
| Credit card (automatic) | 15.24% | $66.51 |

**Churn Rate by Tenure Cohort**
| Tenure | Churn Rate |
|--------|------------|
| 0-12 months | 47.44% |
| 13-24 months | 28.71% |
| 25-48 months | 20.39% |
| 49+ months | 9.51% |

**High-Risk Segment vs Overall**
| Segment | Customers | Churn Rate | Avg Tenure | Avg Monthly Charge |
|---------|-----------|------------|------------|--------------------|
| High-Risk Segment | 1,138 | 62.92% | 18.1 months | $85.91 |
| Overall Customer Base | 7,043 | 26.54% | 32.4 months | $64.76 |

---

## Part 2: Machine Learning

### Feature Engineering
- Binary columns (Yes/No) encoded as 1/0
- Multi-category columns one-hot encoded
- New feature created: `avg_monthly_spend_ratio = MonthlyCharges / (tenure + 1)`
- Train/test split: 80/20, stratified by churn label

### Model Comparison

| Model | AUC Score |
|-------|-----------|
| Logistic Regression | **0.8467** |
| XGBoost | 0.8212 |
| Random Forest | 0.8207 |

### Best Model Performance (Logistic Regression)

| Metric | Retained | Churned |
|--------|----------|---------|
| Precision | 0.84 | 0.67 |
| Recall | 0.91 | 0.52 |
| F1-score | 0.87 | 0.59 |
| Accuracy (overall) | | 80% |

### Confusion Matrix
![Confusion Matrix](https://raw.githubusercontent.com/Rominabh/telco-customer-churn/main/confusion_matrix.png)

### Feature Importance
![Feature Importance](https://raw.githubusercontent.com/Rominabh/telco-customer-churn/main/feature_importance.png)

### Top Features Influencing Churn

Positive coefficients (increase churn risk):
- `InternetService_Fiber optic` (strongest predictor)
- `Contract_Month-to-month`
- `avg_monthly_spend_ratio`
- `PaymentMethod_Electronic check`

Negative coefficients (reduce churn risk):
- `MonthlyCharges` (strongest retention factor)
- `tenure`
- `InternetService_No`
- `Contract_Two year`

---

## Business KPIs

| KPI | Value |
|-----|-------|
| Overall Churn Rate | 26.54% |
| Monthly Revenue Lost | $139,130 |
| High-Risk Active Customers | 422 |
| Avg Tenure (Churned) | 18 months |
| Avg Tenure (Retained) | 37.6 months |
| Monthly Revenue at Risk | $36,410 |

---

## Business Recommendations

1. **Prioritize long-term contract incentives** — month-to-month customers churn at 15x the rate of two-year customers. Offering discounts for annual or biennial contracts could significantly reduce churn.

2. **Target the 422 high-risk active customers** — these customers share the highest-risk profile and represent $36,410 in monthly revenue at risk. A targeted retention campaign for this group would have an immediate financial impact.

3. **Promote TechSupport add-on for Fiber optic customers** — Fiber optic customers without TechSupport churn at 49.37%, dropping to 22.63% with TechSupport. Bundling or discounting TechSupport for this segment could cut churn nearly in half.

4. **Address early-stage churn** — nearly half of customers in their first year (47.44%) churn. An onboarding program or first-year loyalty offer could help retain this high-risk group.

---

## How to Run

1. Clone this repository
2. Open `notebooks/telco_churn_analysis.ipynb` in Google Colab
3. Upload `data/telco_churn.csv` when prompted
4. Run all cells in order
5. Open `dashboard/telco_churn_dashboard.pbix` in Power BI Desktop
