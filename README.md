# Telco Customer Churn Analysis & Prediction

**Tools:** Python · SQL (SQLite) · Power BI  
**Dataset:** IBM Telco Customer Churn · 7,043 customers  

---

## What is this project about?

Every month, telecom companies lose customers who cancel their subscriptions and this is called churn. The goal of this project is to understand *why* customers leave and *who* is most likely to leave next, using a real-world dataset of 7,043 customers.

The project covers the full data workflow: cleaning the data, exploring it with SQL, identifying high-risk customer groups, calculating business KPIs, building a predictive model, and presenting the results in a Power BI dashboard.

---

## What did I find?

A few patterns stood out clearly:

- **Contract type and tenure matter most.** Customers on month-to-month contracts churn at 42.7%, compared to just 2.8% for two-year contracts, and the model ranks tenure and contract type as the two strongest factors.
- **New customers are the most at risk.** Nearly half of customers in their first year (47.4%) end up leaving.
- **One group stands out as extremely high-risk.** Customers who combine a month-to-month contract, Fiber optic internet, no tech support, and electronic check payment churn at 62.9%, more than double the overall average of 26.5%.
- **The financial impact is significant.** This translates to $139K in monthly revenue already lost, and another $36K at risk from the 422 high-risk customers still active.

---

## How did I build it?

**Step 1: Data cleaning (Python)**  
Loaded the raw dataset, fixed a data type issue in the TotalCharges column, and handled 11 missing values caused by customers with zero tenure.

**Step 2: SQL analysis**  
Loaded the cleaned data into a SQLite database split into three related tables (customers, services, billing). Wrote six queries of increasing complexity to explore churn patterns, including CTEs, a window function (NTILE), and multi-table joins.

**Step 3: Business KPIs**  
Defined and calculated six KPIs directly from SQL: overall churn rate, high-risk customer count, revenue lost, revenue at risk, and average tenure for churned vs retained customers.

**Step 4: Machine learning (Python)**  
Engineered features and removed overlapping charge columns that made the model's coefficients misleading. Compared Logistic Regression, Random Forest, and XGBoost with 5 fold cross validation. With reasonable settings all three reach a similar AUC (about 0.85), so I chose Logistic Regression because its coefficients are easy to explain to a business audience. I also tested different decision thresholds: lowering it from 50% to 30% raises the share of churners caught from 55% to 74%.

**Step 5: Power BI dashboard**  
Built a two-page dashboard: one page for the business overview (KPI cards, churn by contract type, churn by tenure), and one page for model results (feature importance, actual vs predicted churn).

---

## Dashboard

### Page 1: Churn Overview
![Churn Overview](https://raw.githubusercontent.com/Rominabh/telco-customer-churn/main/dashboard_page1.png)

### Page 2: Model Insights
![Model Insights](https://raw.githubusercontent.com/Rominabh/telco-customer-churn/main/dashboard_page2.png)

---

## What would I recommend to the business?

1. **Offer incentives for longer contracts.** The churn gap between month-to-month and two-year contracts is enormous. Even a small discount for switching to an annual plan could have a big retention impact.

2. **Focus on the first year.** Nearly half of new customers churn within 12 months. A structured onboarding program or a first-year loyalty offer could significantly reduce early drop-off.

3. **Bundle TechSupport for Fiber optic customers.** Fiber customers without tech support churn at 49.4%. With tech support, that drops to 22.6%. Offering it as a free add-on for the first few months could make a measurable difference.

4. **Act on the 422 high-risk active customers now.** These customers represent $36K in monthly revenue at risk. A targeted retention campaign for this group is the highest-leverage action available.

---

## Model Performance

5 fold cross validated AUC on the training set:

| Model | AUC |
|-------|-----|
| XGBoost (tuned) | 0.850 |
| Random Forest (tuned) | 0.848 |
| Logistic Regression | 0.845 |

The differences are smaller than the variation between folds (about ±0.012), so the three models perform on par. Logistic Regression was chosen for interpretability. On the held out test set it reaches AUC 0.839 and 80% accuracy.

**Top churn drivers (Logistic Regression):** short tenure, month-to-month contract, Fiber optic internet, and electronic check payment increase churn risk; two year contracts, online security, and tech support reduce it.
