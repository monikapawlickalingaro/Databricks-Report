# Report Map — Retail Revenue & Supply Report

> Filled in from the Databricks AI/BI Dashboard structure (pages, widgets, datasets, filters),
> based on the TPC-H sample data in Unity Catalog.

## 0. Report Overview

| Field | Value |
|---|---|
| Client | Internal analytics — based on Databricks TPC-H sample data (samples.tpch) |
| Report name | Retail Revenue & Supply Report |
| Dashboard ID | 01f18b9f63f21fd882e303621bf1510a |
| Number of pages | 2 (Summary · Most Valuable Customers) |
| Data refresh frequency | On-demand — uses static TPC-H sample dataset in Unity Catalog; not scheduled for refresh |
| Primary audience | Sales analysts and supply chain managers analyzing revenue trends, customer value, shipping patterns, and supplier metrics |
| Report language | EN |

## 1. Report Pages

### Page 1: Summary

- **Purpose of the page:** Provide a high-level overview of revenue performance, order priorities, shipping methods, customer and supplier counts, and revenue trends by nation. Acts as the landing page for quick KPI assessment.
- **Key metrics / visuals:**
  - **Unique Suppliers** (counter) — distinct supplier count from lineitem
  - **Unique Customers** (counter) — distinct customer count from orders
  - **Revenue by Order Priority** (pie chart) — revenue share split by order priority level
  - **Most Valuable Customers** (table) — top customers with color-coded revenue bands
  - **Revenue Trend** (bar chart) — revenue over time broken down by nation
  - **Top Shipping Methods** (heatmap) — order count by order priority × shipping method
  - **Revenue by Order Priority** (line chart) — daily total price trend by priority (Jan 1994)
- **Local filters on this page:** Date Range (date-range-picker on `o_orderdate`)
- **Typical user questions on this page:**
  - "What is our total revenue by order priority?"
  - "Which shipping method handles the most urgent orders?"
  - "How does revenue trend across different nations?"
  - "How many unique suppliers and customers do we have?"

### Page 2: Most Valuable Customers

- **Purpose of the page:** Deep-dive analysis of the top 100 highest-revenue customers — their segments, geographies, order frequency, and value metrics. Supports customer segmentation and retention strategy.
- **Key metrics / visuals:**
  - **Total Revenue — Top 100** (counter) — sum of total revenue for top 100 customers
  - **Avg Order Value** (counter) — average order value across top customers
  - **Avg Orders per Customer** (counter) — average number of orders per top customer
  - **Customer Count** (counter) — count of customers shown (100)
  - **Revenue by Segment** (bar chart) — total revenue split by market segment (Building, Machinery, Automobile, Household, Furniture)
  - **Revenue by Nation** (bar chart) — total revenue by customer nation (25 nations)
  - **Top 100 Customers** (table) — detailed table with customer name, segment, nation, order count, total revenue, avg order value, first/last order dates
- **Local filters on this page:** Customer Segment (multi-select), Nation (multi-select), Order Date Range (date-range-picker on `first_order_date`)
- **Typical user questions on this page:**
  - "Who are our highest-revenue customers?"
  - "Which market segment generates the most revenue?"
  - "How are top customers distributed across nations?"
  - "What is the average order value for our best customers?"

## 2. KPI Dictionary

| KPI | Business definition | Unit / format | Where in the report | Notes |
|---|---|---|---|---|
| Unique Suppliers | Count of distinct `l_suppkey` from lineitem | Integer | Summary — counter | Counts suppliers with at least one line item |
| Unique Customers | Count of distinct `o_custkey` from orders | Integer | Summary — counter | Counts customers with at least one order |
| Revenue (Nation Trend) | `SUM(l_extendedprice * (1 - l_discount) * adjustment_factor)` | Currency ($) | Summary — Revenue Trend bar chart | Filtered to 8 selected nations only (Argentina, UK, France, Brazil, China, US, Japan, Jordan) |
| Total Price | `SUM(o_totalprice)` | Currency ($) | Summary — Revenue by Order Priority line chart | Limited to January 1994 date range |
| Order Count | `COUNT(*)` of orders × lineitem | Integer | Summary — Top Shipping Methods heatmap | Grouped by priority and ship mode |
| Total Customer Revenue | `SUM(o_totalprice)` per customer | Currency ($) | Summary — Most Valuable Customers table | Color-coded: blue ($0–$1.5M), yellow ($1.5M–$3M), red ($3M–$5M), gray ($5M+) |
| Total Revenue (Top Customers) | `SUM(o_totalprice)` for top 100 customers | Currency ($) | Most Valuable Customers — counter & charts | Ranked by total revenue descending |
| Avg Order Value | `AVG(o_totalprice)` per customer | Currency ($) | Most Valuable Customers — counter | Average across each customer's orders |
| Order Count (per customer) | `COUNT(DISTINCT o_orderkey)` per customer | Integer | Most Valuable Customers — counter & table | Distinct orders per customer |

## 3. Filters

### Global filters (apply to the whole report)
| Filter | Values | Default setting | Notes |
|---|---|---|---|
| *(none at report level)* | — | — | Each page has its own independent filters |

### Local filters (per page)
| Page | Filter | Type | Field | What it affects |
|---|---|---|---|---|
| Summary | Date Range | Date range picker | `o_orderdate` | All visuals on Summary page |
| Most Valuable Customers | Customer Segment | Multi-select | `segment` | All visuals on MVC page |
| Most Valuable Customers | Nation | Multi-select | `nation` | All visuals on MVC page |
| Most Valuable Customers | Order Date Range | Date range picker | `first_order_date` | All visuals on MVC page |

## 4. Data Sources

| Dataset Name | Source Tables | Key Columns | Used By |
|---|---|---|---|
| Orders and Customers | `samples.tpch.orders` | o_orderdate, o_custkey | Summary — Unique Customers counter |
| Orders vs Returns | `samples.tpch.lineitem` | l_shipdate, l_suppkey, total_orders, return_count | Summary — Unique Suppliers counter |
| Overall Supplier Count | `samples.tpch.supplier` | s_suppkey | (reference dataset) |
| Order count by priority and ship mode | `samples.tpch.orders`, `samples.tpch.lineitem` | priority, ship_mode, order_count, o_orderdate | Summary — Top Shipping Methods heatmap |
| Revenue Trends by Nation | `samples.tpch.customer`, `samples.tpch.orders`, `samples.tpch.lineitem`, `samples.tpch.nation` | o_orderdate, nation, revenue | Summary — Revenue Trend bar chart |
| Revenue by Order Priority | `samples.tpch.orders` | Date, Priority, Total Price | Summary — Revenue by Order Priority line/pie |
| Most Valuable Customers | `samples.tpch.orders`, `samples.tpch.customer`, `samples.tpch.region` | Customer ID, Customer Segment, Total Customer Revenue | Summary — Most Valuable Customers table |
| Top Customers Analysis | `samples.tpch.customer`, `samples.tpch.orders`, `samples.tpch.nation` | customer_id, customer_name, segment, nation, order_count, total_revenue, avg_order_value, first_order_date, last_order_date | Most Valuable Customers page — all widgets |

## 5. Navigation & Interactions

- **Page-to-page navigation:** Two page tabs — Summary and Most Valuable Customers.
- **Cross-filtering:** Filters on each page apply to all widgets sharing the same dataset on that page.
- **No drill-through:** Exploration happens via filter selections; no drill-through pages configured.
- **Resetting filters:** Clear filter selections to return to unfiltered state.

## 6. Genie Agent — natural-language data queries

This dashboard has a **Genie Agent** attached. Genie is Databricks' own
conversational, natural-language-to-SQL interface — separate from this
project's voice trainer (LISA), and important to understand the boundary
between the two:

- **What Genie does:** the user types (or, depending on how it's surfaced,
  asks) a data question in plain English, and Genie generates and runs a
  SQL query against the underlying Unity Catalog tables directly, then
  returns an actual, live answer — including specific numbers.
- **How a user gets to it:** typically via a chat-style entry point
  attached to the dashboard (exact placement depends on how the workspace
  admin has surfaced it — e.g. a "Ask Genie" button/panel on the
  dashboard, or a linked Genie Space). Confirm the exact access point for
  this workspace before writing user-facing instructions.
- **Important nuance — Genie can potentially answer some things this
  dashboard's fixed visuals do not show.** Because Genie queries the
  underlying tables directly rather than reading only what's rendered on
  the dashboard, it may be able to answer questions from Section 7 below
  that the dashboard itself can't — for example, return-related fields
  exist in the `lineitem` table (see Section 4, "Orders vs Returns")
  even though no visual surfaces them. This has **not been verified
  question-by-question** for this specific Genie instance — treat it as
  a hypothesis to test, not a confirmed capability, before telling a user
  "ask Genie" for anything specific.
- **What Genie likely still can't answer:** things the underlying data
  itself doesn't contain — e.g. profit/margin (no cost data anywhere in
  this dataset) or true customer churn classification (the raw dates
  exist, but nothing computes churn from them). Genie can only query what
  exists in the tables; it can't invent columns that aren't there.

### How this shapes the voice agent's (LISA's) role

LISA teaches **structure** — pages, filters, what a metric means, what a
chart deliberately does or doesn't include — and, per this project's
hard guardrail, **never states a live figure itself.** When a user asks
for an actual number or a question the dashboard's fixed visuals can't
answer, LISA's correct move is to say so plainly and, where relevant,
mention that Genie (if available on this dashboard) can run that query
directly — not to attempt the answer itself, and not to assume Genie's
exact scope without it having been tested.

## 7. What the Report Does NOT Show

- **Individual order detail** — no line-item level drill-down to specific orders or products.
- **Supplier performance analysis** — supplier count is shown but no supplier-level revenue, quality, or delivery metrics.
- **Return rate analysis** — return data exists in the Orders vs Returns dataset but is not surfaced in any visual currently.
- **Time-series forecasting** — no predictive or anomaly detection visuals.
- **Real-time data** — uses static TPC-H sample data; not connected to live transactional systems.
- **Profit or margin** — only revenue (total price / extended price) is shown; no cost or profit metrics.
- **Customer churn or retention** — first/last order dates are available but no churn classification is computed.

> Note: as of Section 6 above, some of these gaps (e.g. return rate) may
> be answerable via the dashboard's attached Genie Agent even though the
> dashboard's own visuals don't show them — this list describes the
> **dashboard**, not the full extent of what's queryable through Genie.
