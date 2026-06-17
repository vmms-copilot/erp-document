# Channels Module (Kênh bán / Marketplace Integrations) — Deep Documentation

Module path: `/ecommerce/*` · Menu: "Kênh bán" · businessId=137541
This module connects the store to e-commerce marketplaces and social channels, syncing products, orders, stock, and finance bidirectionally.

Menu structure (captured from nav DOM): Kênh bán → Cửa hàng (POS store) · Shopee · TikTok · Lazada · Tiki · Facebook Shop · Tài chính sàn TMĐT · Hóa đơn điện tử · + Thêm kênh bán.
Each marketplace has the SAME 3 sub-items: **Sản phẩm** (product/listing sync), **Đơn hàng** (order sync), **Đánh giá** (reviews).

Routes (Shopee pattern; identical for tiktok/lazada/tiki/facebook):
- `/ecommerce/shopee/product` — product/listing sync
- `/ecommerce/shopee/order` — order sync
- `/ecommerce/shopee/productreview` — reviews
- `/ecommerce/manage/setting` — channel connections (add/manage)
- `/ecommerce/manage/index` — reconciliation (Đối soát)
- `/ecommerce/manage/cods` — COD reconciliation
- `/ecommerce/manage/return` — marketplace returns/refunds

---

## 1. Channel Connections — `/ecommerce/manage/setting`

Breadcrumb: Kênh bán / Kết nối sàn. The control center for all integrations.

### Channel sidebar (with connection counts)
Tất cả (9) · Shopee (2) · Tiktok · Lazada · Tiki · Facebook Shop (7) · Tiếp thị liên kết (Affiliate) · Website · Chat đa kênh (Omni-channel chat) · Hóa đơn điện tử · Zalo Mini App. Button: Thêm kết nối (Add connection).

### Columns
**Tên gian hàng | Shop ID** (shop name + marketplace shop id, e.g. PHANHNGUYEN277 / 193915742), **Đồng bộ sản phẩm** (product-sync mode: "Đợi ghép sản phẩm giữa 2 bên" = wait for mapping, or "Luôn lấy sản phẩm từ sàn TMĐT về" = always pull from marketplace), **Cập nhật tồn kho** (stock-sync on/off), **Đồng bộ đơn hàng** (order sync: Tự động = auto), **Hạn sử dụng** (subscription expiry), **Hạn token** (OAuth token expiry + "Gia hạn token" renew button; expired tokens flagged red).

### Integration value (page footer, paraphrased)
Sync products/orders/stock with marketplaces — stock change on one channel propagates to ALL channels; track shipped/delayed/returned goods; financial reporting + revenue reconciliation; issue e-invoices for marketplace orders.

---

## 2. Marketplace Order Sync — `/ecommerce/shopee/order` (per channel)

Breadcrumb: Kênh bán / Shopee / Đơn hàng.

### Sync-state tabs
Đơn sàn (Marketplace orders) · Chưa đồng bộ (Not synced) · Đã đồng bộ (Synced).
### Lifecycle sub-tabs (with counts)
Tất cả · Chờ xác nhận · Chờ lấy hàng (Awaiting pickup) · Đang giao · Đã huỷ · Trả hàng · Xuất kho.

### Filters
ID, **ID Shopee** (marketplace order id), Mã vận đơn HVC (carrier tracking), Gian hàng (shop), Trạng thái, Sản phẩm, Khách hàng, Ngày tạo, Hãng vận chuyển, Kho hàng. Buttons: **Tải đơn** (pull orders from marketplace), Thao tác, In đơn. Columns: Sản phẩm, Giá bán, Thành tiền, Vận chuyển, Thanh toán.

(Đơn hàng / Đánh giá sync pages follow the same per-marketplace pattern.)

---

## 3. Reconciliation — `/ecommerce/manage/index` & `/ecommerce/manage/cods`

Breadcrumb: Kênh bán / Đối soát. Tabs: POS Đơn tự kết nối / POS Đơn gửi qua Nhanh; Lịch sử đối soát (reconciliation history) / Chênh lệch đối soát (discrepancies). Filters: kênh bán, Ngày thanh toán, ID đơn hàng, ID thanh toán, Mã đơn sàn. Columns: ID, Ngày thanh toán, Số đơn hàng, Số tiền, Ghi chú, Người tạo, Ngày tạo. Compares marketplace settlement vs internal records and surfaces fee/amount discrepancies. (See also order-deep COD reconciliation.)

---

## API / route summary (Channels)
- `GET /ecommerce/manage/setting` — channel connections (token, sync modes)
- `GET /ecommerce/{shopee|tiktok|lazada|tiki|facebook}/product|order|productreview`
- `GET /ecommerce/manage/index|cods|return` — reconciliation / COD / returns

## Entities surfaced
- **ChannelConnection**: id, platform(shopee/tiktok/lazada/tiki/facebook/zaloMiniApp/affiliate), shopName, shopId, productSyncMode(waitMapping/alwaysPull), stockSync(bool), orderSync(auto), subscriptionExpiry, oauthToken{value,expiry}
- **MarketplaceOrder**: id, platform, marketplaceOrderId, shopId, carrierTrackingCode, status(awaitConfirm/awaitPickup/shipping/cancelled/returned/stockedOut), syncState(notSynced/synced), items[], shipping, payment, customer, createdAt
- **ProductListingSync**: localProductId, platform, marketplaceItemId, mappingStatus
- **Review**: platform, productId, rating, content, date
- **MarketplaceReconciliation**: paymentDate, orderCount, amount, platformFees, discrepancy, note
