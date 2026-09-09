# Customer-Churn-Analytics-in-SaaS-Industry

End-to-end analysis of ~95,000 SaaS accounts and ~3,500 exit interviews: clean dirty data, find who is at risk, classify why they left, and turn that into a 4-page Power BI story with three actions.

**Published dashboard figures (use these on slides and resume):**  
94,687 accounts · 19,676 churned · **20.78%** churn · **₹8.48M** churned ARR

Notebook rate on all known churn flags is 20.91% on 94,419. Power Query drops extra incomplete rows before the model. This repo treats **20.78%** as the official number.

GitHub: https://github.com/satya-suraj-pedada-ds/Customer-Churn-Analytics-in-SaaS-Industry

## Why this project exists

Management saw customers leaving. The brief was:

1. Clean messy structured + text data
2. Find high-risk groups (plan, tenure, tickets, region)
3. Classify exit interviews into reason + mood
4. Show the story in Power BI
5. Give three actions that a retention team can run next week

This is a junior data analyst project: cleaning, metrics, segments, text labels, dashboard, write-up. It is not a production ML product.

## Headline findings (dashboard)

| Finding | Number |
|---|---|
| Company churn | **20.78%** on 94,687 accounts |
| Lost annual spend | **₹8.48M** |
| Highest-rate plans | Basic 22.11% · Standard 22.10% |
| Lowest-rate plan | Enterprise 15.08% |
| Largest lost ARR | Premium (~₹3.53M) even with a lower rate |
| Highest-risk tenure | 0–6 months **26.12%**; later buckets sit near 21% |
| Tickets | 0–3 tickets ~21%; **4+ tickets ~30%** |
| Region | Almost flat (20.5–21.1%) — not the driver |
| Top exit reasons | **Value 40.88%** · **Price 40.39%** · Support 10.69% |
| Mood | **Frustrated 65.74%** · Disappointed 21.19% |

Price = “too expensive.” Value = “not worth it / no ROI.” They are almost the same size. A coupon does not fix Value.
...


## Three actions

1. Save list first: 0–6 month accounts with **4+ support tickets** (onboarding + 48-hour reply SLA).
2. Split Price vs Value. Discount only Price. For Value, show usage and ROI, not a coupon.
3. Weekly queue: every account that crosses 4 tickets, regardless of tenure.

## Repo layout

```text
Customer-Churn-Analytics-in-SaaS-Industry/
├── README.md
├── Notebook/
│   └── Customer Churn Analytics in SaaS Industry.ipynb
├── data/
│   ├── raw/                  # dirty source files
│   │   ├── churn_customers_95k_dirty.csv
│   │   └── churn_exit_interviews_3500_dirty.csv
│   └── processed/            # notebook exports used in Power BI
│       ├── customer_clean.csv
│       └── interviews_classified.csv
├── Power BI/
│   └── Customer Churn Analytics in SaaS Industry.pbix
└── BI Dashboard Images/
    ├── Dashboard-1 SaaS Churn Overview.jpg
    ├── Dashboard-2 Who Is High Risk.jpg
    ├── Dashboard-3 Why Customers Leave.jpg
    └── Dashboard-4 Mood Of Departing Customers.jpg

## Data
Customers (dirty ~95k)

Customer_ID, Age, Gender, Region, Tenure_Months, Subscription_Type, Monthly_Spend, Support_Tickets, Churn_Flag, Signup_Date
Problems in the raw file: mixed churn labels (0/1/Yes/No), duplicate rows and IDs, invalid ages, negative tenure and tickets, plan aliases (mid, STD, gold), region aliases (Nrt, Ctr), currency symbols inside spend, blank churn flags.
Exit interviews (dirty ~3.5k)

Customer_ID, Churn_Flag, Exit_Reason_Text
Problems: HTML tags, emojis, truncated lines, N/A / NULL, mixed ID prefixes (XC…).
Clean outputs

customer_clean.csv — standardized labels, Annual_Spend, Tenure_Group, Signup_Year
interviews_classified.csv — cleaned text, usable, primary_reason, mood, reason flags

Churn_Flag blanks were not imputed. Rate is calculated only on known flags.
