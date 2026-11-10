# Changelog

All notable changes to this project are recorded here.

## [Unreleased]

- Add real screenshots from the final Power BI file.
- Repeat calculation validation against the final file and record the evidence.
- Test the `ALL(Calendar[Date])` behaviour against report-level date slicers and document the result.
- Document base measure definitions and table relationships from the Power BI file.

## [1.0.0] Initial application

### Added
- Landing page with product introduction and entry into the workspace.
- Overview screen with headline sentence, four clickable KPI cards, revenue trend, next 7 days estimate and business performance summary.
- Four question screens: Current inventory, Dead stock, Slow moving and at risk, Replenishment.
- Inventory Explorer with product search, status filter and product detail modal.
- Inventory movement classification: Healthy, Slow Moving, At Risk, Dead Stock, No Stock, No Sales.
- Replenishment logic that separates "reorder now" from "check demand first".
- Revenue chart with period presets (30 days, 90 days, 12 months, all time), day, week and month grouping and previous period comparison.
- Light and dark theme toggle, responsive layout and keyboard support.
- Full documentation set.
