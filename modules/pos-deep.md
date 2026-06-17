# POS Module (Bán hàng / Point of Sale) — Deep Documentation

Module path: `/pos/*` · Menu: "Bán hàng" · businessId=137541 · depots: PHANH (depotId 140387), PHANH 2, PHANH 3
Note: a migration banner warns the legacy POS stops support after 28/02/2026 ("Chuyển đổi sang bán mới").

Menu routes (captured from nav DOM):
- Bán lẻ (Retail sale) → `/pos/bill/add` ; list `/pos/bill/index`
- Bán sỉ (Wholesale) → `/pos/bill/addwholesale` ; list `/pos/bill/wholesale`
- Tìm hóa đơn (Find invoice) → `/pos/bill/lookup`
- Hóa đơn điện tử (E-invoice) → `/pos/invoice/index`
- Trả hàng (Returns) → `/pos/return/index`
- Nợ quà tặng (Gift debt) → `/pos/debt/bill`

---

## 1. Retail Sell Screen — `/pos/bill/add`

On open it forces **Chọn kho hàng** (Select warehouse) modal — options: PHANH / PHANH 2 / PHANH 3. After selecting, URL gains `&depotId=`.

### Top bar
Product search `(F3)`, barcode-scan toggle, multi-bill tabs (Hóa đơn 1, + add new concurrent bill), label/promo tag, warehouse selector, fullscreen.

### Line grid columns
`#`, image, Sản phẩm, Giá bán (price), Số lượng (qty), Số tồn (stock-on-hand), Thành tiền (line subtotal), `%`, Chiết khấu (discount), Tổng (total).

### Customer panel (tab Khách hàng)
Số điện thoại `(F4)`, Mã thẻ (card/loyalty code), Giới tính (Gender: Nam / Nữ / Khác), Ngày sinh (DOB), Tên khách hàng, Nhãn (label), Email, Facebook, Nhóm (Group: VIP1 / VIP2 / VIP3), address cascade (Tỉnh/Quận/Phường + Địa chỉ), Tên công ty + Mã số thuế (company + tax code), Địa chỉ công ty, Cấp độ khách hàng (customer tier), Ghi chú, Lưu khách hàng.
Other tabs: Thao tác nhanh (quick actions). Staff fields: Nhân viên kỹ thuật (technician), Nhân viên bán hàng (salesperson).

### Payment panel (right)
Tổng tiền hàng (order total), Chiết khấu `(F6)` toggle %/$ (đ), VAT, Coupon (with refresh), Khách cần trả (amount due), Khách thanh toán `(F8)`, payment-method radios: **Tiền mặt** (Cash) / **Chuyển khoản** (Bank transfer) / **Quẽt thẻ** (Card) / **Khác** (Other), Tiền thừa (change), Ghi chú, **Lưu (F9)** (Save), and a print-dropdown.

Enums: gender = Nam/Nữ/Khác ; customer group = VIP1/VIP2/VIP3 ; payment = cash/transfer/card/other.

---

## 2. Retail Bill List — `/pos/bill/index`

Tabs: Tất cả (All) / Xác nhận thanh toán (Confirm payment).
Filters: ID Hóa đơn, Cửa hàng (store), Thời gian (date range, default narrow ~10 days), Khách hàng, Sản phẩm, Lọc.

> FILTER MANDATE APPLIED: default narrow date range showed "Không tìm thấy dữ liệu". The date picker offers presets (Hôm nay / Hôm qua / Tuần này / Tuần trước / Tháng này / Tháng trước / 3 tháng / 6 tháng / 12 tháng). Selecting **12 tháng** (or URL `?fromDate=YYYY-MM-DD&toDate=YYYY-MM-DD`) surfaced **31 real bills**.

### Columns
Người tạo (creator + datetime + depot), ID, Khách hàng (name+phone), Sản phẩm (variant lines; may show IMEI, bundled items), Giá bán, SL, Tổng tiền, Thanh toán (payment breakdown with method icons: cash / bank 🏛 / card).

### Real-data signals observed
- Payment method icons per bill (cash / transfer / card)
- **Debt** rows: "Còn nợ: X" (still owes) — partial-payment bills
- **Loyalty**: "Điểm tích lũy: 10" (accrued points)
- **Return linkage**: "Đơn trả 14542786 300.000. Tổng tiền cần thanh toán 100.000"
- IMEI/serial captured on serialized items
- Footer totals: 52 units, 15.141.200 revenue, outstanding debt 5.570.000

---

## 3. Returns — `/pos/return/index`

Filters: ID Hóa đơn, Kho hàng, Sản phẩm, Thời gian, Khách hàng, Lọc. Buttons: Tìm hóa đơn (find bill to return against), Trả hàng (create return), Thao tác.

> FILTER MANDATE APPLIED: default narrow range empty → widened to 12 months → **8 returns** surfaced.

### Columns
Người tạo, ID (+ depot + type tag **[L]** = retail / **[G]** = wholesale), Khách hàng, Sản phẩm (+ SKU code), Giá bán, SL, VAT, Trả lại (refund amount), Phí trả hàng (return fee), Tổng tiền, note linking origin: "Trả hàng từ hóa đơn 14542784". Returns can also reference original marketplace orders ("Trả từ đơn: 592755446"). Refunds may post as customer debt ("Nợ: X").

---

## 4. E-Invoice — `/pos/invoice/index`

Filters: ID Hóa đơn, Thời gian, Trạng thái. Actions: Xuất dữ liệu (Export), Cập nhật trạng thái hóa đơn điện tử (Sync e-invoice status).

### Status enum (Trạng thái)
1. Phát hành (Issued)
2. Nháp (Draft)
3. Hủy phát hành (Issuance cancelled)
4. Bị điều chỉnh (Adjusted)

### Columns
ID, Cửa hàng, Khách hàng, Sản phẩm, Giá bán, SL, VAT, Tổng tiền, Hóa đơn điện tử (e-invoice no.), Mã cơ quan thuế (tax authority code), Người tạo.

---

## 5. Gift Debt — `/pos/debt/bill`

Tracks promotional gift items owed to customers but not yet handed over. Filters: ID Hóa đơn, Kho hàng, Sản phẩm, Thời gian, Lọc. (2 records present.)

### Columns
Hóa đơn, Kho hàng, Ngày hẹn trả (promised delivery date), Khách hàng, Sản phẩm, Giá bán, SL, Tồn kho (stock when warehouse filtered), Trạng thái.

---

## API / route summary (POS)
- `GET /pos/bill/add` (+`depotId`) — retail sell screen
- `GET /pos/bill/index?fromDate&toDate` — retail bill list (date filter drives data)
- `GET /pos/bill/addwholesale`, `/pos/bill/wholesale` — wholesale sell/list
- `GET /pos/bill/lookup` — invoice lookup
- `GET /pos/return/index?fromDate&toDate` — returns list
- `GET /pos/invoice/index` — e-invoices
- `GET /pos/debt/bill` — gift debt

## Entities surfaced
- **Bill (POS retail/wholesale)**: id, depotId, creator, type([L]/[G]), customer{name,phone,gender,dob,group(VIP),cardCode,company,taxCode}, items[], total, discount(%/$), vat, coupon, paymentMethod(cash/transfer/card/other), paid, change, debtRemaining, loyaltyPointsEarned, createdAt
- **BillItem**: productId, variant, sku, price, qty, stockOnHand, lineDiscount, lineTotal, imei?
- **Return**: id, depotId, type, customer, items[], refundAmount, returnFee, vat, total, originBillId, originOrderId, debtPosted
- **EInvoice**: id, billId, store, customer, items, vat, total, eInvoiceNo, taxAuthorityCode, status(Issued/Draft/Cancelled/Adjusted), creator
- **GiftDebt**: billId, depot, promisedDate, customer, product, price, qty, status
