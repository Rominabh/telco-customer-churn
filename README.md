# Telco Customer Churn Analysis & Prediction

**Tools:** Python · SQL (SQLite) · Power BI  
**Dataset:** IBM Telco Customer Churn · 7,043 customers  

---

## What is this project about?

Every month, telecom companies lose customers who cancel their subscriptions — this is called churn. The goal of this project is to understand *why* customers leave and *who* is most likely to leave next, using a real-world dataset of 7,043 customers.

The project covers the full data workflow: cleaning the data, exploring it with SQL, identifying high-risk customer groups, calculating business KPIs, building a predictive model, and presenting the results in a Power BI dashboard.

---

## What did I find?

A few patterns stood out clearly:

- **Contract type matters most.** Customers on month-to-month contracts churn at 42.7%, compared to just 2.8% for two-year contracts.
- **New customers are the most at risk.** Nearly half of customers in their first year (47.4%) end up leaving.
- **One group stands out as extremely high-risk.** Customers who combine a month-to-month contract, Fiber optic internet, no tech support, and electronic check payment churn at 62.9% — more than double the overall average of 26.5%.
- **The financial impact is significant.** This translates to $139K in monthly revenue already lost, and another $36K at risk from the 422 high-risk customers still active.

---

## How did I build it?

**Step 1 — Data cleaning (Python)**  
Loaded the raw dataset, fixed a data type issue in the TotalCharges column, and handled 11 missing values caused by customers with zero tenure.

**Step 2 — SQL analysis**  
Loaded the cleaned data into a SQLite database split into three related tables (customers, services, billing). Wrote six queries of increasing complexity to explore churn patterns, including CTEs, window functions, and multi-table joins.

**Step 3 — Business KPIs**  
Defined and calculated six KPIs directly from SQL: overall churn rate, high-risk customer count, revenue lost, revenue at risk, and average tenure for churned vs retained customers.

**Step 4 — Machine learning (Python)**  
Engineered features, then compared three models: Logistic Regression, Random Forest, and XGBoost. Logistic Regression performed best with AUC 0.8467, suggesting the churn patterns in this dataset are largely linear.

**Step 5 — Power BI dashboard**  
Built a two-page dashboard: one page for the business overview (KPI cards, churn by contract type, churn by tenure), and one page for model results (feature importance, actual vs predicted churn).

---

## Dashboard

### Page 1 — Churn Overview
![Churn Overview](https://raw.githubusercontent.com/Rominabh/telco-customer-churn/main/dashboard_page1.png)

### Page 2 — Model Insights
![Model Insights](https://raw.githubusercontent.com/Rominabh/telco-customer-churn/main/dashboard_page2.png)

---

## What would I recommend to the business?

1. **Offer incentives for longer contracts.** The churn gap between month-to-month and two-year contracts is enormous. Even a small discount for switching to an annual plan could have a big retention impact.

2. **Focus on the first year.** Nearly half of new customers churn within 12 months. A structured onboarding program or a first-year loyalty offer could significantly reduce early drop-off.

3. **Bundle TechSupport for Fiber optic customers.** Fiber customers without tech support churn at 49.4%. With tech support, that drops to 22.6%. Offering it as a free add-on for the first few months could make a measurable difference.

4. **Act on the 422 high-risk active customers now.** These customers represent $36K in monthly revenue at risk. A targeted retention campaign for this group is the highest-leverage action available.

---

## Model Performance

| Model | AUC |
|-------|-----|
| Logistic Regression | **0.8467** |
| XGBoost | 0.8212 |
| Random Forest | 0.8207 |

---

## How to run this project

1. Clone this repository
2. Open `telco_churn_analysis.ipynb` in Google Colab
3. Upload `telco_churn.csv` when prompted
4. Run all cells in order
5. Open `telco_churn_dashboard.pbix` in Power BI Desktop
