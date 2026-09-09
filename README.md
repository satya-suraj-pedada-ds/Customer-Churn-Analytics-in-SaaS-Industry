# Customer-Churn-Analytics-in-SaaS-Industry

End-to-end analysis of ~95,000 SaaS accounts and ~3,500 exit interviews: clean dirty data, find who is at risk, classify why they left, and turn that into a 4-page Power BI story with three actions.

**Published Dashboard Figures (use these on slides and resume):**  
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
```


## Data
### Customers (dirty ~95k)
Customer_ID, Age, Gender, Region, Tenure_Months, Subscription_Type, Monthly_Spend, Support_Tickets, Churn_Flag, Signup_Date
Problems in the raw file: mixed churn labels (0/1/Yes/No), duplicate rows and IDs, invalid ages, negative tenure and tickets, plan aliases (mid, STD, gold), region aliases (Nrt, Ctr), currency symbols inside spend, blank churn flags.

### Exit Interviews (dirty ~3.5k)
Customer_ID, Churn_Flag, Exit_Reason_Text
Problems: HTML tags, emojis, truncated lines, N/A / NULL, mixed ID prefixes (XC…).

### Clean Outputs
- customer_clean.csv — standardized labels, Annual_Spend, Tenure_Group, Signup_Year
- interviews_classified.csv — cleaned text, usable, primary_reason, mood, reason flags

Churn_Flag blanks were not imputed. Rate is calculated only on known flags.

## Method
1. Inspect raw row counts, missingness, duplicate IDs, raw churn labels.
2. Clean customers — drop exact duplicate rows, resolve colliding IDs, map gender/region/plan, clip age and tenure, set negative tickets to 0, parse mixed dates, leave blank churn as missing.
3. Clean text — strip HTML and junk tokens, mark short/empty replies as not usable.
4. Label interviews with a fixed keyword lexicon (not a trained LLM):
   - Reasons: Price, Value, Support, Product, Performance, Onboarding, Competitor, Unclassified
   - Moods: Angry, Frustrated, Disappointed, Neutral, Hopeful, PositiveShort or empty text → Unclassified / Neutral. No invented complaints.
5. Segments — churn rate and churned ARR by plan, tenure group, region, ticket count.
6. Optional baseline — logistic regression on plan / tenure / tickets to rank risk. This is a check, not the product.
7. Power BI — four pages, shared slicers (Plan, Region, Tenure, Signup Year).

## Power BI Pages

| Page | What it answers |
| --- | --- |
| 1 · SaaS Churn Overview | Size of the problem: 20.78%, ₹8.48M, stable by signup year, Premium holds the most lost ARR |
| 2 · Who Is High Risk? | Basic/Standard rate, 0–6 month spike, 4+ ticket jump, tenure × tickets grid |
| 3 · Why Customers Leave | Value ≈ Price as primary reason; Support is third |
| 4 · Mood of Departing Customers | Frustrated is ~2 in 3 labeled replies; Price and Value hold almost all Disappointed comments |

Screenshots live in BI Dashboard Images/.

## How to rerun
Python 3.10+ with pandas, numpy, matplotlib, scikit-learn. Power BI Desktop to open the .pbix.

1. Put the two dirty CSVs under data/raw/ (or edit the DATA path in the first notebook cell).
2. Open Notebook/Customer Churn Analytics in SaaS Industry.ipynb and Run All.
3. Clean files write to outputs/ (copy them into data/processed/ if you keep that folder).
4. Open the .pbix. If paths break: Transform data → Data source settings → Change Source.
5. Do not mix the notebook 20.91% print with dashboard cards. Published rate is 20.78%.

## Honest limits
- Interview labels are rules + keywords, not a fine-tuned language model. Say “lexicon classification,” not “AI detected emotion.”
- Many replies mention more than one theme. primary_reason is the first matched theme, not the only theme. Flag columns keep the rest.
- Competitor and Onboarding counts are small. Do not build a strategy only on n = 12.
- 0–6 months × 7 tickets is a thin cell. The save rule uses 4+ tickets, not one empty cell.
- Notebook and Power BI row counts differ by design (see the first paragraph).

## Resume line
Analyzed 95k SaaS accounts and 3.5k exit interviews; published 20.78% churn and ₹8.48M lost ARR; isolated 0–6 month + 4+ ticket risk and split Price vs Value so retention does not rely on blanket discounts.

## talk track 

Churn is 20.8% and flat by signup year — not a one-year spike.
Rate is highest on Basic/Standard and in month 0–6; money is highest on Premium.
Tickets stay near 21% until 4+, then the rate jumps.
Exit text is Value and Price, almost tied. Frustrated is the main mood.
Three actions: onboarding save list, Price vs Value playbooks, 4-ticket weekly queue.




