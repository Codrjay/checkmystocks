# 09 Validation and testing

## Status

Calculations were checked during development. Those checks were done earlier in the build and need to be repeated against the final Power BI file before the project is presented as fully validated.

## Historical checks during development

The following were compared and reported to match at the time.

| Area |
|---|
| Product inventory value against current stock x unit cost |
| Out of stock quantities |
| Stock risk quantities |
| Reorder soon quantities |
| Blank reorder quantities for products that were not flagged |
| Reorder value |
| Revenue |
| COGS |
| Gross profit |
| Gross margin |
| Inventory turnover |

These are historical results. They are not a substitute for a final validation record.

## Worked example

| Item | Value |
|---|---|
| Product | Bleach 1L |
| Current stock | 148 units |
| Reorder point | 18 units |
| Days since last sale | 0 |

The product sells actively, yet its stock position is healthy because 148 is far above 18. It should not appear on the reorder list. This example illustrates why replenishment is based on stock position and not on sales speed alone.

## Final validation plan

Repeat against the final file and record the evidence (a screenshot, a query result or a reconciled table).

| Check | Method | Result |
|---|---|---|
| Current stock per product | Compare with latest snapshot | _pending_ |
| Inventory value | Sum of stock x cost against measure | _pending_ |
| Dead stock value | Product table sum against `[Dead Stock Value]` | _pending_ |
| Days since last sale | Spot check against transactions | _pending_ |
| Average daily units and stock cover | Recompute for sample products | _pending_ |
| Replenishment flags | Sample each condition including boundaries | _pending_ |
| Shortfall and purchase cost | Recompute for sample products | _pending_ |
| Revenue, COGS, margin, turnover | Compare with a standard report visual | _pending_ |
| Out of stock count | Card against product table | _pending_ |

## Filter-context tests

| Test | Expected | Result |
|---|---|---|
| Apply an external date slicer | Document whether the revenue chart and 30-day window change | _pending_ |
| Apply a category slicer | Product table and totals follow the slicer | _pending_ |
| Apply a product slicer | Product table follows the slicer | _pending_ |
| Compare app period presets with the slicer | Behaviour documented | _pending_ |

## Application tests

| Area | Test |
|---|---|
| Navigation | Every sidebar item and every "Next" button opens the right screen |
| Landing | Enter and Home work and the scroll resets |
| Theme | Toggle works on every screen and the chart redraws |
| KPI cards | Open and close, and only one detail opens at a time per screen |
| Revenue chart | Each preset, each grouping, both sliders, previous period, tooltip |
| Edge cases for the chart | No revenue, one data point, previous period unavailable |
| Explorer | Search, each status filter, combined search and filter, count text |
| Modal | Opens from a row, closes by button, backdrop and Escape |
| Keyboard | Tab, Enter and Space on all buttons |
| Responsive | Check at wide, tablet and narrow widths |
| Empty states | No dead stock, no replenishment needs |
