# Telco Customer Churn Analysis & Prediction

**Tools:** Python · SQL (SQLite) · Power BI  
**Data:** IBM Telco Customer Churn (7,043 customers)

## Goal

Understand why telecom customers cancel their subscriptions and predict who is most likely to leave next.

## Key findings

- **Contract type and tenure matter most.** Month-to-month customers churn at 42.7%, compared to 2.8% on two-year contracts. Nearly half of customers in their first year (47.4%) leave.
- **One segment is extremely high-risk.** Month-to-month + Fiber optic + no tech support + electronic check: 62.9% churn, more than double the 26.5% average.
- **Financial impact:** $139K monthly revenue lost to churn, and $36K at risk from 422 high-risk customers still active.

## Approach

1. **Cleaning (Python):** fixed TotalCharges data type and 11 missing values for zero-tenure customers.
2. **SQL analysis:** split the data into three related tables and explored churn patterns with joins and grouped queries.
3. **KPIs:** churn rate, revenue lost, revenue at risk and tenure, calculated in SQL for the dashboard.
4. **Modelling:** compared Logistic Regression, Random Forest and XGBoost with 5 fold cross validation. All three reach a similar AUC (about 0.85), so I chose Logistic Regression for its interpretability. Overlapping charge columns were removed to keep the coefficients reliable.
5. **Threshold:** lowering the decision threshold from 50% to 30% raises the share of churners caught from 55% to 74%.
6. **Dashboard (Power BI):** business overview and model results.

## Dashboard

![Churn Overview](https://raw.githubusercontent.com/Rominabh/telco-customer-churn/main/dashboard_page1.png)
![Model Insights](https://raw.githubusercontent.com/Rominabh/telco-customer-churn/main/dashboard_page2.png)

## Recommendations

1. **Encourage longer contracts** with a discount for switching to an annual plan.
2. **Focus on the first year** with onboarding or a first-year loyalty offer.
3. **Bundle tech support for Fiber customers:** churn drops from 49.4% to 22.6% with it.
4. **Target the 422 high-risk active customers** with a retention campaign.

## Model performance

| Model | Cross validated AUC |
|-------|-----|
| XGBoost (tuned) | 0.850 |
| Random Forest (tuned) | 0.848 |
| Logistic Regression | 0.845 |

Logistic Regression on the test set: AUC 0.839, accuracy 80%.
