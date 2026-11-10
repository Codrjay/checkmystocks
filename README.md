# Checkmystocks

**Know your stock. Stay in control.**

Checkmystocks is an inventory intelligence application built inside Microsoft Power BI. It turns raw stock and sales data into answers a retail owner can act on.

It is not a dashboard that lists quantities and revenue. It is a decision-support tool built around one idea: **inventory is money you have already spent, and the real question is what that money is doing for your business.**

> Screenshots: add your captures to the `screenshots/` folder and link them here. See [screenshots/README.md](screenshots/README.md).

---

## The problem

Most small retailers watch sales closely and inventory loosely. They can tell you what they sold last week, but not:

- How much cash is sitting on their shelves.
- How much of that cash is locked in products that have stopped selling.
- Which products are slowing down before they become a problem.
- Which products genuinely need to be reordered, and which only look like they do.

Checkmystocks answers these in plain language.

## The four questions

The whole application is organised around four questions a business owner actually asks.

| # | Question | Where it is answered |
|---|---|---|
| 1 | How much inventory do I currently have? | Current inventory |
| 2 | How much money is tied up in dead stock? | Dead stock |
| 3 | Which products are slow moving or at risk? | Slow moving and at risk |
| 4 | Which products need replenishment? | Replenishment |

Every screen opens with a one-sentence headline answer, generated from the live data, before showing any chart or table.

## What makes it different

1. **Money first.** Inventory is valued at cost, so every number reflects capital tied up, not potential revenue.
2. **Movement, not just quantity.** Products are classified by how long it has been since they last sold: Healthy, Slow Moving, At Risk or Dead Stock.
3. **Replenishment with a conscience.** A low-stock product that has not sold in 90 days is not added to the buy list. It goes into a separate "check demand before reordering" list. The application will not tell a retailer to buy more of something nobody is buying.
4. **Plain-language explanations.** Each section has a short explainer and clickable KPI cards that show what sits behind a number.
5. **Application feel.** A landing page, sidebar navigation, light and dark themes, product search, status filters and a product detail modal, all running inside Power BI.

## How it is built

A single DAX measure, `Checkmystocks App`, builds the entire interface as HTML, CSS and JavaScript text. The **HTML Content** custom visual renders that text inside Power BI.

```
Semantic model (tables and base measures)
        |
        v
DAX measure builds product-level data and every number on screen
        |
        v
HTML + CSS + JavaScript assembled as one string
        |
        v
HTML Content visual renders it as an interactive application
```

Read more in [docs/05-architecture.md](docs/05-architecture.md).

## Documentation map

You do not need to read the code to understand this project. Start with the business thinking and go deeper only if you want to.

| Document | What it covers |
|---|---|
| [01 Business problem](docs/01-business-problem.md) | The problem, the user and the positioning |
| [02 Analytical decisions](docs/02-analytical-decisions.md) | Why each rule and threshold was chosen |
| [03 Business rules](docs/03-business-rules.md) | The exact classification and replenishment rules |
| [04 Application walkthrough](docs/04-application-walkthrough.md) | Every screen and interaction |
| [05 Architecture](docs/05-architecture.md) | How DAX, HTML, CSS and JavaScript fit together |
| [06 Data model](docs/06-data-model.md) | Tables, fields and the base measures the app depends on |
| [07 Calculation reference](docs/07-calculation-reference.md) | Every calculation the application performs itself |
| [08 Limitations and caveats](docs/08-limitations-and-caveats.md) | What it does not do and what to watch for |
| [09 Validation and testing](docs/09-validation-and-testing.md) | What was checked and the test plan |
| [10 Roadmap](docs/10-roadmap.md) | Where the project goes next |
| [Setup guide](docs/setup-guide.md) | How to run it in your own Power BI file |

## Repository structure

```
checkmystocks/
|-- README.md
|-- CHANGELOG.md
|-- dax/
|   |-- README.md
|   `-- Checkmystocks_App.dax       (the full application measure)
|-- docs/
|   |-- 01-business-problem.md
|   |-- 02-analytical-decisions.md
|   |-- 03-business-rules.md
|   |-- 04-application-walkthrough.md
|   |-- 05-architecture.md
|   |-- 06-data-model.md
|   |-- 07-calculation-reference.md
|   |-- 08-limitations-and-caveats.md
|   |-- 09-validation-and-testing.md
|   |-- 10-roadmap.md
|   `-- setup-guide.md
`-- screenshots/
    `-- README.md
```

## Tech summary

| Layer | Technology |
|---|---|
| Platform | Microsoft Power BI |
| Logic and data shaping | DAX |
| Interface | HTML and CSS |
| Interactivity and charting | JavaScript, with the revenue chart drawn as SVG |
| Renderer | HTML Content custom visual (Daniel Marsh-Patrick, version 2.0.0.0) |

## About

Built by Emmanuel James, The Data Guy, a business intelligence analyst who creates data solutions and BI products for small and medium businesses.

The goal of this project is to show the business problem behind the data, and a working answer to it.
