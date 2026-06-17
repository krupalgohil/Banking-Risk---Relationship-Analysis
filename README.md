# 🏦 FinSight360 — Banking Risk & Relationship Analytics Dashboard

> An end-to-end Power BI analytics project that profiles bank clients, lending exposure, and deposit behavior — built to support faster, more consistent risk and relationship decisions.

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-Data%20Modeling-orange?style=flat)

---

## 📖 Table of Contents

- [Overview](#-overview)
- [Problem Statement](#-problem-statement)
- [Dataset](#-dataset)
- [Tech Stack](#-tech-stack)
- [Data Preparation](#-data-preparation)
- [Key Measures (DAX)](#-key-measures-dax)
- [Dashboard Preview](#-dashboard-preview)
- [Key Insights](#-key-insights)
- [Project Structure](#-project-structure)
- [How to Use](#-how-to-use)
- [Future Scope](#-future-scope)

---

## 🔎 Overview

**FinSight360** turns raw client-banking records into an interactive Power BI dashboard. Instead of reviewing applicant profiles one at a time, a relationship manager or risk analyst can instantly see how loan exposure and deposit balances are distributed across banking relationship type, gender, nationality, income band, and occupation.

## 🎯 Problem Statement

Develop a working understanding of risk analytics in banking and financial services, and show how client and account data can be organized and visualized to minimize the risk of losing money while lending to customers.

## 🗂️ Dataset

The data is structured as a set of interlinked tables connected through primary and foreign keys:

| Table | Description |
|---|---|
| **Client – Banking** | Core client demographics, income, occupation, nationality, account balances, loans, deposits, and fee structure |
| **Banking Relationship** | Relationship category per client — Private Bank, Retail, Commercial, Institutional |
| **Gender** | Gender reference table |
| **Investment Advisor** | Advisor assigned to each client relationship |
| **Period** | Date/time reference table for time-based analysis |

~3,000 clients are represented across the model.

## 🛠️ Tech Stack

- **Power BI** — data modeling, DAX measures, and the interactive dashboard
- **MySQL** — source data storage
- **Python** (pandas, seaborn, matplotlib) — exploratory data analysis
- **DAX** — calculated columns and measures

## 🧹 Data Preparation

Several calculated columns were added to make the raw fields analysis-ready:

| Column | Purpose |
|---|---|
| `Engagement Days` | `DATEDIFF(Joined Bank, TODAY(), DAY)` — days since the client joined |
| `Engagement Timeframe` | Buckets Engagement Days into `< 1 Year`, `< 5 Years`, `< 10 Years`, `< 20 Years`, `> 20 Years` |
| `Income Band` | Buckets Estimated Income into `Low` (< 100k), `Mid` (< 300k), `High` |
| `Processing Fees` | Maps Fee Structure tier (`High`/`Mid`/`Low`) to a fee rate (0.05 / 0.03 / 0.01) |

## 📐 Key Measures (DAX)

| Measure | Formula |
|---|---|
| Total Clients | `DISTINCTCOUNT('Clients - Banking'[Client ID])` |
| Bank Loan | `SUM('Clients - Banking'[Bank Loans])` |
| Business Lending | `SUM('Clients - Banking'[Business Lending])` |
| Total Loan | `[Bank Loan] + [Business Lending] + [Credit Cards Balance]` |
| Bank Deposit | `SUM('Clients - Banking'[Bank Deposits])` |
| Total Deposit | `[Bank Deposit] + [Savings Account] + [Foreign Currency Account] + [Checking Accounts]` |
| Total Fees | `SUMX('Clients - Banking', [Total Loan] * [Processing Fees])` |
| Engagement Length | `SUM('Clients - Banking'[Engagement Days])` |

## 📊 Dashboard Preview

### 🏠 Home
Landing page with headline KPIs — total clients, total loan, total deposit, total fees, credit card amount, and savings — filterable by gender.

![Home Dashboard](Dashboard%20Photos/Home.png)

### 💳 Loan Analysis
Loan exposure broken down by banking relationship, income band, nationality, and occupation.

![Loan Analysis Dashboard](Dashboard%20Photos/Loan_Analysis.png)

### 🏦 Deposit Analysis
Deposit balances (bank, savings, checking, foreign currency) broken down the same way as the loan view.

![Deposit Analysis Dashboard](Dashboard%20Photos/Deposit_Analysis.png)

### 📋 Summary
A single scorecard view consolidating every loan and deposit KPI for a selected client segment.

![Summary Dashboard](Dashboard%20Photos/Summary.png)

## 💡 Key Insights

- **Private Bank** is the dominant relationship segment — ~\$0.81bn in loans and the largest share of deposits, well ahead of Retail, Commercial, and Institutional.
- **European** clients hold the largest share of both loans (~\$0.78bn) and deposits (~\$0.87bn), followed by Asian, American, Australian, and African clients.
- **Medium-income** clients make up just over half of both loan exposure (53%) and deposit value (54%).
- **Total deposits (\$2.0bn) exceed total loans (\$1.8bn)** across the client base — a deposit-heavy balance sheet position.
- EDA shows a **strong positive correlation** between Bank Deposits, Checking Accounts, Savings Accounts, and Foreign Currency Account balances — clients with a high balance in one account type tend to carry high balances across the others too.

## 📁 Project Structure

```
FinSight360/
├── Dashboard Photos/
│   ├── Home.png
│   ├── Loan_Analysis.png
│   ├── Deposit_Analysis.png
│   └── Summary.png
├── Banking Dataset.csv
├── Banking Dashboard.pbix
├── EDA.ipynb
├── FinSight360_Project_Report.docx
└── README.md
```

## 🚀 How to Use

1. Clone or download this repository.
2. Open `Banking Dashboard.pbix` in **Power BI Desktop** (free download from Microsoft).
3. If prompted, point the data source to your local copy of `Banking Dataset.csv` / MySQL instance.
4. Use the slicers (Banking Relationship, Gender, Investment Advisor) at the top of each page to filter the view.
5. To explore the underlying EDA, open `EDA.ipynb` in Jupyter Notebook with `pandas`, `seaborn`, and `matplotlib` installed.

## 🔭 Future Scope

- Add a credit-scoring measure combining income band, loyalty classification, and risk weighting into a single applicant risk score.
- Build a time-trend view using the Period table to track loan/deposit balances over time rather than as a single snapshot.
- Incorporate a default/delinquency flag to connect dashboard segments directly to loan performance outcomes.
- Use relationship-type insights (e.g. Private Bank's large loan book) to inform growth strategy for Retail and Commercial segments.

---

*Built with Power BI, MySQL, and Python as a hands-on exploration of risk analytics in banking.*
