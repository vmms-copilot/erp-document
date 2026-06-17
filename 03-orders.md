# 03 — Orders

Order management covers the full sales-order lifecycle from creation to fulfilment, payment, and after-sales.

## order/manage/index — Order list
- Status tabs across the top, each backed by an order-status enum (observed values, typical lifecycle):
  - New / Confirming
  - Confirmed
  - Packing
  - Handed to carrier / Shipping
  - Delivered / Success
  - Returning / Returned
  - Cancelled
- Filters: keyword, customer, date range, depot, channel/source, carrier, payment status, employee.
- Columns: order code, customer, total, payment status, ship status, carrier, created date, employee.
- Row + bulk actions: confirm, print, pack, assign carrier, cancel, export.

## order/manage/add — Create order
Form sections:
- Customer: lookup or create inline (name, phone, address); shipping address.
- Items: product search -> add lines; each line has qty, unit price, discount; supports variants/IMEI.
- Pricing: subtotal, line/order discounts, shipping fee, VAT, grand total.
- Payment: method (cash/transfer/COD/card), amount paid, debt remainder.
- Shipping: carrier, service, weight, COD amount, depot to ship from.
- Source/channel: which sales channel the order came from.
- Notes, tags, employee.

### Business logic
- Order total = sum(line qty x price) - discounts + shipping + VAT.
- Debt = total - paid; unpaid balance flows to customer debt (see 07-accounting.md).
- Confirming an order reserves stock; handing to carrier decrements stock at the source depot.
- COD orders reconcile carrier remittance against order total.

## order/manage/checkduplicate
Detects likely duplicate orders by matching customer phone/address + items within a time window.

## order/packing
Packing queue: confirmed orders awaiting pack; scan/confirm items; print packing slip; mark packed.

## order/sendingcarrier
Handoff to carrier: select carrier + service, generate tracking, push to carrier API, print label.

## order/complain
Complaints/returns intake: link to order, reason, resolution status, refund/replacement.

## order/payment/index
Payments received against orders: order ref, method, amount, date, reconciliation status.

## order/deleted
Soft-deleted orders (recoverable); retains audit trail.

## Entities (for schema)
- Order (id, businessId, code, customerId, depotId, channelId, carrierId, employeeId, status, subtotal, discount, shippingFee, vat, total, paid, debt, codAmount, createdAt)
- OrderItem (id, orderId, productId, variantId, imeiId, qty, unitPrice, discount, lineTotal)
- OrderPayment (id, orderId, method, amount, paidAt, status)
- OrderStatusHistory (id, orderId, fromStatus, toStatus, userId, at)
- Complaint (id, orderId, reason, status, resolution, refundAmount)
- Carrier (id, name, services)
- Channel (id, name, type)
