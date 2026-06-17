# 04 — Point of Sale (POS)

In-store selling interface optimised for speed (keyboard shortcuts, depot scoping).

## bill/index — POS bill list
- Lists POS sales (bills) with: bill code, depot, cashier, customer, total, payment method, date, status.
- Filters: depot, date range, cashier, payment method, status.

## bill/add — New sale
- On open, a depot-select modal asks which depot/till the sale belongs to (one of PHANH / PHANH 2 / PHANH 3).
- Layout: product search/scan on the left, cart on the right, totals + payment at the bottom.
- F-key shortcuts observed for fast operation (add line, change qty, apply discount, pay, new bill).
- Cart line: product/variant, qty, unit price, discount, line total.
- Customer can be attached (for loyalty/points/debt) or left as walk-in.
- Payment: cash / transfer / card / mixed; change calculated; supports partial payment -> debt.

### Business logic
- Completing a bill decrements stock at the selected depot immediately.
- Loyalty points accrue to attached customer (see 06-customers.md / 08-promotions.md).
- Discounts/promotions applied at line or bill level.
- Prints receipt; optionally issues e-invoice.

## return/index — Returns
- Returns against a prior bill: select bill, choose lines/qty to return, reason, refund method.
- Restocks returned items at the depot; reverses points.

## invoice/index — Invoices
- E-invoice / VAT invoice issuance and listing tied to bills/orders.

## debt/bill — POS debt
- Outstanding balances from partially-paid bills; ties into customer debt ledger.

## Entities (for schema)
- PosBill (id, businessId, code, depotId, cashierId, customerId, subtotal, discount, vat, total, paid, change, paymentMethod, status, createdAt)
- PosBillItem (id, billId, productId, variantId, qty, unitPrice, discount, lineTotal)
- PosReturn (id, billId, reason, refundMethod, total, createdAt)
- PosReturnItem (id, returnId, billItemId, qty, amount)
- Invoice (id, billId/orderId, number, vat, total, status)
