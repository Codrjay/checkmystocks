# 06 Data model

Checkmystocks sits on a star schema. Two dimension tables describe the business, and two fact tables record what happened. The application reads from this model and does not store any data of its own.

## Model at a glance

```
                      +--------------+
                      |   Calendar   |
                      |  Dimension   |
                      +------+-------+
                             | 1
              +--------------+--------------+
              | *                           | *
   +----------+-----------+     +-----------+----------+
   |   StockTransactions  |     |  DailyStockSnapshot  |
   |         FACT         |     |         FACT         |
   +----------+-----------+     +-----------+----------+
              | *                           | *
              +--------------+--------------+
                             | 1
                      +------+-------+
                      |   Products   |
                      |  Dimension   |
                      +--------------+
```

| Type | Table | What it holds |
|---|---|---|
| Dimension | `Products` | One row per product: name, category, unit cost, reorder point |
| Dimension | `Calendar` | One row per date, the official date table for the model |
| Fact | `StockTransactions` | Stock movements: sales, purchases and other changes |
| Fact | `DailyStockSnapshot` | The quantity physically on hand, per product, per day |

## Why a star schema

Products and Calendar describe the facts. The fact tables hold the events and measurements being analysed. Keeping them separate means one filter on a dimension can flow into every fact table.

For example, selecting `Category = Beverages` filters both facts at once:

| Question | Answered from |
|---|---|
| How much inventory value is tied up in Beverages? | `DailyStockSnapshot` |
| How many units of Beverages have we sold? | `StockTransactions` |

Same product dimension, different facts. That is the reason the model is shaped this way.

## Relationships

| From (one side) | To (many side) | Cardinality | Cross-filter | State |
|---|---|---|---|---|
| `Products[ProductID]` | `StockTransactions[ProductID]` | One to many | Single | Active |
| `Products[ProductID]` | `DailyStockSnapshot[ProductID]` | One to many | Single | Active |
| `Calendar[Date]` | `StockTransactions[Date]` | One to many | Single | Active |
| `Calendar[Date]` | `DailyStockSnapshot[Date]` | One to many | Single | Active |

The "one" side is always the dimension. Filters flow from dimensions to facts only.

**No relationship exists between `StockTransactions` and `DailyStockSnapshot`.** They are both fact tables. Connecting them directly would create ambiguous filter paths. They are only ever compared through the shared dimensions.

## Fields

### Products

| Field | Purpose |
|---|---|
| `ProductID` | Key. Relates to both fact tables |
| `ProductName` | Display and search |
| `Category` | Category grouping. A blank category is shown as "No category" |
| `UnitCost` | Inventory valuation and purchase cost |
| `ReorderPoint` | Replenishment conditions and shortfall |

### Calendar

| Field | Purpose |
|---|---|
| `Date` | Key. Relates to both fact tables |
| `Month Name` | Display. Sorted by `Month Number` so months appear in calendar order |
| `Month Number` | Sort key for `Month Name` |

### StockTransactions

| Field | Purpose |
|---|---|
| `TransactionID` | Unique transaction identifier |
| `ProductID` | Relates to `Products` |
| `Date` | Relates to `Calendar` |
| `StockMovement` | Quantity change for the transaction |

### DailyStockSnapshot

| Field | Purpose |
|---|---|
| `ProductID` | Relates to `Products` |
| `Date` | Relates to `Calendar` |
| `QuantityOnHand` | Units physically on hand |

Further columns in the fact tables (for example transaction type) should be added here if the application or base measures use them.

## Modelling decisions

### 1. Snapshot for position, transactions for movement

Current stock is taken from `DailyStockSnapshot`, not by summing `StockTransactions[StockMovement]`.

| Concept | Source | Question it answers |
|---|---|---|
| Inventory position | `DailyStockSnapshot` | What was physically on hand? |
| Inventory movement | `StockTransactions` | What movements happened, and what was sold, bought or costed? |

These are different ideas. A running total of movements can drift from the physical count because of errors, returns or adjustments. Using a dedicated snapshot keeps the position trustworthy, and keeps the transaction table focused on sales, purchases and COGS.

### 2. Single-direction filtering

Every relationship filters from dimension to fact. This keeps filter behaviour predictable and avoids circular or ambiguous paths. Selecting a date range does not filter products, and selecting a product does not filter the calendar.

### 3. A proper date table

`Calendar` is marked as the date table in Power BI (Table tools, Mark as date table, using `Date`). This tells Power BI that it is the official date dimension and enables time intelligence.

Requirements checked:

| Requirement | Why |
|---|---|
| `Date` has the Date data type | Required for the date table |
| Values are unique | No duplicate dates |
| Dates are continuous | No gaps |
| Covers the full data period | The data runs from 1 January 2026 to 31 August 2026, so the calendar must cover at least that |

### 4. Sorted month names

`Month Name` is sorted by `Month Number`. Without this, Power BI sorts alphabetically (April, August, December, February) instead of chronologically.

### 5. Technical keys are hidden

These columns are hidden from the report field list so nobody drags them into a visual by mistake. Nothing is deleted.

| Table | Hidden columns |
|---|---|
| `Products` | `ProductID` |
| `StockTransactions` | `TransactionID`, `ProductID` |
| `DailyStockSnapshot` | `ProductID` |
| `Calendar` | None. `Date` stays visible because reports use it |

## Base measures

The application does not define these. It calls them from the model, so its results are only as correct as these definitions.

| Measure | Fact table used | Used in the application for |
|---|---|---|
| `[Current Stock Units]` | `DailyStockSnapshot` | Product-level current stock |
| `[Inventory Value]` | `DailyStockSnapshot` with `Products[UnitCost]` | Headline inventory value |
| `[Units Sold]` | `StockTransactions` | Recent sales velocity |
| `[Revenue]` | `StockTransactions` | Revenue trend and 7-day estimate |
| `[COGS]` | `StockTransactions` | Business performance |
| `[Gross Margin %]` | Derived from revenue and COGS | Business performance |
| `[Inventory Turnover]` | Derived from COGS and average inventory | Business performance |
| `[Last Sale Date]` | `StockTransactions` | Movement status |
| `[Days Since Last Sale]` | `StockTransactions` | Movement status |
| `[Dead Stock Value]` | Combines status with inventory value | Headline dead stock value |
| `[Out-of-Stock Products]` | `DailyStockSnapshot` | Replenishment card |

The "fact table used" column reflects the modelling decisions above. Confirm each row against the file.

### Formulas to add

Paste each measure's DAX from the Power BI file so readers can see how the numbers are built.

| Measure | Definition |
|---|---|
| `[Current Stock Units]` | _paste DAX_ |
| `[Inventory Value]` | _paste DAX_ |
| `[Units Sold]` | _paste DAX_ |
| `[Revenue]` | _paste DAX_ |
| `[COGS]` | _paste DAX_ |
| `[Gross Margin %]` | _paste DAX_ |
| `[Inventory Turnover]` | _paste DAX_ |
| `[Last Sale Date]` | _paste DAX_ |
| `[Days Since Last Sale]` | _paste DAX_ |
| `[Dead Stock Value]` | _paste DAX_ |
| `[Out-of-Stock Products]` | _paste DAX_ |

## Model validation

Before writing measures, the model was tested with a temporary table visual showing `Products[ProductName]`, `Products[Category]`, a simple sum of `DailyStockSnapshot[QuantityOnHand]` and a sum of `StockTransactions[StockMovement]`.

The question being tested: does selecting a product correctly filter both fact tables? Product-level quantities should appear, with snapshot quantity and transaction movement shown separately.

## How the model meets the application

The application builds its product table from `VALUES(Products[ProductID])`, so slicers on `Products` flow into every product-level figure. It uses `ALL(Calendar[Date])` for the 30-day sales window and the revenue data, which means those parts do not respond to external date slicers. See [08 Limitations and caveats](08-limitations-and-caveats.md).

## Keeping model and application apart

| Lives in the model | Lives in the application measure |
|---|---|
| Base measures such as revenue, COGS and stock | Movement status, stock cover, shortfall |
| Relationships and filter flow | Replenishment groups and purchase cost |
| Date table | HTML, CSS and JavaScript |

Application calculations are documented in [07 Calculation reference](07-calculation-reference.md).