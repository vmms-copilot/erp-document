# 13 — Reports

Analytics across sales, inventory, finance, customers, and staff. ~79 report variants observed; all share a common pattern: filter bar (date range, depot, channel, employee, category) + tabular/chart output + export.

## report/revenue/index — Revenue report
- Revenue over a period, broken down by day/week/month, depot, channel, product/category, or employee.
- Core metrics: gross revenue, discounts, net revenue, COGS, gross profit, orders count, average order value, units sold.
- Formulas:
  - Net revenue = gross revenue - discounts - returns
  - COGS = sum(unit cost x qty sold)
  - Gross profit = net revenue - COGS
  - AOV = net revenue / orders

## Report families (representative)
- Sales: by time, by product, by category, by employee, by channel, by customer.
- Inventory: stock on hand, stock movement, slow movers (storage time), import/export value, damaged.
- Finance: cash flow, receivables/payables aging, profit & loss.
- Customer: new vs returning, top customers, loyalty/points.
- Promotion: promo usage and impact.

### Business logic
- Reports are read models aggregating orders, POS bills, stock movements, and journal entries.
- All scoped by businessId and the selected filters; depot scoping respects user permissions.
- Money in integer VND; periods derived from local timezone.

## Notes for rebuild
- Implement as materialized/aggregated read models or query-time aggregations over the transactional tables defined in 14-database-schema.md.
- Provide consistent export (CSV/Excel) and common filter component shared across all report screens.
