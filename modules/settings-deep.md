# Settings Module (Cài đặt) — Deep Documentation

Module path: `/setting/*` & `/store/*` · Menu: "Cài đặt" · businessId=137541
Configuration hub: staff/RBAC, store-wide behaviour toggles, notifications, payment, print templates, branches, agents, subscription.

Menu routes (captured from nav DOM):
- Nhân viên (Staff & permissions) → `/store/user/index`
- Cài đặt chung (General settings) → `/setting/store/index`
- Bán hàng và XNK (Sales & stock in/out) → `/setting/store/sale`
- Hóa đơn điện tử (E-invoice) → `/setting/invoice/index`
- Đơn hàng và vận chuyển (Orders & shipping) → `/setting/order/index`
- Cài đặt nhãn (Label settings) → `/setting/store/labels`
- Mẫu in (Print templates) → `/store/template/design`
- Gửi Zalo → `/setting/zalo/index` · Gửi email → `/setting/email/index` · Gửi SMS → `/setting/sms/index`
- Thanh toán (Payment methods/gateways) → `/setting/payment/index`
- Chi nhánh (Branches/depots) → `/store/branch/index`
- Đại lý (Agents) → `/store/agency/index`
- Hạn sử dụng (Subscription/expiry) → `/setting/expire/depot`
- Business Tools → `/business/tool/index`

---

## 1. Staff & RBAC — `/store/user/index`

12 staff users. Tabs: **Nhân viên** (Staff) · **Phòng ban** (Departments) · **Phân quyền** (Permissions) · **Phân quyền nhóm** (Group permissions) · **Phân quyền xem dữ liệu** (Data-visibility permissions) · **Log profile** (audit).
Filters: Kho hàng, ID, Thông tin nhân viên, Quyền (role), Khóa (lock), Đặc điểm, Phòng ban.

### Columns
Nhân viên, Phòng ban, Tên đăng nhập (username), **Nhóm quyền** (role), **Kho hàng** (depot access scope, multi), Trạng thái (Khóa = locked), Đăng nhập 2 yếu tố (2FA).

### Roles observed (Nhóm quyền enum)
- Giám đốc (Director / full admin)
- Cửa hàng trưởng (Store manager)
- Nhân viên kho (Warehouse staff)
- Nhân viên bán hàng (Sales staff)
- Nhân viên thu ngân (Cashier)
Access is depot-scoped (a user may be limited to PHANH / PHANH 2 / PHANH 3). Custom permission groups configurable under Phân quyền / Phân quyền nhóm. Data-visibility (e.g. see only own orders) under Phân quyền xem dữ liệu.

---

## 2. General Settings — `/setting/store/index`

Grouped behaviour toggles. **Sản phẩm (Products)** group:
- Tự động sinh mã vạch khi thêm sản phẩm mới, đầu mã vạch = 20 (barcode prefix, dropdown 20–29)
- Đầu số mã vạch cho cân điện tử = 01 (scale-barcode prefix 01–29)
- Hiển thị số tồn có thể bán khi gợi ý sản phẩm (show sellable stock in product suggest)
- Hiển thị ảnh sản phẩm từ các sàn (Lazada/Shopee/TikTok/Sendo/Tiki) nếu chưa có ảnh
- Khi bán sản phẩm lô lấy theo ngày hết hạn gần nhất (FEFO batch picking)
- Áp dụng với tên file ảnh upload (Tự động lấy theo tên sản phẩm / Giữ nguyên / Ngẫu nhiên)
- Áp dụng tìm kiếm: tuần tự (sequential) vs gần đúng (fuzzy)

**Kế toán (Accounting)** group: Sử dụng hạch toán kế toán tự động (auto-posting on) — ties POS/orders to the GL automatically (see accounting-deep).

(Other groups continue down the page: orders, customers, printing, etc. Save via Cài đặt button.)

---

## 3. Other settings pages (routes confirmed)
- **Bán hàng và XNK** `/setting/store/sale` — POS & stock-movement behaviour
- **Hóa đơn điện tử** `/setting/invoice/index` — e-invoice provider/connection
- **Đơn hàng và vận chuyển** `/setting/order/index` — order defaults & carrier integrations
- **Cài đặt nhãn** `/setting/store/labels` · **Mẫu in** `/store/template/design` (print-template designer)
- **Gửi Zalo/email/SMS** `/setting/{zalo,email,sms}/index` — notification channel config & templates
- **Thanh toán** `/setting/payment/index` — payment methods/gateways (MoMo, bank, card)
- **Chi nhánh** `/store/branch/index` — depots/branches (PHANH/PHANH 2/PHANH 3)
- **Đại lý** `/store/agency/index` — agent/dealer accounts
- **Hạn sử dụng** `/setting/expire/depot` — subscription/expiry per depot & marketplace connection
- **Business Tools** `/business/tool/index`

---

## API / route summary (Settings)
- `GET /store/user/index` — staff & RBAC (roles, depot scope, 2FA, data visibility)
- `GET /setting/store/index|sale|labels` — store config
- `GET /setting/{invoice,order,zalo,email,sms,payment}/index`
- `GET /store/{branch,agency}/index` · `GET /setting/expire/depot`

## Entities surfaced
- **StaffUser**: id, name, username, role(Director/StoreManager/WarehouseStaff/SalesStaff/Cashier), departmentId, depotScope[], locked(bool), twoFactorEnabled, email/phone
- **Department**: id, name (e.g. Phòng Kinh doanh)
- **PermissionGroup**: id, name, permissions[], dataVisibilityRules[]
- **StoreSetting**: key, value (barcodePrefix, scaleBarcodePrefix, showSellableStock, marketplaceImageFallback, fefoPicking, imageNamingMode, searchMode, autoAccountingPosting, ...)
- **Branch/Depot**: id, name, address, subscriptionExpiry
- **Agency**: id, name, terms
- **NotificationConfig**: channel(zalo/email/sms), templates[], credentials
- **PaymentMethodConfig**: method(cash/bank/card/momo), gatewayCreds
