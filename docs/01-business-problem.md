# 01 Business problem and positioning

## The situation

A small retailer buys stock, puts it on shelves and sells it. Most owners track the selling side well: daily sales, weekly revenue, best sellers. The buying side is usually tracked far less carefully.

That gap is expensive. Every unit on the shelf is cash that has already left the business. When products sell, that cash comes back with a margin. When they do not, the cash stays locked in place, and often the retailer keeps buying more of the same product because nobody is asking whether it is still selling.

## What owners usually cannot see

| They can see | They often cannot see |
|---|---|
| Revenue and sales volume | How much capital is tied up in stock |
| Best selling products | Which products have quietly stopped selling |
| That a product is low in stock | Whether it is worth restocking |
| Current quantities | How long the current quantity will last |

## Two common mistakes this project is designed to prevent

**Mistake one: treating a fast seller as a reorder.**
A product selling quickly does not automatically need to be reordered. If there are 148 units in stock and the reorder point is 18, the stock position is healthy even though the product sells every day.

**Mistake two: treating a low product as a reorder.**
A product with low stock that has not sold in 100 days is not a gap in the shelf. It may be a product customers no longer want. Buying more of it makes the problem worse.

## The user

The primary user is the owner or manager of a single-location retail business, who:

- Is not an analyst and does not want to build their own reports.
- Wants answers in plain language.
- Makes purchasing decisions regularly and needs to make them with confidence.
- Is working with limited capital, so every unnecessary purchase matters.

## The core questions

| Area | Question |
|---|---|
| Inventory position | How much inventory do I currently have? |
| Capital exposure | How much money is tied up in dead stock? |
| Inventory movement | Which products are slow moving or at risk? |
| Replenishment | Which products need to be reordered? |
| Product investigation | What is happening with a particular product? |

## Positioning

Checkmystocks is an **inventory intelligence application**, not an inventory dashboard.

A dashboard shows how much stock a retailer has. An intelligence application tells them what that stock means for the business and what to do about it. The value is in the interpretation, not the display.

## What the project demonstrates

- Understanding of a real business problem before touching the data.
- Data modelling and DAX at a production level.
- Analytical reasoning, shown through documented decisions.
- Application design and user experience inside a BI tool.
- Honest handling of limitations.
