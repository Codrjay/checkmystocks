# 04 Application walkthrough

A tour of every screen and what it is for.

## Landing page

The entry point. It introduces the product with the hero name, the tagline "Know your stock. Stay in control." and a short description. Floating glass cards show two live numbers: cash held in stock and the share stuck in dead stock. A story section explains the four questions, and a closing call to action opens the workspace.

Design intent: it should feel like the front door of a real application, not a report page. There is no navigation on this screen.

## Navigation

A sidebar offers six destinations in two groups.

| Group | Item |
|---|---|
| The story | Overview |
| The story | 01 Current inventory |
| The story | 02 Dead stock |
| The story | 03 Slow moving and at risk |
| The story | 04 Replenishment |
| Detail | Inventory Explorer |

Every screen has a **Switch theme** button and a **Home** button that returns to the landing page. On narrow screens the sidebar collapses to icons.

## Overview

A summary of everything.

- **The headline:** one sentence with inventory value, the dead stock share and how many products need replenishment.
- **Four KPI cards:** Current Inventory Value, Money in Dead Stock, Slow Moving or At Risk, Need Replenishment. Each card can be clicked to expand a short explanation and a list of the products behind the number.
- **Revenue panel:** an interactive trend with period presets, day, week and month grouping, adjustable from and to sliders, previous period comparison and a hover tooltip. A sentence above the chart restates what the selection shows.
- **Next 7 days:** projected revenue, expected units sold and how long current stock lasts.
- **Business performance:** revenue, cost of goods sold and gross margin, with inventory turnover in the explainer.

## 01 Current inventory

Answers: how much inventory do I currently have?

- Headline with total value, product count, unit count and the largest category.
- KPI cards: Inventory Value, Units in Stock, Products Tracked, Healthy Share of Value.
- **Stock health bar:** inventory value split by movement status, with a legend giving product count, value and percentage for each status.
- **Where the money sits:** value by category, with the dead stock portion shown in red inside each bar.
- **Five biggest positions:** the products holding the most cash.

## 02 Dead stock

Answers: how much money is tied up in dead stock?

- Headline with the value, share of inventory, product count and the longest time without a sale.
- KPI cards: Dead Stock Value, Share of Inventory Value, Dead Stock Products, Longest Without a Sale.
- **Ageing bars:** dead stock value by how long since the last sale, with a callout for stock that has gone a year or more.
- **Dead stock table:** the top 25 by value, with each product's share of total dead stock.

## 03 Slow moving and at risk

Answers: which products are slow moving or at risk?

- Headline with the count, value at stake and how many could turn dead within 15 days.
- KPI cards: Slow Moving Products, At Risk Products, Value at Stake, Close to Turning Dead.
- **The slide towards dead stock:** three bars showing value at each stage.
- **Table:** the 15 products closest to dead stock.

## 04 Replenishment

Answers: which products need replenishment?

- Headline with how many products are out of stock or running low and still selling, and the cost to restore them.
- KPI cards: Out of Stock, Below Reorder Point, Approaching Reorder Point, Cost to Restore.
- **What should I do next:** three action cards in order.
  1. **Reorder now:** the cost and the names of the top products to start with.
  2. **Stop buying:** the value held in slow, at risk and dead products.
  3. **Release cash:** the dead stock value, and what recovering half at cost would mean for the restock.
- **Reorder list:** the top 15 with stock, reorder point, last sale, shortfall, estimated cost and priority.
- **Check demand before reordering:** low or empty products that have not sold in 90 days or more.
- **Do not reorder:** the largest slow, at risk and dead holdings.

## Inventory Explorer

Product-level detail for everyone.

- A search box that filters by product name.
- A status filter built from the statuses that actually exist in the data.
- A table of all products, largest value first.
- A live count of products displayed.
- Click any row to open a **product detail modal** with inventory value, units in stock, unit cost and movement status.

## Cross-cutting behaviour

| Behaviour | Detail |
|---|---|
| Light and dark theme | Toggle on every screen. The revenue chart redraws on change |
| Keyboard | Enter and Space activate buttons. Escape closes the product modal |
| Responsive | Layouts adapt at roughly 1000, 860 and 560 pixels |
| Reduced motion | Animations are effectively disabled when the user prefers reduced motion |
| Safe text | Product and category names are escaped for HTML before being written into the page |
