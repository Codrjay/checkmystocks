# 03 Business rules

The exact rules the application applies. These are project business rules, not universal retail standards. The day thresholds are defined once at the top of the DAX measure.

## Movement status

Checked in this order. The first match wins.

| Order | Status | Condition |
|---|---|---|
| 1 | No Stock | Current stock <= 0 |
| 2 | No Sales | No last sale date exists |
| 3 | Dead Stock | Days since last sale >= 90 |
| 4 | At Risk | Days since last sale >= 61 |
| 5 | Slow Moving | Days since last sale >= 31 |
| 6 | Healthy | Everything else |

| Threshold variable | Value |
|---|---|
| `_SlowDays` | 31 |
| `_AtRiskDays` | 61 |
| `_DeadDays` | 90 |

## Close to turning dead

A product is "close to turning dead" when it is At Risk and its days since last sale is at least `_DeadDays - _SoonDays`, which is 75 days.

| Variable | Value |
|---|---|
| `_SoonDays` | 15 |

## Replenishment condition

| Condition | Rule |
|---|---|
| Out of stock | Current stock <= 0 |
| Below reorder point | Reorder point > 0 and stock <= reorder point |
| Approaching reorder point | Reorder point > 0 and stock <= reorder point x 1.2 |
| Okay | Everything else |

| Parameter | Value |
|---|---|
| `_ApproachBuffer` | 1.2 |

Products with no reorder point set (zero or blank) cannot be flagged as below or approaching. They can still be flagged as out of stock.

## Replenishment priority

Priority order for the reorder list: out of stock first, then below reorder point, then approaching. Within the same level, faster sellers (higher average daily units) come first.

## Reorder now versus check demand first

Products flagged for replenishment are split into two groups.

| Group | Rule | What the app tells the owner |
|---|---|---|
| Reorder now | Has a last sale date and days since last sale < 90 | Restock these |
| Check demand first | No last sale date, or days since last sale >= 90 | Confirm there is demand before buying |

## Do not reorder

Products that are Slow Moving, At Risk or Dead Stock. The application advises pausing purchases until they sell down.

## Shortfall and cost

| Item | Formula |
|---|---|
| Shortfall | MAX(0, Reorder Point - Current Stock) |
| Estimated purchase cost | Shortfall x Unit Cost |
| Cost to restore | Sum of estimated purchase cost for the "reorder now" group |

## Dead stock ageing bands

| Band | Days since last sale |
|---|---|
| 90 to 119 days | 90 to 119 |
| 120 to 179 days | 120 to 179 |
| 180 to 364 days | 180 to 364 |
| A year or more | 365 and above |

## Display limits

| Table | Rows shown |
|---|---|
| Dead stock products | Top 25 by value |
| Slow moving and at risk | Top 15, longest since last sale first |
| Reorder list | Top 15 by priority |
| Check demand before reordering | Top 6 by reorder point |
| Do not reorder | Top 8 by value |
| Five biggest positions | Top 5 by value |
| Category bars | Top 6 categories by value |
