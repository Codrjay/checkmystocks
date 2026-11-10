# DAX source

This folder holds the full source of the `Checkmystocks App` measure.

## Read this before opening the code

The measure is long because it does three jobs in one place:

1. It calculates product-level data and every figure shown on screen.
2. It writes the interface as HTML and CSS.
3. It writes the JavaScript that makes the interface interactive.

You do not need to read it to understand the project. The business thinking is in the [docs](../docs/) folder. This file is here for completeness and for anyone who wants to reproduce the application.

## How to find your way around

The measure is organised into named variable groups. Search for these names.

| Group | Variables | Purpose |
|---|---|---|
| Base figures | `_InventoryValue`, `_DeadStockValue`, `_Revenue`, `_COGS`, `_Margin` | Pull headline values from the semantic model |
| Rules | `_SlowDays`, `_AtRiskDays`, `_DeadDays`, `_SoonDays`, `_ApproachBuffer` | The business thresholds, defined in one place |
| Sales velocity | `_LastUnitsDate`, `_UnitsSoldLast30Days`, `_AverageDailyUnits`, `_EstimatedStockCoverDays` | Recent demand and stock cover |
| Revenue estimate | `_LastRevenueDate`, `_TrendRows`, `_AverageDailyRevenue`, `_DailyData` | Seven-day estimate and chart data |
| Product table | `_P1`, `_P2`, `_P3`, `_P` | The product-level table, built in stages |
| Summaries | `_Cnt*`, `_Val*`, `_Top5Rows`, `_CatCalc`, `_DeadAging` | Counts, values, categories and ageing |
| Replenishment | `_NeedAll`, `_NeedNow`, `_NeedReview`, `_NeedCost`, `_HoldRows` | Buy now, review first, and do not buy |
| Presentation | `_CSS`, `_Landing`, `_Sidebar`, `_ScOverview`, `_ScC1` to `_ScC4`, `_ScExplorer`, `_Modal`, `_Script` | Interface sections |

To change a business rule, edit the threshold variables near the top. They are the single source for the day counts used across the application.

## Dependencies

The measure calls base measures from the semantic model and does not define them. See [06 Data model](../docs/06-data-model.md).

## Adding the file

Save the full measure as `Checkmystocks_App.dax` in this folder.
