# Inventory Module (Kho hàng) — Deep Documentation

Module path: `/inventory/*` & `/supplier/*` · Menu: "Kho hàng" · businessId=137541 · depots: PHANH / PHANH 2 / PHANH 3

Menu routes (captured from nav DOM):
- Xuất nhập kho (Stock in/out) → `/inventory/bill/index` ; create import `/inventory/bill/import`, export `/inventory/bill/export`
- Chuyển kho (Transfer) → `/inventory/transfer/index` ; create `/inventory/transfer/add`
- Kiểm kho (Stocktake) → `/inventory/check/index`
- Phiếu nháp (Draft requirement slips) → `/inventory/requirement/bill`
- Hạn mức tồn kho (Stock min/max limits) → `/inventory/archive/index`
- Vị trí sản phẩm (Product locations/bins) → `/inventory/position/index` ; categories `/inventory/position/category` ; slips `/inventory/position/bill` ; imex `/inventory/position/imex`
- Dự báo nhập hàng (Purchase forecast / moving avg) → `/inventory/forecasting/movingaverage`
- Tổng công ty (Company-wide availability) → `/inventory/product/companyavailable`
- Tồn kho (Stock on hand) → `/product/item/inventory`
- Nhà cung cấp (Suppliers) → `/supplier/manage/index`
- Nhập từ Excel → `/store/import/index`

---

## 1. Stock In/Out Slips — `/inventory/bill/index`

Tabs: Phiếu xuất nhập kho (slips) / Sản phẩm xuất nhập kho (product-level lines).
Filters: Kho hàng, ID, **Loại** (Type), **Kiểu** (Mode), Ngày (date range).

### Loại (Type) enum
- Nhập (Import / stock-in)
- Xuất (Export / stock-out)

### Kiểu (Mode / movement reason) enum — full set captured
`[G]` Giao hàng (Delivery) · `[L]` Bán lẻ (Retail) · `[C]` Chuyển kho (Transfer) · `[TL]` Tặng kèm (Bán lẻ) · `[N]` Nhà cung cấp (Supplier/purchase) · `[B]` Bán sỉ (Wholesale) · `[K]` Bù trừ kiểm kho (Stocktake adjustment) · `[#]` Khác (Other) · `[BH]` Bảo hành (Warranty) · `[SC]` Sửa chữa (Repair) · `[LKBH]` Linh kiện bảo hành (Warranty parts) · `[TB]` Tặng kèm (Bán sỉ) · `[TG]` Tặng kèm (Giao hàng) · `[CB]` Combo

### Columns
ID | Ngày (+ "Log sửa" edit-history link), Kho hàng (+ slip subtype e.g. "Nhập khác"), SP (product count), SL (qty), Tổng tiền thanh toán (total value), Người tạo, attachment & note icons. Actions: Thêm mới, Thao tác.

---

## 2. Warehouse Transfer — `/inventory/transfer/index`

Tabs: Tất cả / Phiếu nháp (Draft) / Đang chuyển đi (In transit out) / Sắp chuyển đến (Arriving).
Filters: Từ kho (From), Đến kho (To), ID, Ngày tạo, Loại, Sản phẩm.

> FILTER MANDATE APPLIED: default ~1-month range empty → widened (`?fromDate&toDate`) → **4 transfer slips** surfaced (e.g. PHANH 2 → PHANH, PHANH → PHANH 2; Kiểu = "Xuất chuyển kho").

### Columns
ID | Ngày, Kho hàng (From → To), Kiểu (Xuất chuyển kho), SP, SL, Tổng tiền, Người tạo. Footer totals (4 slips / 17 units / 5.604.000).

---

## 3. Stocktake — `/inventory/check/index`

Tabs: Kiểm kho / Sản phẩm kiểm kho. Loads all by default (19 records).
Filters: Kho hàng, Ngày tạo, ID, **Loại kiểm kho** (stocktake type), Phiếu bù trừ (adjustment slip), Ghi chú, Ngày bù trừ.

### Loại kiểm kho enum
- Theo sản phẩm (By product — partial count)
- Toàn bộ (Full / complete count)

### Columns
ID | Ngày, Loại kiểm kho, Kho hàng, Người tạo, SP, SL, **SP thiếu** (missing/discrepant products, zoom icon), **Bù trừ kiểm kho** (adjustment: shows depot + datetime when reconciled, or a "→" link to create one). SL column carries the variance (can be negative, e.g. -10, -3 = shrinkage).

Merge rule (footnote): stocktake slips can be merged only if same store AND not yet adjusted.

---

## 4. Suppliers — `/supplier/manage/index`

Tabs: Nhà cung cấp / Sản phẩm nhà cung cấp (supplier-product mapping).
Filters: ID, Nhà cung cấp, Ngày, Số điện thoại, Người tạo, Loại, Trạng thái. (3 suppliers present.)

### Columns
Mã (code), Tên, **Loại** (Cá nhân / Individual — vs Doanh nghiệp / Company), Điện thoại, Người tạo, Ghi chú, active status. Actions: Thêm mới, Xuất dữ liệu.

---

## Other inventory pages (routes confirmed, structure consistent)
- **Hạn mức tồn kho** `/inventory/archive/index` — min/max stock thresholds per product+depot (reorder alerts)
- **Vị trí sản phẩm** `/inventory/position/index` — bin/shelf locations; `category`, `bill`, `imex` sub-pages for location movements
- **Dự báo nhập hàng** `/inventory/forecasting/movingaverage` — purchase forecast via moving average
- **Tổng công ty** `/inventory/product/companyavailable` — cross-depot availability roll-up
- **Phiếu nháp** `/inventory/requirement/bill` — draft/requirement slips

---

## API / route summary (Inventory)
- `GET /inventory/bill/index` (filters: kho, loai, kieu, date) — stock slips
- `GET /inventory/bill/import`, `/inventory/bill/export` — create slips
- `GET /inventory/transfer/index?fromDate&toDate` — transfers
- `GET /inventory/check/index` — stocktake
- `GET /supplier/manage/index` — suppliers
- `GET /product/item/inventory` — stock on hand (see products-deep)

## Entities surfaced
- **InventoryBill (slip)**: id, depotId, type(Nhap/Xuat), mode([G],[L],[C],[N],[K],[BH],...), productCount, qty, totalValue, creator, createdAt, editLog
- **Transfer**: id, fromDepot, toDepot, mode(Xuat chuyen kho), productCount, qty, totalValue, status(draft/inTransit/arriving), creator, createdAt
- **Stocktake**: id, type(byProduct/full), depot, creator, productCount, qty, missingProducts, variance(signed), adjustmentSlip{depot,datetime}, createdAt
- **Supplier**: code, name, type(individual/company), phone, creator, note, active
- **StockLimit**: productId, depotId, min, max
- **ProductLocation**: productId, depotId, bin/shelf, locationCategory
