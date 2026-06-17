# Products Module — Deep Dive

Page-by-page deep documentation of the Products (Sản phẩm) module, captured live with real tenant data (businessId=137541, 110 products, 3 depots: PHANH / PHANH 2 / PHANH 3). API pattern: GET returns SPA shell, POST returns JSON data envelope { code, errorCode, messages, data }.

---

## 1. product/item/index — Product List

**Tabs:** Sản phẩm (Products) | Lịch sử sửa xóa (Edit/Delete history)

**Filter bar (each filter posts to POST /product/item/index):**
- Cửa hàng (Store/Depot) — multi-select checkbox dropdown with search. Options: PHANH, PHANH 2, PHANH 3.
- ID — text.
- Tên, mã sản phẩm (Name / product code) — text.
- Danh mục (Category) — hierarchical multi-select tree (parent/child indentation). Includes "Chưa gắn danh mục" (uncategorized).
- Tồn (Stock) — single select: "- Tồn -" (all) | Còn tồn (in stock) | Còn có thể bán (available to sell).
- Lọc (Filter) button.

**Columns:** checkbox | Ảnh (image) | Mã vạch (barcode) | Mã (code/SKU) | Tên (name) | Giá vốn (cost price) | Giá bán (sale price, with VAT badge e.g. "VAT: 8%") | Tồn (stock) | 3 depot stock columns (truck/home/box icons) | sortable stock | Bán (sell toggle) | row-action menu.

**Footer:** totals row (Tổng) summing stock columns; pagination "1 - 50 / 110".

**Actions:** Thêm mới (Add new, with dropdown) | Thao tác (bulk actions).

**Real data observed:** simple products, combos ("combo áo phông", "combo 2 men vi sinh"), VAT-bearing items (8%, 10%), variant products (e.g. "Polo nam họa tiết" in M/L/XL/XXL × m01–m04; "Áo Phông Nữ Dáng Boxy" in S/M/XL × Tím/Cam/Đỏ), IMEI products (Iphone 13, Đèn bàn Luxia Base), negative stock rows (oversold, e.g. -1, -60).

**APIs:** POST /product/item/index?businessId=137541 (list+filter+sort; sort via ?sort=avgCost&dir=asc); POST /product/brand/load (brand options).

---

## 2. product/item/add — Add Product

**Tabs:** Thông tin cơ bản (Basic info) | Website | Bảo hành (Warranty)

**Basic info — left column fields:**
- Tên sản phẩm * (Name, required)
- Loại sản phẩm * (Product type, required) — ENUM: Sản phẩm | Voucher | Sản phẩm cân đo (weighed) | Sản phẩm theo IMEI | Gói sản phẩm (bundle) | Dịch vụ (service) | Dụng cụ (tool) | Sản phẩm bán theo Lô (by lot) | Combo | Sản phẩm nhiều đơn vị tính (multi-unit).
- Mã (code/SKU)
- Mã vạch (barcode)
- Trạng thái (Status) — ENUM: Mới (New) | Đang bán (Selling) | Ngừng bán (Stopped) | Hết hàng (Out of stock).
- Sản phẩm cha (Parent product)
- Đơn vị tính (Unit)
- Danh mục (Category) — hierarchical, with + to create. Drives the variant attribute set.
- Danh mục nội bộ (Internal category)
- Thương hiệu (Brand) — with + to create
- Nhãn (Label/Tag) — with + to create
- Chiều dài / rộng / cao (L/W/H) in cm; Khối lượng (weight) in Gram
- Lấy số lượng đến số thập phân thứ (decimal places for qty, 0-6)

**Basic info — right column (pricing):**
- Giá nhập (Import/cost price)
- Giá bán lẻ (Retail price) | Giá bán sỉ (Wholesale price)
- VAT — ENUM: 0% | 5% | 8% | 10% | Không chịu thuế (not taxable) | Không kê khai thuế (not declared).
- Household-business tax block (Dành cho hộ kinh doanh): Danh mục ngành nghề (industry category), Thuế cho hộ kinh doanh (% tax), Giảm VAT (VAT reduction).

**Media:** Ảnh sản phẩm — Upload ảnh sản phẩm.

**Bottom sub-tabs:**
- Tồn đầu (Initial stock)
- Thuộc tính (Attributes) — unlocked after choosing a category. For category "Áo cool ngầu dành cho boss" the attributes are: Mùa (Season), Kích thước (Size), Màu sắc (Color). Size values include: XS, S, S/M, M, M/L, L, ... (24 total).
- Tạo sản phẩm con (Create variants) — generates the child SKU matrix from selected attribute values.

**Save bar:** radio: Thêm mới | Danh sách sản phẩm | In mã vạch sản phẩm; green Lưu (Save).

**Business logic:** product can be simple or have a variant matrix (each combo = its own SKU + barcode). Category selection determines available variant attributes. VAT is per-product and shown at sale time. (No submit performed — read-only exploration.)

---

## 3. product/category/index — Categories

**Tabs:** Danh mục sản phẩm (Product categories) | Danh mục nội bộ (Internal categories)
**Filters:** Tên, Mã, Lọc.
**Columns:** # | Tên (indented to show hierarchy) | Mã | Ảnh | Icon | Trạng thái (active ✓ / inactive ⊖) | Thứ tự (sort order) | Số SP (product count).
**Real data:** hierarchical, e.g. Vải > Vải cotton > Cotton 2 chiều (4 products); đồng hồ; Áo cool ngầu dành cho boss (inactive); Hoa tai (4); Lắc tay (7); Dụng cụ (3); etc.

---

## 4. product/variant/index — Attributes

**Tabs:** Thuộc tính (Attributes) | Nhóm thuộc tính (Attribute groups)
**Filters:** Nhóm thuộc tính, Danh mục, ID, Tên thuộc tính, Mã thuộc tính.
**Columns:** ID | Tên | Giá trị (value count) | Mã thuộc tính (code) | Bắt buộc nhập (required) | Tìm kiếm (searchable) | Thứ tự | Người tạo (creator).
**Real data (3 attributes):**
- Mùa (ID 16268, code "Mùa", 3 values)
- Kích thước (ID 16267, code "size", 24 values)
- Màu sắc (ID 16266, code "color", 27 values)
All searchable; none required; creator PHANH. Inline Lưu to save sort order.

---

## 5. product/imei/index — IMEI / Serial Registry

**Tabs (7):** IMEI | Có hạn bảo hành (in warranty) | Sắp hết hạn bảo hành (warranty expiring) | Đã hết hạn bảo hành (warranty expired) | Lịch sử IMEI (history) | Lệch tồn IMEI (stock discrepancy) | Đổi sản phẩm IMEI (product swap).
**Filters:** Kho hàng (depot), ID, Tên/mã sản phẩm, Số IMEI, Trạng thái, Danh mục, Lọc.
**Columns:** Sản phẩm | IMEI | Nhà cung cấp (supplier) | Kho hàng (depot) | Giá nhập | Giá bán | Trạng thái | actions.
**Status ENUM (full):** Mới (New) | Đã bán (Sold) | Đang vận chuyển (In transit) | Lỗi (Defective) | Hàng Demo (Demo) | Đã trả nhà cung cấp (Returned to supplier) | Tồn trong cửa hàng (Mới + Lỗi) (In-store = New+Defective) | Đang chuyển kho (Transferring depots) | Đang bảo hành (Under warranty/repair) | Đã trả bảo hành (Warranty returned).
**Real data:** 620 serials. e.g. Iphone 13 (serials 119870/136470 = Mới; 11987/13647 = Đã bán) priced 23,000,000; Đèn bàn Luxia Base across PHANH/PHANH 2 with suppliers (Thu Hiền, Nhà cung cấp 2) priced 50,000/10,605,000.

---

## 6. product/item/logprice — Product Edit/Audit Log

**Tabs:** Sản phẩm | Lịch sử sửa xóa.
**Filters:** Sản phẩm, Loại log (log type), Kiểu log (log style), Cha con (parent/child), Người sửa (editor), Ngày tạo (date).
**Columns:** Mã sản phẩm | Tên sản phẩm | Loại log (e.g. "Sửa sản phẩm") | Kiểu log (e.g. "Sửa sản phẩm Combo") | Người sửa (PHANH) | Thời gian (timestamp).
**Actions:** In mã vạch (print barcode), Xuất dữ liệu (export), In thông tin sản phẩm (print info).
**Real data:** 1,177 entries with timestamps (e.g. 06/06/2026 11:08:35), tracking edits across products and variants.

---

## 7. Other product sub-pages (shared patterns)
- product/item/detail — 360 view: identity, prices, per-depot stock, related variants/IMEI/batches.
- product/item/inventory — product × depot stock matrix.
- product/item/storagetime — stock aging per product/depot (slow-mover analysis).
- product/brand/index — brand registry (name, logo, status); loaded via POST /product/brand/load.
- product/batch/index — batch/lot tracking (code, mfg/exp dates, qty, depot).

---

## Entities refined (vs 14-database-schema.md)
- product.product_type ENUM (10 values above) — add column `type` driving behavior (simple / voucher / weighed / imei / bundle / service / tool / lot / combo / multi-unit).
- product.status ENUM: new | selling | stopped | out_of_stock.
- product.vat ENUM: 0 | 5 | 8 | 10 | not_taxable | not_declared.
- variant_attribute: add columns is_required (bool), is_searchable (bool), code, value_count; observed Mùa/size/color.
- imei.status ENUM (10 values above) — richer than initial draft.
- product_log(id, product_id, log_type, log_style, user_id, created_at) — audit covers edits (not just price).
