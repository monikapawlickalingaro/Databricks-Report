# Q&A Knowledge Base — Retail Revenue & Supply Report

> 20 question/answer pairs, grouped by topic. Hard rule, unchanged from
> the Power BI project: **no answer states a specific numeric value** —
> every answer points to the visual where the user can see it themselves.

---

## Navigation

**Q: Where can I see revenue broken down by nation?**
A: Go to the Summary page and look at the Revenue Trend bar chart — it shows revenue over time, broken down by nation.

**Q: Where do I find our top customers?**
A: The Most Valuable Customers page has a dedicated table listing the top 100 customers, ranked by total revenue.

**Q: How do I see revenue split by market segment?**
A: On the Most Valuable Customers page, the Revenue by Segment bar chart splits total revenue across market segments — Building, Machinery, Automobile, Household, and Furniture.

**Q: Where can I check which shipping method is used most for urgent orders?**
A: The Top Shipping Methods heatmap on the Summary page shows order counts by order priority crossed with shipping method — that's where priority-vs-method patterns live.

---

## Metrics & KPIs

**Q: How is "Unique Suppliers" calculated?**
A: It's a distinct count of suppliers that appear on at least one line item — shown as a counter on the Summary page.

**Q: How is "Unique Customers" calculated?**
A: It's a distinct count of customers who have placed at least one order — also a counter on the Summary page.

**Q: What does "Total Customer Revenue" mean in the Most Valuable Customers table on the Summary page?**
A: It's the sum of order totals for that customer, and the table color-codes customers into revenue bands so you can spot high-value customers at a glance.

**Q: What's the difference between the revenue counter on the Most Valuable Customers page and the revenue shown in the Summary table?**
A: The Most Valuable Customers page counter aggregates revenue across the top 100 customers as a group, while the Summary table shows each customer's individual total — they answer different questions, not the same one from two angles.

**Q: How is "Avg Order Value" calculated on the Most Valuable Customers page?**
A: It's the average order total, calculated per customer across all of that customer's orders, then shown as an overall average across the top customers.

**Q: What counts as an "order" in the Top Shipping Methods heatmap?**
A: The heatmap counts order and line-item combinations grouped by priority and shipping method — it's not a simple count of unique orders, so don't expect it to match a plain order count elsewhere in the report.

---

## Filters

**Q: If I filter the Summary page by date, does that also filter the Most Valuable Customers page?**
A: No — there are no report-wide filters here. Each page has its own independent filters, so a change on one page never affects the other.

**Q: What does the Date Range filter on the Summary page actually control?**
A: It filters orders by order date and affects every visual on the Summary page at once.

**Q: Can I filter the top customers list by country?**
A: Yes — the Most Valuable Customers page has a Nation filter (multi-select) that narrows the customer table and its charts to selected countries.

**Q: What happens if I clear my filter selections?**
A: The page returns to its default, unfiltered state — every visual on that page shows the full data again.

**Q: Why does the Revenue Trend chart on the Summary page only show a handful of countries?**
A: That chart is deliberately limited to a fixed set of nations — it isn't showing all countries in the dataset, so don't read it as a global revenue trend. If you need all countries, use the Most Valuable Customers page's Revenue by Nation chart instead.

**Q: Why does the Revenue by Order Priority line chart only seem to cover one month?**
A: That specific chart is scoped to a single month's date range by design — it's meant to show a detailed daily pattern, not the full history. It's not a filter you can change; it's built into the chart.

---

## What the report does not show

**Q: Can I see individual order or line-item details?**
A: No — this report works at an aggregated level. There's no drill-down to a specific order or product line item.

**Q: Does this report show profit or margins?**
A: No — every revenue figure here is based on order price, not cost. There's no profit or margin metric anywhere in this report.

**Q: Can I see supplier performance, quality, or delivery metrics?**
A: No — the report shows how many distinct suppliers exist, but nothing about how well any individual supplier performs.

**Q: Is this live, up-to-date data?**
A: No — this report runs on a static sample dataset, not a live connection. Figures won't reflect real-time transactions.

**Q: Can I see customer churn or retention?**
A: Not directly — the data includes each customer's first and last order dates, but the report doesn't calculate or display any churn or retention classification from them.
