# 02 — Products

Covers the product catalog: items, variants, IMEI/serials, batches, brands, categories, and per-depot inventory.

## product/item/index — Product list
- Server-side paged list. Columns: image, name, code (SKU), barcode, category, brand, retail price, stock-per-depot, status.
- Filters: keyword, category, brand, supplier, status (active/inactive), depot, price range.
- Bulk actions: export, print barcodes, set status, edit prices.

## product/item/add — Add product
Form fields captured (with sample fashion-shop data used to probe structure):
- name (string, required) — e.g. `Ao So Mi Linen Nam Tay Dai`
- code / SKU (string) — e.g. `SM-LINEN-001`
- barcode (string) — e.g. `8938000123456`
- unit (enum/lookup) — e.g. Cai
- category (lookup, hierarchical)
- brand (lookup)
- dimensions: length / width / height in cm; weight in g (e.g. 70 x 50 x 2 cm, 300 g)
- prices: cost (180000), retail (350000), wholesale (300000) — integer VND
- VAT (enum) — e.g. 0% / 5% / 8% / 10%
- variant matrix: attributes x values generate SKUs (e.g. size S/M/L x color)
- images, description (rich text), warranty months, status

### Business logic
- A product may be simple or have a variant matrix; each variant combination is its own stock-keeping unit with its own barcode.
- Pricing supports cost / retail / wholesale tiers; price changes are written to an audit log.
- VAT is per product, applied at sale time.

## product/item/detail
Read-only aggregate: identity, prices, stock across the 3 depots, sales history, related variants/IMEI/batches.

## product/item/inventory
Matrix of product (rows) x depot (columns) showing on-hand quantity; supports filter by depot/category.

## product/item/storagetime
Tracks how long stock has been held (aging) per product/depot — used for slow-mover analysis.

## product/item/logprice
Audit log of price changes (observed ~1177 rows): timestamp, user, old price, new price, price type.

## product/category/index
Hierarchical categories (parent/child; ~63 observed). Fields: name, parent, order, status.

## product/variant/index
Attribute definitions used to build variant matrices. Observed attributes: Mua (season), Kich thuoc (size), Mau sac (color). Each has an ordered list of allowed values.

## product/brand/index
Brand registry: name, logo, status.

## product/imei/index
IMEI / serial registry (~620 observed): serial, linked product, status (in-stock / sold / returned), depot, import/sale references. For serialized goods.

## product/batch/index
Batch / lot tracking: batch code, product, manufacture/expiry dates, quantity, depot. For goods needing lot/expiry control.

## Entities (for schema)
- Product (id, businessId, name, code, barcode, unitId, categoryId, brandId, costPrice, retailPrice, wholesalePrice, vat, length, width, height, weight, warrantyMonths, status)
- ProductVariant (id, productId, attributesJson, code, barcode, prices)
- VariantAttribute (id, name, order) / VariantAttributeValue (id, attributeId, value, order)
- Category (id, name, parentId, order, status)
- Brand (id, name, logo, status)
- Imei (id, productId, serial, status, depotId, importRef, saleRef)
- Batch (id, productId, code, mfgDate, expDate, qty, depotId)
- ProductPriceLog (id, productId, userId, priceType, oldValue, newValue, createdAt)
- Inventory (productId, depotId, qty)
