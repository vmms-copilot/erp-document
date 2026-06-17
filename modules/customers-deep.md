# Customers Module (Khách hàng / CRM) — Deep Documentation

Module path: `/customer/*` · Menu: "Khách hàng" · businessId=137541

Menu routes (captured from nav DOM):
- Khách hàng (Customer list) → `/customer/code/customerlist`
- Thẻ khách hàng (Customer cards) → `/customer/code/index`
- Chăm sóc khách hàng (Customer care/CRM log) → `/customer/care/index`
- Cấp độ (Levels) → `/customer/store/level`
- Nhóm khách hàng (Groups) → `/customer/group/index`
- Hình thức chăm sóc (Care methods) → `/customer/type/index`
- Lý do chăm sóc (Care reasons) → `/customer/reason/index`
- Công nợ (Customer debt) → `/accounting/debts/customer`

---

## 1. Customer List — `/customer/code/customerlist`

271 customers present. Segmentation tabs (RFM-style):
- Tất cả (All)
- Mua nhiều (High spenders)
- Mua nhiều, sinh nhật trong tháng (High spenders w/ birthday this month)
- Mua thường xuyên (Frequent buyers)
- Lâu chưa mua (Lapsed / haven't bought recently)

### Filters
ID, Mã thẻ (card code), Khách hàng (name/phone), **Loại** (Type), **Cấp độ** (Level), **Nhóm** (Group), Lọc.

### Loại (Customer Type) enum
- Khách lẻ (Retail customer)
- Khách sỉ (Wholesale customer)
- Đại lý (Agent / dealer)

### Columns
Khách hàng, Loại, Số điện thoại, Cấp độ (e.g. "Cấp độ 0"), Nhóm, **Tổng tiền** (lifetime spend), **Điểm** (loyalty points), **Lần mua** (purchase count), SL (qty), **Chu kỳ mua hàng** (purchase cycle days). Sortable columns marked with ⇅. Actions: Thêm mới, Thao tác.

### Business-logic footnotes (verbatim intent)
- Lần mua vs Số ngày mua: 2 bills same day => Lần mua=2, ngày mua=1.
- Chu kỳ mua hàng = (last purchase date - first purchase date) / (total non-return orders - 1).
- "Thời gian chưa mua" = days since last purchase.
- Excel export capped at 50,000 rows.

---

## 2. Customer Care / CRM Log — `/customer/care/index`

Breadcrumb: Lịch sử chăm sóc khách hàng. 105 care records.
Filters: ID, Ngày, **Hành động** (Action), **Hình thức** (Method), **Lý do** (Reason), Số điện thoại.

### Hành động (Care Action) enum
- Tặng điểm (Grant points)
- Trừ điểm (Deduct points)
- Tặng tiền tích lũy (Grant loyalty cash/credit)
- Trừ tiền tích lũy (Deduct loyalty cash/credit)
- Gọi điện (Phone call)
- Nhắn tin (Send message / SMS)
- (more below)

### Columns
Khách hàng (name+phone), Hành động, Chi tiết (detail + amount, e.g. "Tặng tiền tích lũy: 275.000"), Mô tả (e.g. "Import tiền khi import khách hàng"), Người tạo, row actions. Buttons: Thêm mới, Xuất dữ liệu, Xóa các dòng đã chọn.

The Hình thức (care method/channel) and Lý do (care reason) filter values are user-defined lookups managed at `/customer/type/index` and `/customer/reason/index`.

---

## 3. Customer Groups — `/customer/group/index`

3 groups: VIP1 (id 1009), VIP2 (id 1010), VIP3 (id 1011). Columns: ID, Tên nhóm, Người tạo, Ngày tạo, Hành động. Filter: Nhóm. Action: Thêm mới.

---

## 4. Supporting config pages (routes confirmed)
- **Cấp độ** `/customer/store/level` — customer tier definitions (Cấp độ 0..n; drives auto-tiering by spend)
- **Thẻ khách hàng** `/customer/code/index` — membership/loyalty card codes
- **Hình thức chăm sóc** `/customer/type/index` — care method lookup (e.g. call/SMS/email/Zalo)
- **Lý do chăm sóc** `/customer/reason/index` — care reason lookup
- **Công nợ khách hàng** `/accounting/debts/customer` — receivables (covered in accounting-deep)

---

## API / route summary (Customers)
- `GET /customer/code/customerlist` (filters: type, level, group; tabs RFM) — customer list
- `GET /customer/care/index` (filters: action, method, reason) — CRM care log
- `GET /customer/group/index` — groups
- `GET /customer/store/level` — levels
- `GET /customer/code/index` — cards

## Entities surfaced
- **Customer**: id, name, type(retail/wholesale/agent), phone, cardCode, level(Cấp độ N), groupId(VIP1/2/3), lifetimeSpend, loyaltyPoints, purchaseCount, qty, purchaseCycleDays, firstPurchase, lastPurchase, birthday, gender, address, email, facebook, company, taxCode, note
- **CareRecord (CRM)**: id, customerId, action(grantPoints/deductPoints/grantCredit/deductCredit/call/sms/...), detail, amount?, method(Hình thức), reason(Lý do), description, creator, createdAt
- **CustomerGroup**: id, name(VIP1/VIP2/VIP3), creator, createdAt
- **CustomerLevel**: id, name(Cấp độ N), thresholds
- **CareMethod / CareReason**: id, name (lookups)
