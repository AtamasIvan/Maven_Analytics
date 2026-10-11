# Maven Sales Analysis

Slovak version: [README.sk.md](README.sk.md)

Analysis of CRM sales data (managers, sales agents, products) using SQL and an interactive Power BI dashboard.

![Main page](screenshots/dashboard-main.png)
![Products page](screenshots/dashboard-products.png)

## Goal

To find out which managers, sales agents and products generate the highest revenue, how successfully they close deals (win rate) and why the revenue of individual managers differs.

## Motivation: why these metrics

I chose the metrics so that together they show the strengths and weaknesses of the sales team, broken down by managers, agents and products:

- **Revenue** shows the scale: who and what brings in the most money.
- **Win rate** shows the quality of selling: how often an open deal is actually won.
- **Average won deal value** shows the value of the deals: whether someone wins many small deals or a few large ones.

Looking at only one of them is misleading. For example, a manager with high revenue may simply have more deals, and a manager with a low average deal value may close deals just as successfully as everyone else.

## Data

Maven Analytics – CRM Sales Opportunities: 8,800 deals (opportunities), 30 sales agents, 6 managers, 7 products.

- 4,238 deals won, 2,473 lost, 2,089 still open (Prospecting / Engaging)
- Deals from the period Q4 2016 – Q4 2017; all revenue comes from deals closed in 2017
- Tables used: `sales_pipeline` and `sales_teams`

## Tools and skills

| Area | Tools / terms used |
|---|---|
| **Database and SQL** | PostgreSQL, Apache Superset (SQL Lab as the SQL editor, export of results to CSV), `SELECT`, `WHERE`, `GROUP BY`, `ORDER BY`, `LEFT JOIN`, `CASE WHEN`, aggregate functions (`SUM`, `COUNT`, `AVG`), `NULLIF`, `ROUND`, type casting (`::numeric`), CTE (`WITH`), `EXTRACT`, `TO_DATE` |
| **Power BI** | Power BI Desktop, Power Query (data check, data types), DAX measures (`SUM`, `DIVIDE`), calculated column, slicers, KPI cards, line / bar / stacked bar / scatter chart, sorting by another column |
| **Analysis** | win rate, revenue, average deal value, cohort analysis by quarter, validation of results (totals between SQL and Power BI), small sample size problem |
| **Other** | GitHub, documentation |

## Workflow

1. Data stored in a PostgreSQL database (`sales_pipeline`, `sales_teams`).
2. Three SQL queries written in Apache Superset SQL Lab: product performance, manager performance, agent performance by quarter.
3. Validation: the total won revenue in all queries is **10,005,534 $**.
4. Query results exported to CSV and loaded into Power BI.
5. Data types set in Power Query, measures created in DAX.
6. Two-page dashboard created (Main page, Products).
7. KPIs on the dashboard checked against the SQL results.

## Metric definitions

| Metric | Definition |
|---|---|
| Won / lost deals | number of deals with stage `Won` / `Lost` |
| Win rate | won / (won + lost); open deals are not counted because their result is not known yet |
| Revenue | sum of `close_value` of won deals |
| Average won deal (Revenue per Deal) | revenue / number of won deals |
| Period | year and quarter in which the deal was opened (engage date) |

## Key findings

- Total revenue is **10.01 M $** from **4,238 won deals** (average deal 2,361 $), win rate **63.15 %**.
- Three products, GTXPro (3.51 M $), GTX Plus Pro (2.63 M $) and MG Advanced (2.22 M $), make up about **84 %** of revenue.
- GTK 500 has the highest average won deal value (**26,707 $**), but only 15 won deals, so its revenue is just 0.40 M $.
- Win rate is almost the same for every product (60.3 – 64.8 %) and every manager (62.1 – 64.4 %). Differences in revenue are therefore caused by the **number and value of deals**, not by how successfully they are closed.
- Melvin Marxen has the highest revenue (2.25 M $). Rocco Neubert has the highest average won deal value (2,837 $), Dustin Brinkmann the lowest (1,465 $).
- The best sales agent brought in 1.15 M $, more than twice as much as the second one (0.48 M $).

## Limitations

- Periods are grouped by the date a deal was **opened**, so the last quarters look weaker: many of those deals had not been closed yet.
- The Q4 2016 cohort is small, so its numbers should not be generalized.
- The results come from a single dataset and do not explain *why* deals were won or lost.
## Repository structure

```
maven-sales-analysis/
├── README.md
├── README.sk.md
├── screenshots/
│   ├── dashboard-main.png
│   └── dashboard-products.png
├── sql/
│   ├── product_performance.sql
│   ├── manager_performance.sql
│   └── agent_cohort_performance.sql
└── powerbi/
    └── maven_sales_dashboard.pbix
```
