# Customer Churn & RFM Segmentation: SQL, Python and Power BI

**Which customers are most valuable, and which are about to leave?**
An end-to-end analytics project that takes 1M+ raw wholesale transactions through SQL cleaning, Python segmentation and an interactive Power BI dashboard.

![SQL Server](https://img.shields.io/badge/SQL%20Server-T--SQL-CC2927?logo=microsoftsqlserver&logoColor=white)
![Python](https://img.shields.io/badge/Python-Pandas-3776AB?logo=python&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-DAX-F2C811?logo=powerbi&logoColor=black)

**Author:** Yasir Sheikh | Aspiring Data Analyst / BI Analyst

![Dashboard preview](images/01_overview_customer.png)

> **Disclaimer:** "IKEA Gifting" is a fictional brand name used only for this educational portfolio project. It is not affiliated with or endorsed by the real IKEA. All figures come from a public, anonymised retail dataset and do not reflect any real company's performance. The dashboard branding is for design and storytelling purposes only.

---

## At a Glance

| | |
|---|---|
| **Scenario** | A UK online wholesale gifting and home décor brand (fictional) selling to bulk buyers |
| **Problem** | Leadership had no structured view of customer value, so retention efforts were reactive instead of targeted |
| **What I built** | A cleaned SQL dataset, a Python RFM and churn model, and a multi-page Power BI dashboard |
| **Result** | 5,881 customers sorted into 6 segments, with a prioritised win-back list |

---

## Headline Results

| Metric | Value |
|---|---|
| Customers analysed | **5,881** |
| Total revenue | **$17.4M** |
| Churn rate (no order in 90+ days) | **50.9%** |
| Revenue from Champions (about 22% of customers) | **68.1%** |
| "At Risk" customers to win back | **823** (about $1.6M in past revenue) |

---

## My Approach

I worked backwards from the business decision rather than from the dataset. Before touching any data, I wrote down the questions the analysis had to answer, and every technical step traces back to one of them.

### Business Questions Answered

| # | Question | Where answered |
|---|---|---|
| 1 | Who are our most valuable customers, and how much revenue do they contribute? | Segment performance table |
| 2 | Which customers have gone quiet and are at risk of churning? | Churn flag and "At Risk" segment |
| 3 | How do customer value and retention change over time? | Month, quarter and year trend charts |
| 4 | How do value and churn differ across countries? | Country performance view |
| 5 | What action should the business take for each segment? | Recommendations below |

### Real Data Problems I Had to Solve

I used the raw data rather than a pre-cleaned version, so the project includes genuine quality issues:

| Problem | How I handled it |
|---|---|
| Mixed date formats across rows | `COALESCE` with two `TRY_CONVERT` styles, so no rows were dropped |
| Invalid price and type values | `TRY_CONVERT` to `DECIMAL`, plus a count of failed conversions |
| Missing customer IDs (guest checkouts) | Quantified, then excluded from customer-level analysis |
| Returns and cancellations (negative quantity) | Identified, then excluded from RFM and revenue |
| Exact duplicate transactions | Removed with `ROW_NUMBER()` in a CTE |

---

## Key Findings

1. **A small group drives most of the revenue.** Champions are roughly 22% of customers but generate about 68% of revenue, a clear 80/20-style concentration.
2. **Churn is a serious risk, not a minor one.** More than half of customers have been inactive for 90+ days.
3. **There is a clear win-back target.** 823 "At Risk" customers used to buy often and are now inactive.
4. **Growth stalled after 2010.** Active customers rose from 1.0K (2009) to 4.2K (2010), then stayed flat in 2011.
5. **Sales peak in Q4**, which fits a gifting-focused product range (Q4: 4,101 customers and $6.2M revenue).
6. **The UK is the core market**, with about 5,350 customers and roughly $14.4M in revenue.

---

## Recommended Actions

| Segment | Who they are | Suggested action |
|---|---|---|
| **Champions** | Recent, frequent, high spenders | Loyalty perks and early access to new ranges |
| **Loyal** | Steady repeat buyers | Upsell to move them toward Champions |
| **At Risk** | Used to buy often, now inactive | Personalised win-back discounts (top priority) |
| **Need Attention** | Buying less often | Reorder reminders and time-limited offers |
| **New** | Recent first-time buyers | Welcome offer to drive a second order |
| **Lost** | Inactive, low engagement | Low-cost email reactivation only |

---

## Project Workflow

```
Business questions → SQL Server → Python → Power BI
                     Clean & merge   Score & segment   Visualise & share
```

| Step | Tool | What I did |
|---|---|---|
| **1. Define** | Documentation | Wrote the business problem, questions and key definitions (RFM, churn) before any analysis |
| **2. Clean** | SQL Server | Imported both yearly sheets, fixed types and dates, ran exploratory checks, removed duplicates, saved `RetailCleaned` |
| **3. Analyse** | Python (Pandas) | Calculated Recency, Frequency and Monetary per customer, scored each 1 to 5, assigned segments and a 90-day churn flag |
| **4. Visualise** | Power BI + DAX | Merged results back onto transactions into one analysis-ready table and built the dashboard |

**What is RFM?** A standard way to rate customers on three questions: *How recently did they buy? How often? How much do they spend?*

---

## Dashboard

| Page | Contents |
|---|---|
| **Business Overview** | Headline KPIs (customers, revenue, churn %, average customer value), a Customer / Revenue toggle, segment breakdowns, and month, quarter and year trends |
| **RFM & Churn Deep-Dive** | Segment performance table (customers, revenue, average R/F/M score, churn %), country performance, average customer value rankings, and churn % over time |
| **Glossary** | Plain-language definitions of Recency, Frequency, Monetary and Churned % for non-technical readers |

**Usability features:** filters for Month, Quarter, Year, Country, Segment and Churn status, bookmark-based toggles, and a "Clear all slicers" button.

| Customer view | Revenue view |
|---|---|
| ![Customer view](Images/Power_Bi_Snapshot/Mockups/1.png) | ![Revenue view](images/02_overview_revenue.png) |

| Segment deep-dive | Country deep-dive |
|---|---|
| ![Segment view](images/03_deepdive_segment.png) | ![Country view](images/04_deepdive_country.png) |

---

## Skills Demonstrated

| Area | Skills |
|---|---|
| **SQL** | `UNION ALL`, CTEs, window functions, `TRY_CONVERT`, `COALESCE`, data quality checks |
| **Python** | Pandas, `groupby` aggregation, quantile scoring (`qcut`), rule-based segmentation |
| **Power BI** | Data modelling, DAX measures, bookmarks, conditional formatting, multi-page design |
| **Analytics** | RFM analysis, churn definition, customer segmentation, KPI design |
| **Communication** | Business requirements, plain-English insights, actionable recommendations |

---

## Code Samples

**SQL: handling mixed date formats**

```sql
SELECT
    Invoice, StockCode, Description, Quantity,
    COALESCE(
        TRY_CONVERT(datetime2, InvoiceDate, 120),
        TRY_CONVERT(datetime2, InvoiceDate, 105)
    ) AS InvoiceDate,
    TRY_CONVERT(decimal(10,2), Price) AS Price,
    Customer_ID, Country
INTO dbo.RetailCombined
FROM dbo.[Year 2009-2010];
```

**SQL: removing exact duplicates**

```sql
WITH Deduped AS (
    SELECT *,
           ROW_NUMBER() OVER (
               PARTITION BY Invoice, StockCode, Description, Quantity,
                            InvoiceDate, Price, Customer_ID, Country
               ORDER BY (SELECT NULL)) AS RowNum
    FROM dbo.RetailCombined
)
SELECT * INTO dbo.RetailCleaned FROM Deduped WHERE RowNum = 1;
```

**Python: RFM scoring**

```python
rfm['R_Score'] = pd.qcut(rfm['Recency'], 5, labels=[5, 4, 3, 2, 1]).astype(int)
rfm['F_Score'] = pd.qcut(rfm['Frequency'].rank(method='first'), 5, labels=[1, 2, 3, 4, 5]).astype(int)
rfm['M_Score'] = pd.qcut(rfm['Monetary'], 5, labels=[1, 2, 3, 4, 5]).astype(int)
rfm['IsChurned'] = rfm['Recency'] > 90
```

<details>
<summary><b>Segmentation rules and DAX measures (click to expand)</b></summary>

### Segmentation Rules (applied in this order)

| Segment | Rule (scores 1 to 5) | Customers |
|---|---|---:|
| Champions | R ≥ 4, F ≥ 4, M ≥ 4 | 1,289 |
| Loyal | R ≥ 3, F ≥ 3 | 1,416 |
| At Risk | R ≤ 2, F ≥ 3 | 823 |
| New | R ≥ 4, F ≤ 2 | 442 |
| Need Attention | R = 3, F ≤ 2 | 382 |
| Lost | Everyone else | 1,529 |

### Metric Definitions

| Metric | Definition |
|---|---|
| Recency | Days between the snapshot date (2011-12-10) and the last order |
| Frequency | Number of distinct invoices |
| Monetary | Sum of `Quantity × Price` |
| Churn | Recency greater than 90 days |

### Core DAX Measures

```DAX
Total Revenue      = SUM(Retail[Revenue])
Total Customers    = DISTINCTCOUNT(Retail[Customer_ID])
Churn Rate (%)     = DIVIDE(
                       CALCULATE(DISTINCTCOUNT(Retail[Customer_ID]), Retail[is_churn] = TRUE),
                       [Total Customers])
Avg Customer Value = DIVIDE([Total Revenue], [Total Customers])
```

### Limitations

- The 90-day churn rule is fixed; real buying cycles vary by customer.
- Churn is measured at one snapshot date, so earlier periods naturally show more churn.
- Score cut-offs are relative to this dataset.

</details>

---

## Data Source

**Online Retail II**, UCI Machine Learning Repository, donated by Daqing Chen. A real, anonymised transactional dataset from a UK-based online retailer of gift-ware, covering December 2009 to December 2011 with 1,067,371 raw rows across two sheets. Licensed under CC BY 4.0.

## Repository Structure

```
├── 01_sql/         data cleaning scripts
├── 02_python/      rfm_segmentation.ipynb
├── 03_powerbi/     dashboard (.pbix)
├── images/         dashboard screenshots
└── README.md
```

## How to Reproduce

1. Download the Online Retail II dataset from the UCI repository.
2. Run the scripts in `01_sql` to build the `RetailCleaned` table, then export it to CSV.
3. Run the notebook in `02_python` to generate the segmented dataset (Excel).
4. Open the file in `03_powerbi` and point it to the exported Excel file.

## Next Steps

- Predictive churn model (logistic regression or gradient boosting)
- Customer lifetime value (CLV) estimation
- Cohort retention analysis

## Acknowledgements

Dataset: Daqing Chen, UCI Machine Learning Repository. README structure inspired by Mansi Goel's end-to-end analytics tutorial series.

## Connect

[LinkedIn](https://www.linkedin.com/in/your-profile) · [GitHub](https://github.com/your-username) · [Email](mailto:your-email@example.com)
