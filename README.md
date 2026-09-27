# Retail Sales & Profitability Analysis — Sample Superstore

Exploratory data analysis on the Sample Superstore dataset (2014–2017), built to dig into a real business problem: **revenue has been growing year over year, but profit margin hasn't kept pace.** The project traces that gap down to category, region, sub-category, discounting and shipping behavior, and closes with concrete recommendations.

## Business Problem

Sales grew every year from 2014 to 2017, but profit growth was inconsistent — margin actually *dropped* in the most recent year of data despite revenue being at its highest. The goal of this analysis is to find where that margin is leaking and why.

## Dataset

- **Source:** Sample Superstore dataset (US retail orders)
- **Size:** 9,994 orders × 14 columns, spanning 2014–2017
- **Storage:** loaded into PostgreSQL, queried into pandas via SQLAlchemy

| Column | Type | Description |
|---|---|---|
| `delivery_time` | float | Days between order and delivery |
| `yearwise` | int | Order year (2014–2017) |
| `ship_mode` | object | Shipping mode used |
| `segment` | object | Customer segment (Consumer / Corporate / Home Office) |
| `country`, `city`, `state`, `region` | object | Order location |
| `category`, `sub_category` | object | Product classification |
| `sales` | float | Order revenue ($) |
| `quantity` | int | Units ordered |
| `discount` | float | Discount applied (0–1) |
| `profit` | float | Order profit ($) |

No missing values in any column.

## Tech Stack

- **PostgreSQL** — data storage
- **Python** — pandas, SQLAlchemy, matplotlib, seaborn
- **Jupyter Notebook** — analysis environment
- **Power BI** — interactive dashboard (in progress)

## Approach

1. Pull cleaned data from PostgreSQL into a pandas DataFrame
2. Data quality check — nulls, dtypes, summary statistics, outlier detection (boxplots)
3. Year-wise revenue, profit, and margin trend, with YoY growth rates
4. Discount vs. profit and discount vs. sales relationships
5. Category, sub-category, region, state, shipping mode, and segment breakdowns (average sales / profit / discount)
6. Correlation heatmap across numeric fields
7. Insights synthesis and business recommendations

## SQL

```sql
SELECT * FROM samplestore;
```
<!-- If you did any cleaning/transformation in SQL before this (renaming columns, computing delivery_time, etc.), paste that script here too — it belongs in this section. -->

## Key Insights

**Year-wise**

![Revenue vs Profit by Year](images/year_revenue_profit.png)
![Profit Margin % by Year](images/year_margin_trend.png)

- 2014→2015: sales dipped slightly (-2.83%) but profit rose 24.37%, pushing margin from 10.23% to 13.10%
- 2015→2016: strongest revenue growth of the period (+29.47%), profit grew in step (+32.74%), margin held at 13.43%
- 2016→2017: revenue kept growing (+20.36%) but profit growth slowed sharply (+14.24%), margin fell to 12.74% — coincides with average discounts climbing back toward ~15.65%

**Category & Sub-category**

![Average Sales by Category](images/avg_sales_by_category.png)
![Average Profit by Category](images/avg_profit_by_category.png)
![Average Discount by Category](images/avg_discount_by_category.png)
![Average Profit by Sub-category](images/avg_profit_by_subcategory.png)

- Technology is the strongest category — highest average sales with relatively low average discount (~13%), driving strong profitability. Copiers are the standout performer.
- Furniture has good sales but the highest average discount (~17%), which erodes margin. Tables and Bookcases post *negative* average profit.
- Office Supplies' "Supplies" sub-category also runs negative average profit despite minimal discounting — a pricing/cost issue, not a discounting one.

**Region**

![Average Profit by Region](images/avg_profit_by_region.png)

- West: highest profitability, lowest average discount (~10%) — most efficient pricing.
- East: solid balance of sales and profit.
- South: strong sales but leans on higher discounts (~15%), hurting margin.
- Central: weakest region — low sales, highest discounts (~26%), lowest profitability.

**State, Shipping, Segment**

![Average Profit by Shipping Mode](images/avg_profit_by_shipmode.png)
![Average Profit by Segment](images/avg_profit_by_segment.png)

- Vermont has the highest average profit; Ohio the lowest.
- Shipping modes perform similarly on sales; First Class edges ahead on average profit.
- Home Office is the best-balanced customer segment — solid sales, lower discounts, best profitability.

**Discounting**

![Discount vs Profit](images/discount_vs_profit.png)
![Discount vs Sales](images/discount_vs_sales.png)

- Discounts above 30% consistently hurt profitability.
- 10–15% discount range balances sales lift against margin.

**Correlation**

![Correlation Heatmap](images/correlation_heatmap.png)

## Business Recommendations

1. Cut excessive discounting in Furniture, especially Tables and Bookcases.
2. Replicate the West region's pricing approach in other regions.
3. Investigate Central region's underperformance (low sales, high discounting).
4. Push marketing on Copiers, Accessories, and Phones — the biggest sales/profit drivers.
5. Revisit pricing for Supplies and other chronically loss-making lines.
6. Keep discounts in the 10–15% range wherever possible.
7. Grow the Home Office segment — strong sales with healthy margins.

```

## How to Run

1. Set up a PostgreSQL database and load the Superstore dataset into a table named `samplestore`.
2. Install dependencies:
```bash
   pip install sqlalchemy psycopg2-binary pandas matplotlib seaborn
```
3. Set your database credentials as environment variables and update the connection string in the first cell.
4. Run the notebook top to bottom.

## Next Steps

- Power BI dashboard: problem statement → KPI home page → year-wise → category/region/sub-category → profit vs. sales → discount vs. profit/sales.
