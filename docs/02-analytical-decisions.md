# 02 Analytical decisions

This document explains why the application works the way it does. Each section states the decision, the reasoning and the trade-off.

---

## Decision 1: Value inventory at cost, not at selling price

**Rule:** Inventory Value = Current Stock x Unit Cost.

**Why:** The question being answered is how much money the retailer has tied up. That is what they paid, not what they hope to receive. Valuing at selling price would overstate exposure and mix in margin, which is a separate topic.

**Trade-off:** The figure does not show potential revenue. It is a capital measure, not a sales potential measure.

---

## Decision 2: Classify by time since last sale

**Rule:** Products are placed in a status based on days since their last sale.

| Status | Condition |
|---|---|
| No Stock | Current stock is zero or less |
| No Sales | No last sale date exists |
| Healthy | 0 to 30 days |
| Slow Moving | 31 to 60 days |
| At Risk | 61 to 89 days |
| Dead Stock | 90 days or more |

**Why:** Not every unsold product is the same. A product that last sold 40 days ago is behaving differently from one that last sold 100 days ago, and the owner should respond differently. Having stages gives early warning before something becomes a loss.

**Why 90 days for dead stock:** It is a business rule for this project, chosen as a practical cut off. It is not a universal retail standard, and a business with seasonal or slow-turning products may want a different number. The thresholds live in one place at the top of the DAX so they can be changed easily.

**Order of checks:** No Stock is tested first, then No Sales, then the day thresholds. A product with no stock is not called "dead" because there is nothing to be stuck.

---

## Decision 3: Use recent sales, not lifetime sales, to judge demand

**Rule:** Demand is measured over the 30 calendar days ending on the latest date with recorded sales.

**Why:** Lifetime averages hide change. A product that sold heavily last year and little now will look healthy on a long average. Recent activity is more relevant to a buying decision made today.

**Why anchor to the latest sales date, not today's date:** It keeps the figure meaningful when the data ends before the current date.

**Trade-off:** Thirty days can be noisy for products that sell infrequently. Recent demand is also not guaranteed to continue.

---

## Decision 4: Stock cover as an estimate

**Rule:** Stock Cover = Current Stock / Average Daily Units. It is left blank when average daily units are zero.

**Why:** "How many days will this last?" is a more useful question than "how many do I have?". It connects stock to demand.

**Trade-off:** It is an estimate at the observed rate. It should be read alongside supplier lead time, which the application does not know.

---

## Decision 5: Reorder logic is not just stock against reorder point

**Rule:** A product is flagged for replenishment when it is out of stock, at or below its reorder point, or within 20 percent above its reorder point. Then it is split into two groups:

- **Reorder now:** has a valid last sale date and sold within the last 90 days.
- **Check demand first:** has no sale on record, or has not sold for 90 days or more.

**Why:** A simple stock-versus-reorder-point rule would send the retailer to buy products that nobody is buying. Separating the two groups protects working capital. This is the most important analytical decision in the project.

**The 20 percent approach buffer:** Gives early notice so the owner can plan a purchase before the product runs short.

---

## Decision 6: Shortfall restores to the reorder point

**Rule:** Shortfall = MAX(0, Reorder Point - Current Stock). Estimated Purchase Cost = Shortfall x Unit Cost.

**Why:** It gives a conservative, comparable estimate of what it takes to bring selling products back to their trigger level. MAX with zero prevents negative quantities.

**Trade-off:** This is a floor, not a recommended order quantity. It ignores supplier minimums, lead time and demand growth.

---

## Decision 7: "Do not reorder" means "not now"

The "Do not reorder" list contains products that are slow moving, at risk or dead. It does not mean the product should never be bought again. It means that, given its current stock position and movement, buying more now would tie up more cash. A healthy stock position today does not rule out replenishment later.

---

## Decision 8: Make the dead stock number actionable

**Rule:** The replenishment screen compares half of the dead stock value with the cost of the restock.

**Why:** It reframes dead stock as a source of funds. If recovering half the value at cost could pay for the whole restock, the retailer can see the connection between two problems.

**Trade-off:** "Half" is an illustrative scenario, not a forecast of what can be recovered.

---

## Decision 9: Directional estimates, not forecasts

The "Next 7 days" figures multiply a recent daily average by seven. They are labelled as directional estimates. They are not statistical forecasts and should not be presented as such.

---

## Decision 10: Plain language first

Each screen opens with a sentence built from live numbers, such as how much inventory is worth and what share is dead stock. The reasoning is that an owner should get the answer before the evidence. Charts and tables then support it.

---

## Decision 11: Build the interface inside the BI tool

**Why:** The goal was an application that feels like a product, with navigation, a landing page, themes, search and a detail view, while staying inside Power BI where the data and the model already live.

**Trade-off:** It relies on a custom visual and on long DAX text generation, which is harder to maintain than a conventional web application. See the roadmap for the planned path.
