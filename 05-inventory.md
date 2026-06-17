# 05 — Inventory (Import / Export / Stock)

Warehouse operations across the 3 depots: imports, exports, stocktakes, transfers, positions, damaged goods.

## bill/index — Import/Export bill list
- Lists stock movement bills (import receipts, export issues) with: code, type, depot, supplier/partner, value, status, date.
- Filters: type (import/export), depot, supplier, date range, status.

## bill/import — Create import
- Import type enum (observed): supplier (Nha cung cap) / other (Khac).
- Fields: depot (destination), supplier, import date, reference, lines (product, qty, unit cost, lot/expiry, IMEI if serialized), totals, notes.
- Posting increments stock at the destination depot and creates an accounting entry (inventory value up, payable/cash).

## check/index — Stocktake
- Physical count session per depot: system qty vs counted qty -> variance; approve to adjust stock.

## transfer/index — Inter-depot transfer
- Move stock between depots: source depot, destination depot, lines, qty; in-transit then received.
- Decrements source on dispatch, increments destination on receipt.

## archive/index — Archive
- Archived/closed stock documents for record retention.

## position/index — Bin/shelf positions
- Storage locations within a depot (zone/shelf/bin); products mapped to positions for picking.

## product/damaged — Damaged goods
- Records damaged/written-off stock: product, qty, reason, depot; decrements sellable stock, posts loss.

### Business logic
- Every stock movement is depot-scoped and creates an audit record.
- Weighted-average or FIFO cost (cost tracked on import lines; powers COGS in reports).
- Stocktake variances and damages reduce stock and post to accounting.

## Entities (for schema)
- ImExBill (id, businessId, code, type, depotId, partnerId, refDepotId, totalValue, status, createdAt)
- ImExBillItem (id, billId, productId, variantId, batchId, imeiId, qty, unitCost, lineValue)
- StockCheck (id, depotId, status, createdAt) / StockCheckItem (id, checkId, productId, systemQty, countedQty, variance)
- Transfer (id, fromDepotId, toDepotId, status, createdAt) / TransferItem (id, transferId, productId, qty)
- Position (id, depotId, zone, shelf, bin) / ProductPosition (productId, positionId)
- DamagedStock (id, depotId, productId, qty, reason, createdAt)
