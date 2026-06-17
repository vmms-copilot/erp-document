# 08 — Promotions

Discount programs and loyalty-points configuration.

## setting/discount — Discounts
- List of discount programs/campaigns. Fields per program: name, type, value, scope, conditions, date range, active.
- Discount types (typical): percentage, fixed amount, buy-X-get-Y, bundle/combo, free shipping.
- Scope: all products, specific products/categories/brands, specific customer groups/levels, specific channels/depots.
- Conditions: min order value, min qty, applicable days/times, usage limits (per customer / total).
- Stacking rules: whether multiple promos combine or are exclusive.

## setting/point — Loyalty points config
- Earning rule: points per amount spent (e.g. 1 point per N VND), per-product/category overrides, multipliers by level.
- Redemption rule: point-to-VND conversion, min redeemable, max % of order payable by points.
- Expiry: point validity period.

### Business logic
- At sale time, the engine evaluates applicable discounts by scope + conditions, applies best/stacked per rules, and records which promo applied.
- Points earned = floor(eligible spend / rate) x level multiplier; redemption decrements balance and reduces payable.
- Promo usage is tracked for limits and reporting.

## Entities (for schema)
- Promotion (id, businessId, name, type, value, scopeType, scopeRefs, minOrder, minQty, startAt, endAt, usageLimit, perCustomerLimit, stackable, active)
- PromotionUsage (id, promotionId, orderId/billId, customerId, amount, usedAt)
- PointRule (id, businessId, earnRate, levelMultipliers, redeemRate, minRedeem, maxRedeemPercent, expiryDays)
