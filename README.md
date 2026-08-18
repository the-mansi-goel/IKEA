# IKEA Gifting: Customer Analytics and RFM Segmentation

*An end to end data analytics project using SQL, Python, and Power BI*

**Author:** Mansi Goel | YouTube: [Mansi G.](https://www.youtube.com/@Mansi.goel.offical) | Analytics Engineer

---

## Disclaimer

**IKEA Gifting** is a fictional company name created for this educational portfolio project. It is not affiliated with, endorsed by, or representative of the real IKEA corporation in any way. All financial figures, customer counts, and business metrics shown in this project are derived entirely from a public, anonymized transactional dataset and do not reflect any real company's actual performance. The IKEA visual branding used in the dashboard is for design and storytelling purposes only.

---

## Project Overview

This project simulates a real world analytics engagement for a fictional UK based gifting and home decor ecommerce brand, **IKEA Gifting**. The goal was to take raw, messy transactional data all the way through to a decision ready business intelligence dashboard, using the same workflow a data analyst would follow inside a real company: define the business problem first, clean and validate the data in SQL, build the analytical logic in Python, and deliver the insight through an interactive Power BI report.

Built as part of a public tutorial series to teach **SQL, Python, and Power BI together as one connected skillset**, rather than as three isolated topics.

---

## My Thinking Process

Most portfolio projects start with a dataset and work backward to a question. I reversed that. I started by writing a **Business Requirements Document** before touching any data, forcing myself to define exactly what business decision this analysis needed to support. Every technical step after that traces back to one of the questions in that document.

I also deliberately chose a dataset that's less commonly used in existing tutorials (Online Retail II instead of the more saturated Olist dataset), and chose to genuinely clean the data rather than use a pre cleaned version, so this project includes real data quality problems: mixed date formats, invalid row types, duplicate transactions, and missing customer identifiers. Solving these is a large part of what this project actually demonstrates.

---

## Business Problem

The business wants to identify its most valuable customers and reduce churn among customers who are at risk of leaving. Leadership currently has no structured view of customer value, so retention efforts are reactive instead of targeted.

## Business Questions Answered

- Who are our most valuable customers, and how much revenue do they actually contribute?
- Which customers have gone quiet and are at risk of churning?
- How does customer value and retention trend across time, by month, quarter, and year?
- How does customer value and churn differ across geographic markets?
- What specific, concrete actions should the business take for each customer segment?

## Key Findings

- **Champions** represent roughly 22% of the customer base but generate close to **68% of total revenue** — a clear 80/20 concentration.
- Overall churn sits at **50.86%** (90 day inactivity threshold), indicating retention is a significant, not marginal, business risk.
- Average customer value varies sharply by segment, from a few hundred for Lost customers to several thousand for Champions.
- Revenue shows clear seasonal spikes in Q4, consistent with a gifting focused product category.

---

## Data Source

**Online Retail II**, UCI Machine Learning Repository, donated by Daqing Chen. A real, anonymized transactional dataset from a UK based, non store online retailer selling all occasion gift ware, covering December 2009 to December 2011. 1,067,371 raw transaction rows across two sheets. Licensed under Creative Commons Attribution 4.0 International.

## Tools and Tech Stack

| Tool | Purpose in this project |
|---|---|
| SQL Server / SSMS | Data import, cleaning, deduplication, and exploratory querying on the full 1M+ row dataset |
| Python (pandas) | RFM calculation, customer scoring, segmentation logic, and churn flagging |
| Jupyter Notebook | Interactive development environment for the Python analysis |
| Power BI Desktop | Three page interactive dashboard: executive summary and RFM deep dive |
| DAX | Measures for customer counts, revenue totals, and average customer value |

## Core Concepts Demonstrated

- **RFM Analysis** — scoring customers on Recency, Frequency, and Monetary value to quantify customer worth on a comparable scale
- **Customer Segmentation** — translating raw RFM scores into business meaningful groups (Champions, Loyal, At Risk, New, Needs Attention, Lost)
- **Churn Definition and Flagging** — applying a fixed inactivity threshold to identify customers at risk of lapsing
- **Data Quality Handling** — resolving mixed date formats, invalid type conversions, and duplicate detection using SQL window functions
- **Dimensional Modeling for BI** — structuring a single unified table so multiple report pages can share one clean source without manual relationships
- **Interactive BI Design** — slicers, bookmark based toggles, and a multi page navigation structure inside Power BI

---

## Project Workflow

**Step 1 — Business Requirements Document.** Defined scope, business questions, key definitions, and success criteria before any data work began.

**Step 2 — SQL, Data Cleaning and Exploration.** Imported both raw sheets into SQL Server, corrected data types, resolved a mixed date format issue affecting over half the dataset, removed 32,907 exact duplicate rows, and validated row counts at every step against the published UCI total.

**Step 3 — Python, RFM Segmentation.** Loaded the cleaned data into pandas, calculated Recency, Frequency, and Monetary value per customer, scored each customer on a 1 to 5 scale per dimension, and assigned each of the 5,881 customers to one of six segments. Added a 90 day churn flag.

**Step 4 — Power BI, Dashboard Build.** Merged the segmentation results back onto transaction level data to create a single analysis ready table, then built a three page report.

---

## Dashboard Structure

**Page 1 — Home:** project title, navigation to the two report pages, and a glossary defining Recency, Frequency, Monetary, and Churned %.

**Page 2 — Business Overview:** total customers, total revenue, churn %, and average customer value as headline KPIs, with a revenue/customer toggle, segment breakdowns, and month/quarter/year trend views. Filterable by month, quarter, year, country, segment, and churn status.

**Page 3 — RFM Analysis:** a segment performance summary table showing customer count, revenue, and average R/F/M score per segment, average customer value by segment, and churn % trended over time.

---

## Sample SQL: Handling Mixed Date Formats

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

## Sample Python: RFM Scoring

```python
rfm['R_Score'] = pd.qcut(rfm['Recency'], 5, labels=[5, 4, 3, 2, 1]).astype(int)
rfm['F_Score'] = pd.qcut(rfm['Frequency'].rank(method='first'), 5, labels=[1, 2, 3, 4, 5]).astype(int)
rfm['M_Score'] = pd.qcut(rfm['Monetary'], 5, labels=[1, 2, 3, 4, 5]).astype(int)
```

---

## How to Reproduce This Project

1. Download the Online Retail II dataset from the UCI Machine Learning Repository
2. Run the SQL scripts in `01_sql` in order to recreate the cleaned `RetailCleaned` table
3. Export `RetailCleaned` to CSV and run the notebook in `02_python` to generate the segmented dataset
4. Open the Power BI file in `03_powerbi` and point it to the exported Excel file to refresh

---

## Connect

This project is on my YouTube channel, **Mansi G.**, where I break down real, end to end data analyst projects for freshers and early career professionals. Full build walkthrough video linked above.

