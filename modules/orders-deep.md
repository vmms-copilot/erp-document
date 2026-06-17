# Orders Module — Deep Documentation

Module path: `/order/*` · Business: nhanh.vn multi-channel retail/ERP · businessId=137541
Architecture: Angular SPA. GET = shell, POST = JSON data. Envelope: `{code, errorCode, messages, data}`.

This is a page-by-page deep dive of the Orders (Đơn hàng) module. All exploration was read-only; no orders were created, modified, or deleted.

---

## 1. Order List — `/order/manage/index`

The central order management grid. Loads via `POST /order/manage/index` plus supporting calls:
- `POST /order/manage/loadorderdata?act=loadCarrierAccountIds` — carrier account options
- `POST /order/manage/loadorderdata?act=loadTrafficSourceIds` — traffic/order-source options
- `POST /order/manage/loadorderdata?act=loadNumberOrder` — per-tab order counts (badges)

### Workflow tabs (order lifecycle stages)
These tabs segment orders by processing stage; numeric badges show counts:
- **Tất cả** (All)
- **Xác nhận thanh toán** (Confirm payment)
- **Xác nhận** (Confirm) — badge: 3
- **In & đóng gói** (Print & pack) — badge: 1
- **Đang chuyển** (In transit)
- **Cần xử lý** (Needs handling)
- **Thanh toán** (Payment)
- **Khiếu nại** (Complaints)
- **GH1P** (Giao hàng 1 phần / partial delivery)
- **Xuất kho** (Stock-out) — badge: 7

Note: standalone routes like `/order/packing/index` and `/order/complain/index` return "Bạn không có quyền truy cập" (no access) — these stages are implemented as TABS within the list, not separate pages.

### Filters
- **Ngày tạo** (Created date) — date range (default last ~60 days)
- **Cửa hàng** (Store) — PHANH / PHANH 2 / PHANH 3 (multi-depot)
- **ID** — order ID text
- **Trạng thái** (Status) — multi-select; full enum below
- **Khách hàng** (Customer) — name/phone search
- **Sản phẩm** (Product) — product search
- **Hãng vận chuyển** (Carrier) — carrier dropdown (SPX, GHN, ...)
- **Mã vận đơn** (Tracking code)
- **Lọc** (Filter) submit button

### Order Status enum (Trạng thái) — full set captured
1. Mới (New)
2. Chờ xác nhận (Pending confirmation)
3. Đang xác nhận (Confirming)
4. Đã xác nhận (Confirmed)
5. Hết hàng (Out of stock)
6. Đổi kho hàng (Warehouse transfer)
7. Đang đóng gói (Packing)
8. Thất bại (Failed)
9. Khách hủy (Customer cancelled)
10. Hệ thống hủy (System cancelled)
11. HVC hủy (Carrier cancelled)
12. Đang hoàn (Returning)
13. Xác nhận hoàn (Confirm return)
14. Đã hoàn (Returned)
(Plus operational states seen on rows: Thành công / Success.)

### Grid columns
ID (+ created datetime + depot), Khách hàng (phone, name, address), Sản phẩm (variant lines), Giá (price, with discount line), SL (qty), carrier icon column (SPX/GHN), COD/payment column (bank-transfer / COD amounts), tracking/status column, and a status pill (Mới / Thành công / Chờ xác nhận / Đang xác nhận / Đã hoàn / Đang đóng gói).

### Bulk action menu (Thao tác)
- Tạo phiếu xuất kho (Create stock-out slip)
- Tạo phiếu nhập kho (Create stock-in slip)
- Đổi trạng thái (Change status)
- Đổi kho hàng (Change warehouse)
- Gộp các đơn đã chọn (Merge selected orders)
- Tách đơn Mega Live (Split Mega Live order)
- Xuất dữ liệu (Export data)
- Gửi đơn sang hãng vận chuyển (Send orders to carrier)
- Thêm đơn vào biên bản bàn giao (Add to handover record)
- Xóa (Delete)
- Thêm đối soát đơn tự vận chuyển (Add self-shipping reconciliation)

Other toolbar: Thêm mới (New, + dropdown), edit (pencil) menu, In đơn (Print order).

---

## 2. Create Order — `/order/manage/add`

Full single-page order entry form, four panels:

### Khách hàng (Customer)
- Tên (Name), Điện thoại (Phone, with save icon), Địa chỉ (Address), Tỉnh/Thành phố, Quận/Huyện, Phường/Xã (province/district/ward cascade)
- Ghi chú khách hàng (Để in) — customer note (printable)
- Ghi chú CSKH (Nội bộ) — internal care note

### Đơn hàng (Order meta)
- Nguồn đơn hàng (Order source) — dropdown + add
- Nhãn (Label/tag) — dropdown + add
- Nhân viên bán hàng (Sales staff)
- **Delivery method** (Giao hàng) dropdown enum: Giao hàng tận nhà (Home delivery), Mua tại quầy (Buy at counter), Đặt trước (Pre-order), Đổi quà (Gift exchange), Đổi sản phẩm (Product exchange), Khách trả lại hàng (Customer return)

### Sản phẩm (Products)
- Chọn kho hàng (Select warehouse), (F3) Tìm sản phẩm (product search), Xem tồn (View stock)
- Giá bán (Price tier) selector — default "Bán lẻ" (Retail)
- Line table: Sản phẩm / SL / Giá / Tổng

### Thanh toán (Payment)
- Chiết khấu (Discount) — toggle % or $ (đ)
- Tiền đặt cọc (Deposit), Tiền chuyển khoản (Bank transfer), Tiền quẽt thẻ (Card swipe)

### Vận chuyển (Shipping)
- Mã khuyến mại (Promo code, + add), Phí ship báo khách (Ship fee shown to customer), Lấy theo phí HVC (use carrier fee)
- Toggles: Giao hàng một phần (Partial delivery), Hàng dễ vỡ (Fragile), Khai giá (Declare value), Gộp kiện (Combine parcels)
- KL (gram) / Chiều dài / Chiều rộng / Chiều cao (weight & dimensions)
- Tự vận chuyển (Self-ship) toggle, Ngày giao hàng (Delivery date)
- Order status selector at footer enum: Mới, Đã xác nhận, Đang xác nhận, Chờ xác nhận, Hết hàng, Hệ thống hủy, Thất bại

### Footer
- Post-save mode radios: Tiếp tục thêm (continue adding) / Xem danh sách (view list) / Xem chi tiết (view detail)
- Thu khách / Trả shop totals
- (F9) Lưu (Save) · (F10) Lưu và in (Save & print)

---

## 3. Order Detail (modal) — opened from list row

A modal with 4 tabs: Thông tin / In & đóng gói / Gửi vận chuyển / API. Header shows order ID + status pill; In đơn and Thao tác menus.

### Thông tin (Info) tab
- **Khách hàng**: name, phone (with telco label e.g. Viettel), birthday, address; mini status-distribution bar
- **Sản phẩm**: line items (#, name/variant, SL, Giá bán, Tổng)
- **Carrier block** (e.g. SPX Express): Dịch vụ (service = Tiêu chuẩn/Standard), Cân nặng (weight gr), Khai giá hàng hóa (declared value), Phí vận chuyển (ship fee), view/try-on flag, pickup time
- **File đính kèm** (attachments) with Upload
- **Nhãn** (Labels) with + add
- **Thanh toán**: Phí ship báo khách, Thu người nhận (collect from receiver), Thu người gửi (collect from sender)
- **Thông tin**: depot (PHANH), sender address+phone, Loại đơn (order type), Xuất kho status (Chưa xuất kho / not yet), NVBH (sales staff), Hóa đơn điện tử (e-invoice number + status pill e.g. Nháp/Draft), Ngày tạo
- **Lịch trình** (Timeline): chronological status-change log per depot (e.g. "Thành công → Đã hoàn"), with Sao chép liên kết (copy link)

### API tab
Webhook/integration log of outbound events to connected channels. Columns: Nội dung (content, viewable), AppName (integration app id, e.g. 77241, 1008), Loại (type, e.g. "Send order status"), Ngày (timestamp). Confirms order-status is pushed to connected marketplaces via webhooks on every status change.

---

## 4. COD Reconciliation — `/order/payment/index`

Breadcrumb: Đơn hàng / Đối soát / Thanh toán COD. Cross-checks COD remittances from carriers/channels.

### Channel tabs
- POS: Đơn tự kết nối (self-connected orders)
- POS: Đơn gửi qua Nhanh (orders sent via Nhanh)
- Sàn TMĐT (e-commerce marketplaces)

### Sub-tabs
- Đã thanh toán (Paid)
- Đơn hàng đã thanh toán (Orders paid)
- Nợ cước (Fee debt)

### Filters
Ngày tạo (Created date), Loại thanh toán (Payment type: Thanh toán tiền / Thu cước), ID thanh toán, ID đơn hàng, Lọc.

### Columns
ID, Ngày thanh toán, Tài khoản nhận tiền, Ngân hàng, Tổng đơn, Số tiền, Hoàn cước, Số tiền thanh toán, Xác nhận nhận tiền. Actions: Xuất dữ liệu (Export), Xác nhận nhận tiền toàn bộ (Confirm all receipts).

Note: empty for this test account (no real carrier remittances exist for test orders) — structure documented; filters confirmed.

---

## API summary (Orders)
- `GET  /order/manage/index` — page shell
- `POST /order/manage/index` — order list data
- `POST /order/manage/loadorderdata?act=loadCarrierAccountIds`
- `POST /order/manage/loadorderdata?act=loadTrafficSourceIds`
- `POST /order/manage/loadorderdata?act=loadNumberOrder` — tab badge counts
- `GET  /order/manage/add` — create-order shell
- `GET  /order/payment/index` — COD reconciliation

## Entities surfaced
- **Order**: id, businessId, depotId, customer{name,phone,telco,birthday,address(province/district/ward)}, source, label, salesStaff, deliveryMethod, status, createdAt
- **OrderItem**: productId, variant, qty, price, total, discount
- **Shipping**: carrier(SPX/GHN), service, weight, dimensions(L/W/H), declaredValue, shipFee, codAmount, trackingCode, fragile, partialDelivery
- **Payment**: deposit, bankTransfer, cardSwipe, discount(%/amount), shipFeeToCustomer, collectFromReceiver, collectFromSender
- **OrderTimeline**: depot, fromStatus, toStatus, timestamp
- **OrderWebhookLog**: appName, type, content, timestamp
- **CODReconciliation**: paymentDate, receivingAccount, bank, totalOrders, amount, feeRefund, paidAmount, confirmed
