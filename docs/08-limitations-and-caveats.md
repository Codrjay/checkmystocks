# 08 Limitations and caveats

Honest documentation is part of the project. This page lists what the application does not do and what to watch for.

## Filter context: `ALL(Calendar[Date])`

The measure uses `ALL(Calendar[Date])` when it works out the latest sales date, recent units sold, the revenue estimate and the daily revenue data. This removes the calendar date filter in those calculations.

**Effect:** These parts of the application may ignore report-level date slicers or filters. The revenue chart, the 30-day window and the 7-day estimates are always based on the full date range of the data.

**Why it can be intentional:** The application has its own period controls, and the estimates are meant to reflect the most recent activity regardless of a slicer.

**Status:** The intended behaviour needs to be confirmed and tested against the final file. Until then, do not assume every number responds to external date filters in the same way.

## Other filters

Product-level calculations are built from the products in the current filter context. A slicer on category or product will change the product table and every figure built from it. The headline measures (`[Inventory Value]`, `[Dead Stock Value]`) come from the model and follow their own filter behaviour.

## Two sources for some figures

Some numbers can come from two places.

| Figure | Source A | Source B |
|---|---|---|
| Dead stock value | `[Dead Stock Value]` measure, used in headline sentences and cards | Sum of dead stock rows in the product table, used in tables and charts |
| Out of stock count | `[Out-of-Stock Products]` measure, used on the replenishment card | Count of products flagged out in the product table |

If the two sources differ in the final model, the screens can disagree. Reconcile them during validation.

## Analytical limits

| Limit | Detail |
|---|---|
| Thresholds are project rules | 31, 61 and 90 days are chosen for this project. Other businesses may need different values |
| Seasonality | The classification does not know that some products sell seasonally |
| Thirty-day window | Can be noisy for products with infrequent sales |
| Estimates are not forecasts | The 7-day figures use a simple recent average |
| Stock cover ignores lead time | A product may need ordering before cover runs out |
| Shortfall is a floor | It restores to the reorder point. It does not account for supplier minimums or growth |
| Reorder points are inputs | The app trusts the reorder point in the products table and does not calculate it |
| Single location | The design assumes one location |

## Technical limits

| Limit | Detail |
|---|---|
| Custom visual dependency | Requires the HTML Content visual and its behaviour in the viewing environment |
| Font loading | The interface requests Sora and Manrope from Google Fonts. If the environment blocks external requests, it falls back to Arial |
| Large catalogues | All product rows are written into the page text. Very large catalogues will increase size and evaluation time |
| Display caps | Tables are limited to top N rows, as listed in [03 Business rules](03-business-rules.md) |
| Maintainability | The interface lives inside a long DAX string, which is harder to edit and test than a normal web project |

## What the application does not do

- It does not place orders or integrate with suppliers.
- It does not recommend order quantities beyond the shortfall estimate.
- It does not model price changes, discounts or margin by product.
- It does not predict demand statistically.
- It does not handle multiple locations.
