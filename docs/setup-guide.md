# Setup guide

How to run Checkmystocks in your own Power BI file.

## Requirements

- Power BI Desktop.
- The **HTML Content** custom visual by Daniel Marsh-Patrick (version 2.0.0.0 was used).
- A semantic model with the tables and base measures listed in [06 Data model](06-data-model.md).

## Steps

1. **Prepare the model.** Create or confirm the tables `Products`, `Calendar`, `DailyStockSnapshot` and `StockTransactions`, with the fields listed in the data model document.
2. **Create the base measures.** Define `[Inventory Value]`, `[Dead Stock Value]`, `[Out-of-Stock Products]`, `[Inventory Turnover]`, `[Revenue]`, `[COGS]`, `[Gross Margin %]`, `[Current Stock Units]`, `[Last Sale Date]`, `[Days Since Last Sale]` and `[Units Sold]`.
3. **Add the application measure.** Create a new measure named `Checkmystocks App` and paste in the contents of [dax/Checkmystocks_App.dax](../dax/Checkmystocks_App.dax).
4. **Add the visual.** Import the HTML Content custom visual from the visuals marketplace and place it on a blank report page.
5. **Bind the measure.** Drag `Checkmystocks App` into the visual's values field.
6. **Size the page.** Make the visual fill the whole page and hide page chrome where possible so it feels like an application.
7. **Test.** Work through the checklist in [09 Validation and testing](09-validation-and-testing.md).

## Customising the rules

Edit the threshold variables near the top of the measure.

| Variable | Default | Meaning |
|---|---|---|
| `_SlowDays` | 31 | Days since last sale where Slow Moving begins |
| `_AtRiskDays` | 61 | Days where At Risk begins |
| `_DeadDays` | 90 | Days where Dead Stock begins |
| `_SoonDays` | 15 | Window before dead stock that counts as close to turning dead |
| `_ApproachBuffer` | 1.2 | Stock within this multiple of the reorder point counts as approaching |

Some explanatory text in the interface refers to these variables, so it updates when they change. The dead stock ageing bands (90, 120, 180, 365 days) are written directly into the measure and should be reviewed if `_DeadDays` is changed.

## Troubleshooting

| Symptom | Likely cause |
|---|---|
| Blank visual | The measure is not bound to the visual, or the base measures are missing |
| Plain fonts | External font request blocked, so the fallback font is used |
| Revenue chart empty | No revenue rows returned by `[Revenue]` |
| Numbers differ from a standard report | Filter context. See [08 Limitations and caveats](08-limitations-and-caveats.md) |
