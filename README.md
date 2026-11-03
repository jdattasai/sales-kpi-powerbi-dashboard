# Sales KPI Dashboard — Power BI Sample Project

A full, Power BI-ready sample dashboard project with clean sample datasets, a star-schema data model, and copy-paste DAX measures for a full set of sales KPIs.

**No real customer data is used. All data is synthetic sample data, generated for learning and portfolio use.**

## What is in this repo

```
data/
  fact_sales.csv       1,800 orders, 2023–2025
  dim_product.csv      36 products
  dim_customer.csv     180 customers, Canada
  dim_calendar.csv     2023-01-01 to 2025-12-31
powerbi/
  DAX_Measures.md      Copy-paste DAX for every KPI below
  Dashboard_Layout.md  Page-by-page visual layout for Power BI Desktop
docs/
  Data_Model.md        Star schema, relationships, and data dictionary
  KPIs.md              Verified KPI totals for this dataset
```

## Verified KPIs for this dataset

These totals were calculated directly from `data/fact_sales.csv`, so your Power BI measures should match them:

| KPI | Value |
| --- | --- |
| Total Sales | $1,162,469.20 |
| Total Profit | $431,152.20 |
| Profit Margin | 37.09% |
| Total Orders | 1,800 |
| Units Sold | 4,952 |
| Average Order Value | $645.82 |
| Customers | 180 |
| Sales 2023 | $335,385.20 |
| Sales 2024 | $362,942.25 |
| Sales 2025 | $464,141.75 |
| Sales Growth 2025 vs 2024 | +27.88% |
| Top Category | Furniture — $475,688.80 |
| Top Province | Ontario — $318,860.60 |

Sales by category:
- Furniture: $475,688.80
- Technology: $436,308.35
- Appliances: $195,894.65
- Office Supplies: $54,577.40

## Quick start in Power BI Desktop

1. Open Power BI Desktop.
2. Get Data > Text/CSV, and import all four CSVs from `data/`.
3. In Model view, create these relationships (see `docs/Data_Model.md`):
   - `fact_sales[OrderDate]` → `dim_calendar[Date]` (many-to-one, single direction)
   - `fact_sales[ProductID]` → `dim_product[ProductID]` (many-to-one, single direction)
   - `fact_sales[CustomerID]` → `dim_customer[CustomerID]` (many-to-one, single direction)
   - Mark `dim_calendar` as a Date Table.
4. Create a new measure table (or add measures to `fact_sales`) and paste the measures from `powerbi/DAX_Measures.md`.
5. Build the pages in `powerbi/Dashboard_Layout.md`:
   - Page 1: Overview — KPI cards, sales trend, category and province breakdowns
   - Page 2: Product Performance — category, subcategory, and product tables
   - Page 3: Customer & Channel — segment, province, and channel analysis
6. Add slicers for Year, Category, Province, Segment, and Channel on every page.
7. Publish to the Power BI Service if you want to share it, or keep it as a Desktop (.pbix) file for your portfolio.

## KPIs included (full DAX in `powerbi/DAX_Measures.md`)

Core: Total Sales, Total Cost, Total Profit, Profit Margin %, Total Orders, Units Sold, Average Order Value, Customer Count, Average Discount %

Time intelligence: Sales YTD, Sales PY (previous year), Sales YoY %, Sales YoY Change, Profit YTD, Orders YTD

Breakdowns: Sales by Category, Sales by Province, Sales by Segment, Sales by Channel, Top Product Sales, Sales per Customer

Quality checks: if a KPI card does not match the table above, check that the calendar relationship is active and that no filter is left on a slicer.

## Sample data notes

- Geography is Canada-focused: Ontario, Quebec, British Columbia, Alberta, Manitoba, Saskatchewan, and Nova Scotia.
- Channels are Online, Retail, and Wholesale.
- Segments are Consumer, Corporate, and Home Office.
- Profit in the CSV is already calculated as Sales − Cost, where Sales = Quantity × UnitPrice × (1 − Discount) and Cost = Quantity × UnitCost. The DAX measures recompute the same logic from the fact table, so they will reconcile.
- This is sample data for a portfolio dashboard. Do not use it to make real business decisions.

## Author

Datta Sai Jonnadula — Data Analyst & BI Engineer
GitHub: https://github.com/jdattasai
LinkedIn: https://www.linkedin.com/in/dattasaijonnadula/
