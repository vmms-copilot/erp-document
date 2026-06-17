# Reports Module (Báo cáo) — Deep Documentation

Module path: `/report/*` · Menu: "Báo cáo" · businessId=137541
A multi-dimensional BI/analytics suite spanning every business domain. Reports are read-only, filterable by date range / store / brand / category, and support **Xuất dữ liệu** (export) and **In báo cáo** (print).

Top-level report categories: Doanh thu (Revenue) · Đơn hàng (Orders) · Bán lẻ (Retail) · Bán sỉ (Wholesale) · Kho hàng (Inventory) · Sản phẩm (Products) · Bảo hành (Warranty) · Kế toán (Accounting) · Sổ kế toán (Accounting books) · Khách hàng (Customers) · Khuyến mại (Promotions) · Zalo · SMS.

---

## 1. Revenue reports — `/report/revenue/*`

A revenue cube sliced by many dimensions (routes captured from nav DOM):
- Theo thời gian (By time) → `/report/revenue/index`
- Theo cửa hàng (By store) → `/report/revenue/depot`
- Theo thương hiệu (By brand) → `/report/revenue/brand`
- Theo nhân viên (By staff) → `/report/revenue/staff`
- Theo phòng ban (By department) → `/report/revenue/department`
- Theo danh mục sản phẩm (By product category) → `/report/revenue/category`
- Theo danh mục nội bộ (By internal category) → `/report/revenue/internalcategory`
- Theo sản phẩm (By product) → `/report/revenue/product`
- Theo nhà cung cấp (By supplier) → `/report/revenue/supplier`
- Theo khách hàng (By customer) → `/report/revenue/customer`
- Tỷ suất doanh thu / tồn kho (Revenue/inventory ratio) → `/report/revenue/inventoryrate`

### Revenue-by-time columns & formulas (verbatim intent)
Thời gian, Đơn thành công (successful orders), Bán lẻ, Bán sỉ, Phí trả hàng, Chiết khấu, Tiêu điểm (points used), **Doanh thu** (revenue [13]=10-11-12), **Giá vốn** (COGS), **Lợi nhuận** (profit).
- Lợi nhuận = (Revenue after discount) − (COGS of stock-out items) + (return fee) − (points used)
- Doanh thu dự kiến (projected) = ordered revenue − (wholesale+retail+order discounts) − loyalty credit
- Đơn đặt (ordered bucket) = orders in statuses: Mới, Chờ xác nhận, Đang xác nhận, Đã xác nhận, Đổi kho hàng, Đang đóng gói, Đã đóng gói, Chờ thu gom, Đang chuyển, Thành công (minus [T] customer returns).
Filters: Hiển thị (Theo ngày/By day, etc.), Khoảng ngày, Kho hàng, Thương hiệu, Danh mục. Tùy chỉnh hiển thị (customize columns).

---

## 2. Product reports — `/report/product/*` (submenu)
Bán chạy nhất (Best sellers) · Bán chạy theo cửa hàng (Best sellers by store) · Tốc độ bán hàng (Sales velocity) · Theo kênh bán (By channel) · Theo danh mục và cửa hàng (By category & store) · Theo khoảng giá (By price range) · Theo ngày (By day) · Bán hàng theo IMEI (Sales by IMEI/serial) · Theo thuộc tính (By attribute).

---

## 3. Customer reports — `/report/customer/*`
Tổng quan (`/report/customer/index`) · Cấp độ khách hàng (`/level`) · Nhóm khách hàng (`/group`) · Theo sản phẩm (`/product`) · Tỉ lệ khách hàng (`/total`) · Sinh nhật khách hàng (`/birthday`) · Báo cáo chu kỳ mua hàng (`/frequencybought`). Plus revenue-by-customer (`/report/revenue/customer`).

## 4. Promotion reports — `/report/promotion/*`
Tỉ lệ với doanh thu (`/discountrate`) · Khuyến mại theo sản phẩm (`/productdiscountrate`) · Quà tặng theo sản phẩm (`/productgift`) · Quà tặng theo cửa hàng (`/bonusbydepot`) · Doanh thu theo bảng giá (`/pricelist`).

## 5. Other categories (submenus)
- **Đơn hàng** (Orders) — order volume/status analytics
- **Bán lẻ / Bán sỉ** (Retail/Wholesale) — POS channel reports (incl. `/report/retail/switch`, `/report/wholesale/switch`)
- **Kho hàng** (Inventory) — stock value, ageing, movement
- **Bảo hành** (Warranty) — ticket/repair analytics
- **Kế toán / Sổ kế toán** (Accounting / books) — P&L, cash flow, ledgers, financial statements
- **Zalo / SMS** — notification campaign reports

---

## API / route summary (Reports)
- `GET /report/revenue/{index,depot,brand,staff,department,category,internalcategory,product,supplier,customer,inventoryrate}`
- `GET /report/product/{bestseller,velocity,bychannel,byimei,byattribute,...}`
- `GET /report/customer/{index,level,group,product,total,birthday,frequencybought}`
- `GET /report/promotion/{discountrate,productdiscountrate,productgift,bonusbydepot,pricelist}`
- `GET /report/{order,inventory,warranty,accounting}/*`
All accept date-range + dimension filters; export & print supported.

## Notes for rebuild
- Reports are derived/aggregate views over the transactional entities documented in the other module docs (orders, pos bills, inventory slips, accounting transactions, customers, promotions). Implement as a reporting layer (materialized views / OLAP) keyed by date, depot, product, brand, category, staff, customer, channel.
- Profit/revenue formulas above are authoritative for the revenue cube.
