# 07 Calculation reference

Every calculation the application performs itself. Base measures from the model are listed in [06 Data model](06-data-model.md).

## Sales velocity

| Item | Calculation |
|---|---|
| Latest sales date | Latest date in the calendar with non-blank units sold |
| Window start | Latest sales date minus 29 days |
| Units sold, last 30 days | Units sold between the window start and the latest sales date |
| Average daily units | Units sold in the last 30 days / 30 |
| Estimated 7-day units | Average daily units x 7 |
| Stock cover (days) | Current stock / average daily units. Blank when average daily units is zero |

The same 30-day window is applied per product for the product table and for the portfolio total.

## Revenue estimate

| Item | Calculation |
|---|---|
| Latest revenue date | Latest date with non-blank revenue |
| Trend base | The latest 30 dates that have non-blank revenue, on or before the latest revenue date |
| Average daily revenue | Average revenue across those dates |
| Projected 7-day revenue | Average daily revenue x 7. Shows N/A when there are no rows |

The 30 dates are the latest 30 dates **with revenue**, not necessarily 30 consecutive calendar days. If some days have blank revenue, the window reaches further back in time.

## Inventory value and shares

| Item | Calculation |
|---|---|
| Product value | Current stock x unit cost |
| Dead stock share | Dead stock value / inventory value |
| Healthy share | Value of Healthy products / sum of product values |
| Stuck share | (Slow + At Risk + Dead value) / sum of product values |
| Other value | Total value minus Healthy, Slow, At Risk and Dead. Covers out of stock and no sales |
| Top 5 share | Value of the five largest products / sum of product values |
| Category share | Category value / sum of product values |

## Replenishment

| Item | Calculation |
|---|---|
| Replenishment condition | out, below, approaching or ok, as described in [03 Business rules](03-business-rules.md) |
| Shortfall | MAX(0, reorder point - current stock) |
| Estimated purchase cost | Shortfall x unit cost |
| Need all | Products whose condition is not ok |
| Reorder now | Need all, with a last sale date and fewer than 90 days since last sale |
| Check demand first | Need all, with no last sale date or 90 or more days since last sale |
| Cost to restore | Sum of estimated purchase cost across Reorder now |
| Stop buying value | Dead stock value + slow moving value + at risk value from the product table |
| Half of dead stock | Dead stock value / 2 |
| Coverage message | If half of dead stock is at least the cost to restore, "enough to fund the whole restock". Otherwise the percentage of the restock cost it would cover |

## Ranking

| List | Order |
|---|---|
| Reorder list | Priority rank ascending (out, below, approaching), then average daily units descending |
| Dead stock table | Value descending |
| Slow and at risk table | Days since last sale descending |
| Check demand table | Reorder point descending |
| Do not reorder table | Value descending |

## Revenue chart (JavaScript)

| Item | Behaviour |
|---|---|
| Source data | Daily revenue as text pairs of date and value, written by DAX |
| Presets | 30 days, 90 days, 12 months, all time, counted back from the last revenue date |
| Default grouping | 30 days groups by day, up to 90 days by week, longer by month |
| Weeks | Start on Monday |
| Previous period | A window of the same number of calendar days immediately before the selected start. Unavailable if it starts before the first data date |
| Comparison sentence | Percentage change of selected total against previous total, when the previous total is above zero |

## Product detail modal

Values shown in the modal are read from the text of the clicked table row, so they match what the table displays.

## Business performance bars

| Bar | Width |
|---|---|
| Revenue | 100 percent |
| Cost of goods sold | COGS / Revenue, capped at 100 percent |
| Gross margin | Margin x 100, limited to between 0 and 100 percent |
