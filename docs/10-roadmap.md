# 10 Roadmap

## Near term: complete the portfolio version

- Add real screenshots from the final file.
- Fill in the base measure definitions and relationships in [06 Data model](06-data-model.md).
- Complete the validation plan in [09 Validation and testing](09-validation-and-testing.md) and record evidence.
- Resolve the `ALL(Calendar[Date])` question and document the decision.
- Reconcile figures that have two sources, such as dead stock value.

## Product improvements

| Idea | Value |
|---|---|
| Configurable thresholds | Let a user change the 31, 61 and 90 day rules without editing DAX |
| Lead time input | Make stock cover and reorder timing more realistic |
| Order quantity suggestions | Go beyond restoring the reorder point |
| Seasonality awareness | Avoid labelling seasonal products as slow or dead unfairly |
| Category level views | Compare health by category |
| Export | Let users export the reorder list and review lists |
| Product detail history | Show the sales history of a single product in the modal |

## Technical direction

The Power BI version proves the concept and the analytical logic. A longer-term step is a dedicated web application where:

- Business logic is separated from presentation.
- The interface is easier to maintain and test.
- Users can sign in, upload data and set their own thresholds.
- The same classification and replenishment rules apply, carried over from this project.

## Principle

Whatever the platform, the project's purpose stays the same: help a retailer understand the financial health of their inventory, avoid poor replenishment decisions and make better use of working capital.
