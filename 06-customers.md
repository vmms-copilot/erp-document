# 06 — Customers

Customer master data, segmentation, care activities, and loyalty.

## code/customerlist — Customer list
- Server-side paged list of customers. Columns: code, name, phone, email, group, level, total spend, debt, last order, created.
- Filters: keyword, group, level, source/channel, date range, has-debt.
- Actions: add, edit, merge, export, view 360 (orders/debt/points history).

## care/index — Customer care
- Care/CRM activity log: customer, activity type (call/message/visit), note, assigned employee, due/next-action date, status.
- Used for follow-ups, win-back, after-sales.

## group/index — Customer groups
- Segmentation groups (observed: VIP1 / VIP2 / VIP3 and others). Fields: name, criteria/notes, default discount, order.
- A customer belongs to a group; group can drive pricing/discounts.

## store/level — Loyalty tiers
- Loyalty levels with thresholds (e.g. spend or points) and benefits (discount %, point multiplier).
- Customer level is derived from accumulated spend/points.

### Business logic
- Customer 360 aggregates orders, POS bills, debt, points, and care history.
- Points accrue from purchases (config in 08-promotions.md) and can be redeemed.
- Debt = sum of unpaid order/bill balances (see 07-accounting.md).
- Group vs Level: group = manual/marketing segment; level = automatic loyalty tier.

## Entities (for schema)
- Customer (id, businessId, code, name, phone, email, address, groupId, levelId, points, totalSpend, debt, source, createdAt)
- CustomerGroup (id, name, defaultDiscount, criteria, order)
- CustomerLevel (id, name, threshold, discountPercent, pointMultiplier, order)
- CareActivity (id, customerId, type, note, employeeId, nextActionAt, status, createdAt)
- CustomerPointLog (id, customerId, refType, refId, points, balanceAfter, createdAt)
