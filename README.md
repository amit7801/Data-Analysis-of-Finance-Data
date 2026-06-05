# 🏦 Banking Dashboard — Power BI Project

A Power BI analytics project that delivers interactive dashboards for banking risk analytics, loan analysis, and deposit tracking across 3,000 client records. The dashboards help financial institutions assess client profiles, manage lending risk, and monitor key financial KPIs in real time.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Project Structure](#project-structure)
- [Dataset](#dataset)
- [Data Model & Relationships](#data-model--relationships)
- [Data Cleaning & Derived Columns](#data-cleaning--derived-columns)
- [DAX Measures & KPIs](#dax-measures--kpis)
- [Dashboard Pages](#dashboard-pages)
- [Key Insights](#key-insights)
- [Tools Used](#tools-used)

---

## Overview

**Problem Statement:** Develop a foundational understanding of risk analytics in banking and financial services, and demonstrate how data can be used to minimise the risk of lending to high-risk customers.

**Solution:** Power BI dashboards that allow bank managers and analysts to evaluate a client's profile — including loans, deposits, income, nationality, and loyalty tier — to make informed lending decisions.

---

## Project Structure

```
├── Banking_Dashboard.pbix          # Original Power BI dashboard
├── Banking_Dashboard_(2025).pbix   # Updated 2025 version of the dashboard
├── Banking.csv                     # Primary client-banking dataset (raw)
├── Banking.xlsx                    # Primary client-banking dataset (Excel format)
├── clients.csv                     # Gender lookup table
├── Banking_Report.docx             # Full project documentation & DAX reference
└── Banking.pptx                    # Project presentation with KPI highlights
```

---

## Dataset

### `Banking.csv` / `Banking.xlsx` — Primary Dataset
**3,000 client records | 25 columns**

| Column | Description |
|---|---|
| `Client ID` | Unique client identifier (e.g. `IND81288`) |
| `Name` | Client full name |
| `Age` | Client age |
| `Location ID` | Branch/location reference |
| `Joined Bank` | Date the client joined |
| `Banking Contact` | Assigned banking advisor |
| `Nationality` | American, African, European, Asian, Australian |
| `Occupation` | Client's job title |
| `Fee Structure` | High / Mid / Low |
| `Loyalty Classification` | Jade / Gold / Silver / Platinum |
| `Estimated Income` | Annual income estimate (USD) |
| `Superannuation Savings` | Retirement savings balance |
| `Amount of Credit Cards` | Number of credit cards held |
| `Credit Card Balance` | Outstanding credit card balance |
| `Bank Loans` | Outstanding bank loan amount |
| `Bank Deposits` | Total deposits held at the bank |
| `Checking Accounts` | Checking account balance |
| `Saving Accounts` | Savings account balance |
| `Foreign Currency Account` | Foreign currency holdings |
| `Business Lending` | Business loan amount |
| `Properties Owned` | Number of properties owned |
| `Risk Weighting` | Client risk score |
| `BRId` | Banking Relationship ID (foreign key) |
| `GenderId` | Gender lookup key (foreign key) |
| `IAId` | Investment Advisor ID (foreign key) |

**Key statistics:**
- Estimated Income range: $15,919 – $522,330 (avg ~$171,305)
- Bank Loans range: $0 – $2.67M (avg ~$591,386)
- Business Lending range: $0 – $3.83M (avg ~$866,760)

### `clients.csv` — Gender Lookup
Maps `GenderId` to gender labels (Male / Female).

---

## Data Model & Relationships

The data model is built across multiple related tables:

| Table | Description |
|---|---|
| Clients - Banking | Primary fact table (Banking.csv) |
| Gender | Lookup via `GenderId` |
| Banking Relationship | Lookup via `BRId` |
| Investment Advisor | Lookup via `IAId` |
| Period | Date/time dimension table |

Tables are linked through primary and foreign key relationships to enable cross-filtering across the dashboards.

---

## Data Cleaning & Derived Columns

The following transformations were applied in Power BI:

| Column | Transformation |
|---|---|
| `Engagement Timeframe` | Categorises how long a client has been with the bank |
| `Engagement Days` | `DATEDIFF(Joined Bank, TODAY(), DAY)` — days since joining |
| `Income Band` | Bins `Estimated Income`: Low (< $100K), Mid (< $300K), High (≥ $300K) |
| `Processing Fees` | Derived from `Fee Structure`: High → 0.05, Mid → 0.03, Low → 0.01 |

---

## DAX Measures & KPIs

### Core KPIs

| Measure | DAX Formula | Description |
|---|---|---|
| Total Clients | `DISTINCTCOUNT('Clients - Banking'[Client ID])` | Unique client count |
| Bank Loan | `SUM('Clients - Banking'[Bank Loans])` | Total outstanding bank loans |
| Business Lending | `SUM('Clients - Banking'[Business Lending])` | Total business loans |
| Credit Cards Balance | `SUM('Clients - Banking'[Credit Card Balance])` | Total CC debt |
| **Total Loan** | `[Bank Loan] + [Business Lending] + [Credit Cards Balance]` | Combined lending exposure |
| Bank Deposit | `SUM('Clients - Banking'[Bank Deposits])` | Total bank deposits |
| Savings Account | `SUM('Clients - Banking'[Saving Accounts])` | Total savings balances |
| Checking Accounts | `SUM('Clients - Banking'[Checking Accounts])` | Total checking balances |
| Foreign Currency Account | `SUM('Clients - Banking'[Foreign Currency Account])` | FX holdings |
| **Total Deposit** | `[Bank Deposit] + [Savings Account] + [Foreign Currency Account] + [Checking Accounts]` | Combined deposits |
| **Total Fees** | `SUMX('Clients - Banking', [Total Loan] * 'Clients - Banking'[Processing Fees])` | Revenue from processing fees |
| Engagement Length | `SUM('Clients - Banking'[Engagement Days])` | Total engagement days across clients |

### Highlighted Figures (from presentation)

| KPI | Value |
|---|---|
| Total Clients | 188 |
| Total Loan | $139.9M |
| Total Deposit | $111.49M |
| Total Fees | $5.17M |
| Bank Loan | $55.5M |
| Bank Deposit | $60.18M |
| Business Lending | $83.52M |
| Savings Account | $19.85M |
| Foreign Currency | $2.71M |

---

## Dashboard Pages

### 🏠 Home
Overview landing page with high-level KPI cards and navigation to detailed views.

### 💳 Loan Analysis
Breaks down total lending by income band, nationality, contract type, and loyalty classification. Enables risk assessment per client segment.

### 🏦 Deposit Analysis
Visualises deposit distribution across checking, savings, foreign currency, and bank deposit accounts segmented by gender, nationality, and advisor.

### 📊 Summary Dashboard
Consolidated view combining loan and deposit metrics with filters for nationality, income band, fee structure, and time period.

---

## Key Insights

- **Bank loans are highest for the Mid income band** and lowest for the High income band
- **European clients carry the highest bank loans**; Australian clients the lowest
- **Private banks have more clients** than other banking types — a strategic opportunity for competitors
- Clients on high fee structures tend to have greater overall lending exposure
- **Nationality breakdown by loan volume:** European > Asian > American > African > Australian

---

## Tools Used

| Tool | Purpose |
|---|---|
| Power BI Desktop | Dashboard development & DAX calculations |
| Microsoft Excel / CSV | Raw data storage & preparation |
| DAX | KPI measures, calculated columns, aggregations |
| PowerPoint | Project presentation |
| Word | Technical documentation |
