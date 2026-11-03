# DAX Measures — Sales KPI Dashboard

Paste these into Power BI Desktop. Create a measure table named `Measures` first (Modeling > New Table, enter `Measures = {BLANK()}`), or add them to `fact_sales` if you prefer.

All measures assume:
- `fact_sales` is the fact table
- `dim_calendar[Date]` is marked as a Date Table and related to `fact_sales[OrderDate]`
- `dim_product` and `dim_customer` are related as described in `docs/Data_Model.md`

## Core KPIs

```dax
Total Sales =
SUM ( fact_sales[Sales] )

Total Cost =
SUM ( fact_sales[Cost] )

Total Profit =
SUM ( fact_sales[Profit] )

Profit Margin % =
DIVIDE ( [Total Profit], [Total Sales], 0 )

Total Orders =
COUNTROWS ( fact_sales )

Units Sold =
SUM ( fact_sales[Quantity] )

Average Order Value =
DIVIDE ( [Total Sales], [Total Orders], 0 )

Customer Count =
DISTINCTCOUNT ( fact_sales[CustomerID] )

Average Discount % =
AVERAGE ( fact_sales[Discount] )

Sales per Customer =
DIVIDE ( [Total Sales], [Customer Count], 0 )
```

## Time intelligence KPIs

```dax
Sales YTD =
TOTALYTD ( [Total Sales], dim_calendar[Date] )

Profit YTD =
TOTALYTD ( [Total Profit], dim_calendar[Date] )

Orders YTD =
TOTALYTD ( [Total Orders], dim_calendar[Date] )

Sales PY =
CALCULATE (
    [Total Sales],
    SAMEPERIODLASTYEAR ( dim_calendar[Date] )
)

Sales YoY Change =
[Total Sales] - [Sales PY]

Sales YoY % =
DIVIDE ( [Sales YoY Change], [Sales PY], 0 )

Sales Previous Year Total =
CALCULATE (
    [Total Sales],
    PREVIOUSYEAR ( dim_calendar[Date] )
)
```

## Breakdown KPIs

Use these in tables, bar charts, and matrix visuals. They respect slicers for Category, Province, Segment, and Channel.

```dax
Sales by Category =
-- Use [Total Sales] with dim_product[Category] in the visual.
-- No separate measure is needed; this is a reminder to put Category on the axis.
[Total Sales]

Top Product Sales =
MAXX (
    VALUES ( dim_product[ProductName] ),
    [Total Sales]
)

Profit by Category =
[Total Profit]
-- Put dim_product[Category] on the axis in the visual.
```

## Conditional formatting helpers

```dax
Profit Margin Flag =
IF ( [Profit Margin %] >= 0.35, "Strong", IF ( [Profit Margin %] >= 0.25, "OK", "Review" ) )

YoY Flag =
IF ( [Sales YoY %] >= 0, "▲ Growth", "▼ Decline" )
```

## Validation checks

After pasting the measures, with no slicers applied, you should see:

- Total Sales: $1,162,469.20
- Total Profit: $431,152.20
- Profit Margin %: 37.09%
- Total Orders: 1,800
- Units Sold: 4,952
- Average Order Value: $645.82

If a value differs, check:
1. The calendar relationship is active and `dim_calendar` is marked as a Date Table.
2. No slicer is filtering Year, Category, Province, Segment, or Channel.
3. `fact_sales[Sales]`, `[Cost]`, and `[Profit]` imported as Decimal Number, not Text.
