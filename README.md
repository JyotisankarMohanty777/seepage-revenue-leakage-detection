# Seepage — Revenue Leakage Detection for Subscription Businesses

## The Problem

Subscription businesses lose an estimated 5-10% of revenue to "silent" billing errors — not fraud, just small cracks: customers billed on outdated pricing after a plan change, discounts that never expire, failed payments that quietly stop instead of retrying, and usage that doesn't match what's billed. Most companies don't discover this until a finance audit, months later.

**Seepage** finds these cracks automatically, quantifies exactly how much revenue is leaking, and flags new invoices that look likely to leak before they're finalized.

## Approach

1. **Data**: Generated a realistic synthetic dataset (SQL Server) — 5,000 customers, 87,614 invoices over ~2 years, with deliberately injected leakage patterns matching real-world scenarios.
2. **Detection (SQL)**: T-SQL query comparing each customer's contracted/expected billing amount against what was actually invoiced, categorizing every mismatch.
3. **Analysis (Python/Pandas)**: Quantified total leakage, broke it down by category, customer segment, and month.
4. **ML layer (Isolation Forest)**: Trained an anomaly-detection model as a second, independent signal — compared its findings against the SQL rules to understand where each method is strong/weak.
5. **Dashboard (Power BI)**: Built a finance-facing report — KPIs, trends, category/segment breakdowns, and a top-offenders table.

## Key Findings

- **₹86.4 lakh** in total detected revenue leakage across the dataset
- **13.83%** of invoices had some form of leakage
- **Failed payments with no retry** were the single largest category (₹59.6L) — more damaging than pricing/discount errors combined
- Leakage was fairly evenly distributed across customer segments (Enterprise 35%, Mid-Market 33%, SMB 32%) — not concentrated in one segment
- The Isolation Forest model caught 100% of failed-payment cases but only ~30-40% of subtler pricing/discount mismatches — reinforcing that rule-based detection is the reliable primary method, with ML adding value mainly for catching novel/unexpected patterns

## Tech Stack

SQL Server, Python (Pandas, NumPy, scikit-learn, Faker), Jupyter Notebook, Power BI

## Project Structure
​​
├── notebooks/       → Full analysis notebook
├── sql/             → Schema creation + leakage detection query
├── powerbi/         → Dashboard (.pbix)
└── README.md

## Power BI Report
<img width="1155" height="657" alt="Screenshot 2026-09-19 120526" src="https://github.com/user-attachments/assets/35cb3eb3-d356-4097-8e55-e1cbf063ca51" />




## Limitations

This project uses a synthetic dataset generated to reflect realistic patterns, not real company data. Leakage categories and injection rates were designed based on commonly reported industry patterns (5-10% of subscription revenue), not a specific company's actual data.

## Author

Jyotisankar Mohanty — jyotisankarmohanty777@gmail.com
