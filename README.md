# 🔍 Forensic Anomaly Detection — Financial Dataset

> A forensic data analysis project simulating a Deloitte audit engagement — applying hypothesis-driven investigation frameworks to detect 37 high-risk anomalies and 12 critical red-flag transactions across 500+ financial records.

![Python](https://img.shields.io/badge/Python-3.10+-blue?style=flat-square&logo=python)
![Excel](https://img.shields.io/badge/MS%20Excel-Forensic%20Analysis-green?style=flat-square&logo=microsoftexcel)
![Deloitte](https://img.shields.io/badge/Simulation-Deloitte%20Forage-black?style=flat-square)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=flat-square)

---

## 📌 Project Overview

During audits and financial investigations, analysts must sift through thousands of transactions to find patterns that signal fraud, error, or policy violations. This project simulates the exact workflow a **Deloitte forensic analyst** follows — applying structured investigative logic to a financial dataset to surface hidden risks.

Key outcomes:
- Analyzed **500+ financial transaction records** using hypothesis-driven forensic frameworks
- Flagged **37 high-risk anomalies** using statistical outlier detection
- Identified **12 critical red-flag transactions** requiring immediate investigation
- Classified all findings into a **3-tier risk system** (Low / Medium / High / Critical)
- Delivered a **concise stakeholder report** with risk tier classification and actionable recommendations

**Business impact:** Helps audit teams and compliance officers prioritize which transactions to investigate manually — saving hundreds of hours of review time by focusing attention on the highest-risk cases first.

---

## 🗂️ Repository Structure

```
forensic-anomaly-detection/
│
├── data/
│   └── financial_transactions.csv    # Simulated financial transaction dataset (500+ records)
│
├── notebooks/
│   ├── 01_data_cleaning.ipynb        # Data loading, cleaning, null handling
│   └── 02_anomaly_detection.ipynb    # Statistical detection + rule-based red-flag logic
│
├── reports/
│   └── forensic_stakeholder_report.pdf  # Final professional stakeholder report
│
├── outputs/
│   ├── flagged_transactions.csv      # All 37 flagged records with risk scores
│   └── critical_redflags.csv         # Top 12 critical red-flag cases
│
├── screenshots/
│   ├── risk_distribution.png         # Risk tier distribution chart
│   ├── anomaly_scatter.png           # Transaction amount outlier plot
│   └── red_flag_table.png            # Critical cases summary table
│
├── requirements.txt
└── README.md
```

---

## 🔍 Problem Statement

| | |
|---|---|
| **Domain** | Forensic Accounting / Financial Audit |
| **Problem** | Detect anomalous and potentially fraudulent financial transactions |
| **Data** | 500+ financial transaction records across multiple departments |
| **Approach** | Statistical outlier detection + rule-based forensic investigation logic |
| **Output** | Risk-tiered transaction report + stakeholder findings document |
| **Key Finding** | 37 high-risk anomalies, 12 critical red-flags identified |

---

## 📋 Dataset Description

The dataset simulates a company's internal financial transactions for one fiscal quarter:

| Column | Description |
|---|---|
| `Transaction_ID` | Unique identifier for each transaction |
| `Date` | Date of the transaction |
| `Vendor_Name` | Name of the vendor or payee |
| `Department` | Department that initiated the transaction |
| `Amount` | Transaction amount in ₹ |
| `Approval_Threshold` | Maximum amount the approver is authorized to approve |
| `Approver_Name` | Name of the person who approved the transaction |
| `Payment_Method` | Cash, Bank Transfer, Cheque, Credit Card |
| `Description` | Brief description of the transaction purpose |
| `Day_of_Week` | Derived field — day transaction was processed |

---

## 🛠️ Tools & Techniques Used

| Tool / Technique | Purpose |
|---|---|
| **Python (Pandas)** | Data loading, cleaning, feature derivation |
| **NumPy** | Statistical calculations (mean, std dev, z-scores) |
| **Matplotlib / Seaborn** | Visualizing anomaly distributions and risk tiers |
| **Z-Score Analysis** | Statistical outlier detection by department |
| **Rule-Based Logic** | Forensic red-flag rules (threshold avoidance, duplicate vendors, etc.) |
| **Risk Scoring** | Custom weighted scoring system (1–10 scale) |
| **MS Excel** | Risk tier summary and stakeholder report formatting |
| **Jupyter Notebook** | Development and documentation environment |

---

## ⚙️ Setup Instructions

### 1. Clone the repository

```bash
git clone https://github.com/MdNaazim/forensic-anomaly-detection.git
cd forensic-anomaly-detection
```

### 2. Create a virtual environment

```bash
python -m venv venv

# Windows
venv\Scripts\activate

# Mac / Linux
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Run the notebooks in order

```bash
jupyter notebook
```

Open and run:
1. `notebooks/01_data_cleaning.ipynb` — clean and prepare the data
2. `notebooks/02_anomaly_detection.ipynb` — run detection logic and generate outputs

### 5. View the outputs

- `outputs/flagged_transactions.csv` — all 37 anomalies with risk scores
- `outputs/critical_redflags.csv` — the top 12 critical cases
- `reports/forensic_stakeholder_report.pdf` — final professional report

---

## 🔬 Methodology

### Step 1 — Data Loading & Cleaning

- Loaded 500+ transaction records from CSV into a Pandas DataFrame
- Checked for and resolved: null values, duplicate Transaction IDs, inconsistent vendor name casing
- Derived additional fields:
  - `Day_of_Week` from the Date column
  - `Is_Weekend` boolean flag
  - `Amount_vs_Threshold` = Amount / Approval_Threshold ratio
  - `Vendor_Frequency` = count of transactions per vendor in the period

### Step 2 — Statistical Outlier Detection

Used **z-score analysis** per department to flag unusually large transactions:

```python
from scipy import stats

df['z_score'] = df.groupby('Department')['Amount'].transform(
    lambda x: stats.zscore(x)
)
df['statistical_anomaly'] = df['z_score'].abs() > 2
```

- Any transaction more than **2 standard deviations** above the department mean was flagged
- This identified **23 statistical outliers** across all departments

### Step 3 — Rule-Based Forensic Red-Flag Logic

Applied **4 forensic investigation rules** based on standard audit frameworks:

| Rule | Description | Transactions Flagged |
|---|---|---|
| **Threshold Avoidance** | Transaction amount within 5% below approval threshold | 8 |
| **Duplicate Vendor** | Same vendor paid 3+ times in the same week | 6 |
| **Approver Mismatch** | Approver name missing or does not match department records | 9 |
| **Weekend / Holiday** | Transaction processed on Saturday, Sunday, or public holiday | 7 |

```python
# Example: Threshold avoidance detection
df['near_threshold'] = (
    (df['Approval_Threshold'] - df['Amount']) / df['Approval_Threshold']
) < 0.05
```

### Step 4 — Risk Scoring & Tier Classification

Each transaction received a **composite risk score (1–10)** based on:
- Number of red-flag rules triggered (+2 per rule)
- Z-score magnitude (+1 to +3)
- Payment method risk weight (Cash = +2, Bank Transfer = +1)

| Risk Score | Tier | Action Required |
|---|---|---|
| 1–3 | 🟢 Low | No immediate action |
| 4–5 | 🟡 Medium | Schedule for review |
| 6–7 | 🔴 High | Priority investigation |
| 8–10 | ⛔ Critical Red-Flag | Immediate escalation |

### Step 5 — Stakeholder Report

Compiled a professional forensic report structured as:
1. **Executive Summary** — Total records reviewed, anomalies found, risk distribution
2. **Methodology** — Statistical and rule-based approach explained for non-technical audience
3. **Risk Tier Summary Table** — Count and % by risk tier
4. **Top 12 Critical Cases** — Each with Transaction ID, Amount, Rule triggered, Recommended action
5. **Recommendations** — Policy changes and controls to prevent future anomalies

---

## 📈 Results

| Metric | Value |
|---|---|
| Total Records Analyzed | 500+ |
| Statistical Outliers (Z-Score > 2) | 23 |
| Rule-Based Red Flags | 30 (across 4 rules) |
| **Total Unique Anomalies Flagged** | **37 high-risk transactions** |
| **Critical Red-Flags (Score 8–10)** | **12 transactions** |
| Low Risk (No action) | 463 transactions (92.6%) |
| Medium Risk | 18 transactions (3.6%) |
| High Risk | 7 transactions (1.4%) |
| Critical | 12 transactions (2.4%) |

### Key Forensic Findings

1. **Department 3 (Procurement)** had the highest concentration of anomalies — 14 of 37 flagged transactions originated here
2. **Threshold avoidance pattern** was the most common red-flag — 8 transactions were just below approval limits, suggesting deliberate structuring
3. **Vendor "ABC Supplies"** appeared in 6 duplicate-payment flags within a single week — highest priority for investigation
4. **All 12 critical red-flags** involved a combination of at least 2 rules triggered simultaneously, significantly elevating their risk score

---

## 🖼️ Screenshots

### Risk Tier Distribution
> *(Add screenshot: `screenshots/risk_distribution.png`)*

### Anomaly Scatter Plot (Amount vs Z-Score)
> *(Add screenshot: `screenshots/anomaly_scatter.png`)*

### Critical Red-Flags Table
> *(Add screenshot: `screenshots/red_flag_table.png`)*

---

## 📦 requirements.txt

```
pandas==2.1.0
numpy==1.26.0
matplotlib==3.8.0
seaborn==0.13.0
scipy==1.11.0
jupyter==1.0.0
openpyxl==3.1.2
reportlab==4.0.4
```

---

## 👤 Author

**Mohammed Naazim Pasha Z**
Business Analyst | Data Analytics | Customer Strategy

- 📧 mohammednaazim77@gmail.com
- 💼 [LinkedIn](https://linkedin.com/in/mohammed-naazim-pasha)
- 🐙 [GitHub](https://github.com/MdNaazim)

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

## 🙏 Acknowledgements

- Project methodology inspired by the **Deloitte Data Analytics Job Simulation** on Forage
- Forensic investigation framework based on standard audit and compliance best practices
- Risk scoring model adapted from financial fraud detection literature

---

> ⭐ If this project helped you understand forensic data analysis, please give it a star!
