# Data Model — Star Schema

```
dim_calendar (1) ──< fact_sales >── (1) dim_product
                         │
                         └── (1) dim_customer
```

## Relationships (all many-to-one, single direction, active)

| From | To | Cardinality | Cross-filter direction |
| --- | --- | --- | --- |
| fact_sales[OrderDate] | dim_calendar[Date] | Many to one | Single |
| fact_sales[ProductID] | dim_product[ProductID] | Many to one | Single |
| fact_sales[CustomerID] | dim_customer[CustomerID] | Many to one | Single |

Mark `dim_calendar` as a Date Table in Power BI Desktop (Table tools > Mark as Date Table, using `dim_calendar[Date]`). This is required for the YTD and SAMEPERIODLASTYEAR measures to work correctly.

Do not create a relationship on `fact_sales[ShipDate]` to the calendar unless you add a second, inactive relationship and activate it with USERELATIONSHIP in a specific measure. Keep ShipDate as a descriptive column only.

## Data dictionary

### fact_sales (1,800 rows)

| Column | Type | Description |
| --- | --- | --- |
| OrderID | Text | Unique order identifier, e.g. ORD-2025-0001 |
| OrderDate | Date | Date the order was placed |
| ShipDate | Date | Date the order shipped (OrderDate + 2 to 7 days) |
| CustomerID | Text | Foreign key to dim_customer |
| ProductID | Text | Foreign key to dim_product |
| Channel | Text | Online, Retail, or Wholesale |
| Quantity | Whole number | Units ordered |
| UnitPrice | Decimal | Product list price at time of sale |
| Discount | Decimal | Discount rate, 0 to 0.20 |
| Sales | Decimal | Quantity × UnitPrice × (1 − Discount) |
| Cost | Decimal | Quantity × UnitCost from dim_product |
| Profit | Decimal | Sales − Cost |

### dim_product (36 rows)

| Column | Type | Description |
| --- | --- | --- |
| ProductID | Text | Primary key, e.g. P001 |
| ProductName | Text | Product name |
| Category | Text | Technology, Furniture, Office Supplies, Appliances |
| Subcategory | Text | Sub-group within the category |
| UnitPrice | Decimal | List price |
| UnitCost | Decimal | Cost price |

### dim_customer (180 rows)

| Column | Type | Description |
| --- | --- | --- |
| CustomerID | Text | Primary key, e.g. C001 |
| CustomerName | Text | Sample customer label (Customer 001, etc.) |
| Segment | Text | Consumer, Corporate, Home Office |
| Province | Text | Canadian province |
| City | Text | City in that province |
| Country | Text | Canada for all rows |

### dim_calendar (1,096 rows, 2023-01-01 to 2025-12-31)

| Column | Type | Description |
| --- | --- | --- |
| Date | Date | Primary key |
| Year | Whole number | Calendar year |
| Quarter | Text | Q1–Q4 |
| Month | Whole number | 1–12 |
| MonthName | Text | January–December |
| MonthYear | Text | YYYY-MM, for trend axis sorting |
| Day | Whole number | Day of month |
| Weekday | Text | Monday–Sunday |

## Grain

One row in `fact_sales` is one order line: one product, for one customer, on one order date, through one channel. There are no duplicate OrderID + ProductID combinations that would double-count sales if you sum Sales directly.

## How to extend this model

- Add a `dim_channel` table if you want channel as a dimension instead of a text column in the fact table.
- Add a returns table with a negative quantity if you want to demonstrate returns handling.
- Add a targets table by Month × Category if you want to add Sales vs Target KPIs.
