# Churn Analysis and Customer Intelligence

An end-to-end churn analytics project for an OTT subscription platform. I pulled data from a relational SQLite database into Python, cleaned and engineered features, calculated 20+ KPIs, visualized the patterns, and turned the findings into business recommendations.

## Business Challenge

In the competitive OTT market (Uber,Netflix, Rogers, Bell), retention decides survival. The goal of this project is to identify high-risk subscribers using three data sources: customer demographics, subscription tiers and support escalations.

The analysis answers three questions:

- **Who** left, or is likely to leave?
- **Why** did they leave, based on their earlier behavior?
- **When** is the "danger zone" in which users cancel?

For this project, churn means a subscription with a cancellation date.

## Tech Stack

| Area | Tools |
|---|---|
| Language | Python |
| Database | SQLite (`sqlite3`) |
| Data work | `pandas`, `numpy` |
| Visualization | `matplotlib`, `seaborn` |
| Environment | Jupyter Notebook |

## Database

Database: `customer_churn`

| Table | Columns |
|---|---|
| `db_customer` | customerid, name, country, state, gender, dob, interests, pincode |
| `db_subscription` | customerid, subscription_start_date, subscription_type, renewal_date, plan_type, contract_type, cancellation_date, cancellation_reason, monthly_charges, cltv, churn_score |
| `db_support` | customerid, complaint_date, escalations, csat_score, comment |

The tables are joined on `customerid`.

## Project Workflow

1. **Connect and import:** connect Python to SQLite and load every table into a pandas DataFrame with SQL queries.
2. **Data cleaning:**
   - Drop unused columns (`interests`, `pincode`) and rename `name` to `customer_name`.
   - Convert date columns to datetime.
   - Standardize gender values (Men to Male, Women to Female).
   - Fill missing countries using a state-to-country mapping.
   - Remove unused columns from the support table.
3. **Feature engineering:**
   - `churn_flag` (1 if a cancellation date exists).
   - `complaint_count` per customer.
   - Customer `age` and `tenure_days`.
   - `churn_risk` tier (low below 50, med from 50 to 69, high from 70) based on `churn_score`.
4. **Analysis:** aggregation, group by and pivot tables for each KPI.
5. **Visualization:** matplotlib and seaborn charts (see below).
6. **Insights:** findings and recommended actions.

## KPIs Calculated

| KPI | Formula |
|---|---|
| Churn rate | Churned customers / total customers |
| Retention rate | 1 - churn rate |
| Churn by plan type | Churn rate by `plan_type` (Basic, Standard, Premium) |
| Churn by state | Churn rate by country and state |
| ARPU | Sum of monthly charges / active customers |
| Average tenure | Average days from start date to cancellation date (or today) |
| Revenue at risk | Sum of monthly charges for high-risk or churned customers |
| Escalation rate | Escalations / complaints x 100 |
| Complaints per customer | Total complaints / distinct customers |
| Escalations vs churn | Correlation between escalations and churn flag |

## Visualizations

- Monthly churn trend (line chart)
- Churn rate by plan type and by state (bar charts)
- Correlation heatmap across plan, contract, churn score, escalations and risk tier
- Pair plot and category plots of plan type, monthly charges, gender and risk tier

## Key Insights

- **Churn rate is 28.6%** and retention rate is 71.4%.
- **Contract type is the strongest driver:** monthly-contract subscribers churn at **55.6%**, compared with **8.3%** for annual contracts, which is 6.7 times higher.
- **Most churn comes from the Basic plan**, so the direct revenue impact is limited.
- **Churn peaked in September 2024**, and Karnataka is the most affected state.
- **Average tenure is 1,451 days** and ARPU is 18.8.
- **Revenue lost to churn is 74 out of 395 (about 18%)**, and CLTV lost is 2,047.

## Recommended Actions

1. Investigate Karnataka for price increases, user complaints or technical issues.
2. Review any changes to the Basic plan, especially around September.
3. Benchmark competitor offers, since at least one user switched to a competitor.
4. Prioritize High and Medium risk customers by CLTV, review their complaints, and reach out by email, SMS or call.
5. Promote migration from monthly to annual contracts.

## Repository Structure

```
.
├── Churn_Analysis.ipynb       # Full analysis notebook
├── customer_churn.db          # SQLite database
├── exported_churn_data.csv    # Cleaned, merged dataset
└── README.md
```

## How to Run

```bash
# 1. Clone the repository
git clone https://github.com/dhimandipanshi/Churn_Analysis.git
cd Churn_Analysis

# 2. Install dependencies
pip install numpy pandas matplotlib seaborn jupyter

# 3. Open the notebook
jupyter notebook
```

Make sure `customer_churn.db` is in the same folder as the notebook.

## Author

**[Your Name]**, Data Analyst

- LinkedIn: [[your-link](https://www.linkedin.com/in/dipanshidhiman/)]
- GitHub: [dhimandipanshi](https://github.com/dhimandipanshi)
