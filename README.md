
# 💳 Retail Banking Transaction Analytics & Risk Rules Engine

Welcome to the **Banking Transaction Analytics** portfolio project! In this project, I stepped into the role of a **Junior Analyst in a Data & Risk team** at a retail bank.

The objective was twofold: first, analyze customer behavior and transaction trends from a fictional retail banking database; second, engineer an automated, table-driven risk-rules engine to detect suspicious and potentially fraudulent transactions.

---

## 📊 Dataset Overview

* **Database Name:** `BankingAnalyticsDB`
* **Scope:** Full year of 2025 transactions
* **Scale:** 600 customers, 16,132 transactions
* **Currency:** Saudi Riyal (SAR)
* **Environment:** T-SQL (Designed for SQL Server 2017+)

---

## 🗂️ Project Phases & Architecture

### Phase 1: Setup & Data Quality Checks (`01_data_quality_checks.sql`)

* **Objective:** Verify data integrity and trustworthiness before diving into analytics.
* **Key Tasks:**
* Calculated total counts for customers, accounts, and transactions, alongside the overall date range.
* Inspected categorical columns (Transaction Types, Channels, Countries, Merchant Categories, and Account Types).
* Scanned for `NULL` values, duplicate IDs, and structural **orphans** (accounts without a customer, transactions without an account).
* Uncovered critical data quality anomalies—such as 8 transactions dated *before* their respective accounts were opened.



### Phase 2: Business Overview (`02_business_overview.sql`)

* **Objective:** Map out core banking volumes, operational channels, and temporal spending patterns.
* **Key Tasks:**
* Analyzed transaction counts and total monetary volumes grouped by transaction type (Purchases, Withdrawals, Transfers, Deposits) and channel.
* Computed monthly transaction trends and Month-over-Month (MoM) percentage changes using window functions (`LAG`).
* Identified peak business hours and busiest weekdays.
* Evaluated geographic distribution (including international transactions outside Saudi Arabia) and top merchant spending categories, alongside average monthly salary deposits.



### Phase 3: Customer Analysis (`03_customer_analysis.sql`)

* **Objective:** Segment the customer base to understand lifetime value, engagement tiers, and churn risk.
* **Key Tasks:**
* Identified top-spending customers and established three value tiers (High, Medium, Low) using `NTILE` to evaluate Pareto spending distributions.
* Developed an **RFM-lite** model (Recency, Frequency, Monetary scoring) to categorize customers into strategic segments (*Champions*, *Loyal*, *At Risk*, and *Dormant*).
* Tracked account inactivity across 90-day and 180-day windows as of December 31, 2025.



### Phase 4: Risk & Fraud Rules Engine (`04_risk_rules.sql`)

* **Objective:** Build a modular, automated T-SQL risk scoring pipeline using physical tables and stored procedures.
* **Architecture:**
* **Rule Staging Tables (`Banking.Rule_*`):** Individual physical tables capturing outputs for each risk rule.
* **Master Alerts Table (`Banking.Fraud_Alerts`):** Consolidated log mapping transactions to triggered rules and assigned risk points.
* **Scoring Table (`Banking.Fraud_Scores`):** Aggregates risk points per transaction, assigns risk levels (*High*, *Medium*, *Low*), and concatenates triggered rule names.
* **Execution Engine (`Banking.usp_Run_Fraud_Rules`):** A stored procedure that truncates and re-runs the entire rules pipeline cleanly.


* **Implemented Rules:**
1. **Velocity (30 pts):** 3 consecutive transactions on the same account within 10 minutes.
2. **AmountSpike (25 pts):** Outgoing transaction exceeding 5x the account's historical average for that type (requiring $\ge 5$ prior transactions).
3. **Structuring (40 pts):** 3 deposits between 9,000 and 9,999 SAR within a 7-day window (evading reporting thresholds).
4. **ImpossibleTravel (50 pts):** Transactions in different countries within 60 minutes of each other.
5. **DormantSpike (35 pts):** Transaction $> 5,000$ SAR on an account dormant for over 6 months.
6. **NightActivity (15 pts):** Outgoing transaction $\ge 1,000$ SAR between 01:00 and 04:59.



---

## 🚀 Getting Started & Running the Scripts

1. Clone or download this repository.
2. Run the initialization script to set up the database structure and seed data.
3. Execute the analytical and rule scripts in sequential order:
```sql
-- 1. Data Quality Checks
-- 2. Business Overview
-- 3. Customer Analysis
-- 4. Risk Rules & Stored Procedure Execution
EXEC Banking.usp_Run_Fraud_Rules;

```



---

*Project built as part of a hands-on Data & Risk Analytics portfolio.*
