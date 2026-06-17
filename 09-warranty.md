# 09 — Warranty

After-sales warranty tracking for sold goods (especially serialized/IMEI items).

## history/index — Warranty history
- Log of warranty cases: code, customer, product (+ IMEI/serial), linked sale (order/bill), issue description, received date, status, technician, resolution, return date.
- Status flow (typical): received -> diagnosing -> repairing / replacing / rejected -> completed -> returned to customer.
- Filters: keyword, customer, product, status, date range.

### Business logic
- Warranty eligibility derived from sale date + product warrantyMonths (see 02-products.md).
- Serialized items link the case to a specific IMEI; repairs/replacements may update IMEI status or issue a new serial.
- Replacements can trigger stock movement (issue replacement, write off faulty).
- Costs (parts/labour) can post to accounting.

## Entities (for schema)
- WarrantyCase (id, businessId, code, customerId, productId, imeiId, saleRefType, saleRefId, issue, status, technicianId, receivedAt, resolvedAt, returnedAt, resolution)
- WarrantyAction (id, caseId, type, note, cost, partsUsed, createdAt)
