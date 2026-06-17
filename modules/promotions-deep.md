# Promotions Module (Khuyến mại) — Deep Documentation

Module path: `/promotion/setting/*` · Menu: "Khuyến mại" · businessId=137541

Menu routes (captured from nav DOM):
- Chiết khấu / Khuyến mại (Discounts/promos) → `/promotion/setting/discount`
- Thiết lập bảng giá (Price lists) → `/promotion/setting/pricelist`
- Tích điểm (Loyalty points config) → `/promotion/setting/point`
- Mã giảm giá / Coupon → `/promotion/setting/coupon`
- Quà tặng (Gifts) → `/promotion/setting/gift`
- Hoa hồng bán hàng (Sales commission) → `/promotion/setting/commission`

Common status tabs across promo pages: Tất cả · Đang khuyến mại (Active) · Sắp khuyến mại (Upcoming) · Chưa kích hoạt (Not activated) · Ngừng khuyến mại (Stopped).
Two promotion engines coexist: **V1 (CTKM)** and **V2 (CTKM V2, Beta)** — selectable on create and filterable by Phiên bản.

---

## 1. Discounts / Promotions — `/promotion/setting/discount`

28 programs. Filters: ID, Ngày thực hiện, Tên, Trạng thái, Mô tả.

### Global discount settings (toggles on page)
- Lập hóa đơn bán lẻ: require phone number if a discount is applied
- Allow computing discount-by-promo-program when editing a bill
- Apply discount to service products (off by default)
- (Staff self-edit discount permissions configured under phân quyền nhân viên: retail / wholesale / order)

### Promotion types (subtitle on each program)
- Giảm giá theo số lượng sản phẩm (Discount by product quantity / tiered)
- Khuyến mại theo từng sản phẩm (Per-product promotion)
- Giá theo Số lượng sản phẩm (Price-by-quantity / volume pricing)

### Columns
ID, Ảnh, Chương trình (name + type, + v2 badge), Ưu tiên (priority, integer), Áp dụng (scope: Tất cả / specific), Thời hạn (start–end), Người tạo, active toggle, row actions.

---

## 2. Coupons — `/promotion/setting/coupon`

9 coupon programs. Filters: Kho hàng, ID, Ngày thực hiện, Tên, Sản phẩm.

### Coupon settings (on page)
- Toggle: apply coupon discount BEFORE promo-program discount (stacking order)
- Auto-gift coupon when printing/email/SMS/Zalo a retail bill or order, gated by Giá trị tối thiểu (min) / Giá trị tối đa (max) order value and Số lượng coupon tặng (qty)

### Columns
Chương trình, SP/Mô tả, Thời hạn, Số coupon (count issued), Số lần đã sử dụng (used), Số lần đã tặng (gifted), **Giá trị** (fixed amount e.g. 20.000 OR percentage e.g. 10%/20%), Giá trị tối đa (cap), **Độ dài** (code length, e.g. 5), **Tiền tố** (prefix e.g. AONEV, QWER, JULY5), **Hậu tố** (suffix), Người tạo, active. Codes auto-generated from prefix+random(length)+suffix.

---

## 3. Gifts — `/promotion/setting/gift`

6 gift programs. Filters: Áp dụng, ID, Khoảng ngày, Tên, Mô tả, Trạng thái, **Phiên bản** (V1 / V2).
Columns: ID, Ảnh, Chương trình, Ưu tiên, Mô tả, Áp dụng, Thời hạn, Trạng thái, Người tạo. Gift items not yet handed over are tracked at `/pos/debt/bill` (see pos-deep).

---

## 4. Other promotion config (routes confirmed)
- **Thiết lập bảng giá** `/promotion/setting/pricelist` — price tiers/lists (Retail/Wholesale/custom); drives the "Giá bán" selector on POS & order forms
- **Tích điểm** `/promotion/setting/point` — loyalty-point accrual rules (points per spend, redemption)
- **Hoa hồng bán hàng** `/promotion/setting/commission` — sales-staff commission rules

---

## API / route summary (Promotions)
- `GET /promotion/setting/discount` (tabs by status; v1/v2) — promotions
- `GET /promotion/setting/coupon` — coupons
- `GET /promotion/setting/gift` (v1/v2) — gifts
- `GET /promotion/setting/pricelist|point|commission` — price lists / points / commission

## Entities surfaced
- **Promotion**: id, name, engineVersion(v1/v2), type(qtyDiscount/perProduct/volumePrice), priority, scope(all/depot/product), startDate, endDate, status(active/upcoming/inactive/stopped), creator
- **Coupon**: id, programName, validity{start,end}, codeCount, usedCount, giftedCount, value(amount|percent), maxValue, codeLength, prefix, suffix, autoGiftRule{minOrder,maxOrder,qty,channels[print,email,sms,zalo]}, creator
- **Gift**: id, name, engineVersion(v1/v2), priority, scope, startDate, endDate, status, creator
- **PriceList**: id, name(Retail/Wholesale/custom), productPrices[]
- **PointRule**: accrualRate, redemptionRate
- **CommissionRule**: staffScope, rate, basis
