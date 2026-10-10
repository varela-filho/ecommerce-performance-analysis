# Project 01 | Nordmarkt E-commerce Performance Investigation

**Role:** Junior Data Scientist, consulting analytics team  
**Client:** Nordmarkt (fictional German e-commerce retailer)  
**Analysis period:** 2025-01-01 through 2025-12-31  
**Reporting currency:** EUR  
**Data:** Fully synthetic. No real customers or business records.

## Client request
Management suspects that revenue deteriorated during the most recent quarter. Establish whether this is supported by the data, quantify the magnitude, investigate plausible explanations, and recommend prioritized next actions. Do not treat management's suspicion as an established cause.

## Questions to answer
1. How did revenue and order activity evolve by week/month/quarter? Which revenue definition are you using?
2. Is the change related to order volume, average order value, units per order, product mix, discounts, order status, or some combination?
3. Which product categories, acquisition channels, devices, regions, and customer cohorts merit attention?
4. What do website sessions and marketing spend suggest? Which metrics are comparable, and which are only proxies?
5. How reliable are the data, and how sensitive are conclusions to cleaning choices?
6. What should management do first, and how will success be measured?

## Required deliverables
- `sql/`: documented SQL queries covering joins, aggregations, time comparisons, and data-quality checks
- `notebooks/` or `src/`: reproducible Python EDA and analytical investigation
- `dashboards/`: Power BI report, or screenshots plus instructions if sharing a PBIX is impractical
- `reports/`: concise executive deck (suggested 6–10 slides) and a 1-page recommendations summary
- `README.md`: problem, approach, findings, assumptions, limitations, how to reproduce

## Scope and evaluation
- Work independently; no predictive model is required.
- Quantify each major claim, show denominators, and separate observations from causal hypotheses.
- Use at least two independent checks for your most important conclusion.
- Include a data-quality log with issue counts and remediation choices.
- Treat cancellations/returns and discount fields deliberately, not implicitly.
- Do not assert session-to-order conversion at the individual level: these files do **not** include a shared session-to-order identifier.
- Present business actions, estimated upside when defensible, implementation priority, and risks.

## Suggested sequence (not a solution)
1. Import and inspect table keys and date ranges.
2. Audit completeness, uniqueness, validity, referential integrity, and types.
3. Define business metrics and construct analysis-ready tables.
4. Explore time patterns and segment differences.
5. Test competing explanations and check robustness.
6. Build the dashboard and executive recommendation.

## Working assumptions
- This is a fictional scenario, not evidence about the actual German retail market.
- Order items contain line-level prices and discounts; order-level discount is a separate field. Define your net sales logic explicitly and avoid double counting.
- `order_status` distinguishes `completed`, `cancelled`, and `returned`; choose an appropriate revenue treatment and document it.
- Website sessions are channel/day-level traffic observations; marketing spend is recorded per channel/day.
- `products.list_price_eur` is reference product metadata, not necessarily the actual realized transaction price.
- Data include intentional quality defects. Do not assume every field is clean or every business definition unambiguous.

## Constraints
Use free/local tools only. You are responsible for the analysis; the senior reviewer will provide feedback on your methods and conclusions.
