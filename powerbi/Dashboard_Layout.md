# Dashboard Layout — Power BI Desktop

Three pages. Use the same slicers on every page: Year, Category, Province, Segment, Channel.

## Page 1 — Overview

Title: Sales Overview

KPI cards (top row, left to right):
1. Total Sales — `[Total Sales]`, currency, 0 decimals
2. Total Profit — `[Total Profit]`, currency
3. Profit Margin % — `[Profit Margin %]`, percentage, 1 decimal
4. Total Orders — `[Total Orders]`, whole number
5. Average Order Value — `[Average Order Value]`, currency
6. Sales YoY % — `[Sales YoY %]`, percentage, with the YoY Flag as a subtitle / conditional colour

Visuals:
- Line chart: Sales by Month — X-axis `dim_calendar[MonthYear]`, Y-axis `[Total Sales]`, legend by Year if you want a year-over-year overlay
- Clustered column chart: Sales by Category — X-axis `dim_product[Category]`, Y-axis `[Total Sales]`
- Donut chart: Sales by Province — Legend `dim_customer[Province]`, Values `[Total Sales]`
- Table: Top 10 Products by Sales — Columns: ProductName, Category, `[Total Sales]`, `[Total Profit]`, `[Profit Margin %]`, sorted by Total Sales descending

Insights to call out in a text box:
- 2025 sales grew 27.88% vs 2024 in this sample dataset.
- Furniture is the top category, Ontario is the top province.

## Page 2 — Product Performance

Title: Product Performance

Visuals:
- Matrix: Category > Subcategory > ProductName, with `[Total Sales]`, `[Total Profit]`, `[Profit Margin %]`, `[Units Sold]`
- Bar chart: Profit by Category — X-axis `dim_product[Category]`, Y-axis `[Total Profit]`
- Scatter or bar chart: Units Sold vs Profit by Product — use ProductName as detail, Units Sold on X, Profit on Y, to spot high-volume / low-margin products
- Card: Top Product Sales — `[Top Product Sales]`

Filters to test:
- Category = Technology, then Furniture, and compare margin. Office Supplies is low-sales but useful for margin discussion.

## Page 3 — Customer & Channel

Title: Customer & Channel

Visuals:
- Clustered column chart: Sales by Segment — X-axis `dim_customer[Segment]`, Y-axis `[Total Sales]`
- Bar chart: Sales by Channel — X-axis `fact_sales[Channel]`, Y-axis `[Total Sales]`
- Map (filled map, if available in your Power BI version): Sales by Province — Location `dim_customer[Province]`, Values `[Total Sales]`
- Table: Top 20 Customers by Sales — Columns: CustomerID, Segment, Province, `[Total Sales]`, `[Total Orders]`, `[Sales per Customer]`
- Card: Customer Count — `[Customer Count]`
- Card: Sales per Customer — `[Sales per Customer]`

## Formatting guidance

- Theme: use a consistent dark or light theme, one accent colour for Sales and a second for Profit.
- Number formats: Currency $#,##0, Percentages 0.0%, Counts #,##0.
- Titles: give every visual a clear title, not the default measure name.
- Tooltips: turn on tooltips for the trend and category charts so a viewer can see the exact value without a table.
- Do not add a visual that cannot be explained in one sentence. If a chart needs a paragraph to explain, replace it with a table.
