# 05 Architecture

## Overview

The application is a single DAX measure that writes a complete web page as text. Power BI evaluates the measure and the HTML Content custom visual displays the result.

```
+---------------------------------------------------------------+
| Power BI semantic model                                       |
| Products | Calendar | DailyStockSnapshot | StockTransactions   |
| Base measures: Inventory Value, Revenue, Units Sold, ...      |
+------------------------------+--------------------------------+
                               |
                               v
+---------------------------------------------------------------+
| Measure: Checkmystocks App                                    |
|  1. Read headline measures and set business rules             |
|  2. Compute recent sales velocity                             |
|  3. Build the product table in stages (_P1 to _P)             |
|  4. Summarise: counts, values, categories, ageing             |
|  5. Build replenishment groups                                |
|  6. Turn rows into HTML tables, bars and sentences            |
|  7. Join CSS, screens and JavaScript into one string          |
+------------------------------+--------------------------------+
                               |
                               v
+---------------------------------------------------------------+
| HTML Content custom visual                                    |
| Renders the string. JavaScript runs in the page               |
+---------------------------------------------------------------+
```

## The custom visual

| Property | Value |
|---|---|
| Name | HTML Content |
| Publisher | Daniel Marsh-Patrick |
| Visual ID | `htmlContent443BE3AD55E043BF878BED274D3A6855` |
| Version | 2.0.0.0 |

## Why one measure

Putting everything in one measure keeps the data and the interface in sync. When the model changes, the numbers on screen change with it, and there is nothing to export or refresh separately.

## The staged product table

The product-level table is built in four steps so each stage stays readable and avoids repeating work.

| Stage | Adds |
|---|---|
| `_P1` | One row per product with name, category, current stock, unit cost, reorder point, last sale date, days since last sale and units sold in the last 30 days |
| `_P2` | Inventory value, average daily units, HTML-safe names, and movement status |
| `_P3` | Status key, stock cover, replenishment condition and shortfall |
| `_P` | Purchase cost, priority rank, labels, days text and the finished HTML table row |

Products with a blank name are dropped at `_P2`.

## Server side versus browser side

Work is split on purpose.

| Done in DAX (when the measure evaluates) | Done in JavaScript (in the browser) |
|---|---|
| Product classification | Screen switching and navigation |
| All counts, values and percentages | KPI card expand and collapse |
| Ranking and top N lists | Revenue grouping by day, week and month |
| Replenishment groups and costs | Period presets and from and to sliders |
| HTML for tables and bars | Previous period comparison |
| Headline sentences | Chart drawing as SVG and hover tooltip |
| Daily revenue serialised as text | Product search and status filter |
| | Product detail modal |
| | Theme toggle and keyboard support |

## How revenue reaches the chart

DAX writes daily revenue as one text string of date and value pairs, for example `2026-01-01:12500,2026-01-02:9800`. JavaScript splits the string into rows and does all grouping and comparison in the browser. This is why the period buttons respond instantly without recalculating the model.

## Interface layers

| Section variable | Content |
|---|---|
| `_CSS` | Styling, themes, animation and responsive rules |
| `_Landing` | Landing page |
| `_Sidebar` | Navigation |
| `_ScOverview`, `_ScC1` to `_ScC4`, `_ScExplorer` | Screens |
| `_Modal` | Product detail modal |
| `_Script` | JavaScript |

## Safety measures

- Product and category names are escaped for `&`, `'`, `<` and `>` before being placed into HTML.
- The script guards against running twice by marking the app as initialised.
- All styles are scoped under `#checkmystocks` so they do not leak into the host page.

## Maintainability notes

- Business thresholds are variables near the top, so rules change in one place.
- The measure is long because it contains the whole interface. See [dax/README.md](../dax/README.md) for a guide to finding things.
- The roadmap discusses moving to a conventional web application.
