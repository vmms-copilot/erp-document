# Warranty Module (Bảo hành) — Deep Documentation

Module path: `/warranty/*` · Menu: "Bảo hành" · businessId=137541
A repair/warranty-ticket workflow subsystem (RMA), linked to products & IMEI/serials.

Menu routes (captured from nav DOM):
- Bảo hành / Lịch sử bảo hành (Warranty tickets) → `/warranty/history/index`
- Trung tâm bảo hành (Service centers) → `/warranty/setting/servicecenter`
- Danh mục & sản phẩm bảo hành (Warranty categories & products) → `/warranty/setting/category`
- Lý do bảo hành (Warranty reasons) → `/warranty/setting/reason`
- Trạng thái sản phẩm, phiếu bảo hành (Statuses) → `/warranty/setting/productstatus`
- Bảo hành mở rộng (Extended warranty) → `/warranty/setting/extend`

---

## 1. Warranty Tickets — `/warranty/history/index`

3 tickets present. Repair-workflow tabs (with counts):
- Tất cả (All)
- Mới (New, badge 2)
- Chờ link kiện (Waiting for parts)
- Đang sửa (Repairing)
- Đã sửa xong (Repaired)
- Trả hôm nay (Return today)
- Quá hạn trả (Overdue return)

### Filters
Cửa hàng, ID, Ngày tiếp nhận (intake date), Loại (Type), **IMEI** (serial lookup), Khách hàng.

### Columns
ID (+ intake date), Khách hàng (name+phone), Sản phẩm (item, may carry IMEI), Loại (Bảo hành / warranty), **Phí** (repair fee, + add), **Lý do** (reason: Hỏng/Broken, Lỗi/Defect — from reason lookup), **Hẹn trả** (promised return date; turns red when overdue), **status pills** (dual): repair status (Mới tiếp nhận / Đã sửa xong) + return status (Chưa trả khách / Đã trả khách + datetime), **Người sửa** (technician). Buttons: Thêm mới, Thao tác.

A ticket carries two independent state machines: (a) repair lifecycle (New → waiting parts → repairing → repaired) and (b) customer-return state (not returned → returned).

---

## 2. Warranty Status config — `/warranty/setting/productstatus`

User-defined statuses, 2 records, with a **Loại** discriminator:
- "Trạng thái phiếu bảo hành" (Ticket status) — e.g. "Còn bảo hành" (Still under warranty)
- "Trạng thái sản phẩm" (Product condition status) — e.g. "Pin phồng" (Swollen battery)
Columns: Tên, Loại, Trạng thái (active), Người tạo. Filters: Ngày tạo, Tên, Loại, Trạng thái. Add/Delete supported.

---

## 3. Other warranty config (routes confirmed)
- **Trung tâm bảo hành** `/warranty/setting/servicecenter` — service-center directory (where repairs are sent)
- **Danh mục & sản phẩm bảo hành** `/warranty/setting/category` — which product categories/products are warrantable + warranty periods
- **Lý do bảo hành** `/warranty/setting/reason` — reason lookup (Hỏng, Lỗi, ...)
- **Bảo hành mở rộng** `/warranty/setting/extend` — extended-warranty plans

---

## API / route summary (Warranty)
- `GET /warranty/history/index` (tabs by repair stage; filter by IMEI) — tickets
- `GET /warranty/setting/productstatus` — statuses
- `GET /warranty/setting/servicecenter|category|reason|extend` — config lookups

## Entities surfaced
- **WarrantyTicket**: id, depotId, intakeDate, customerId{name,phone}, productId, imei?, type(warranty), fee, reasonId(Hỏng/Lỗi), promisedReturnDate, repairStatus(new/waitingParts/repairing/repaired), returnStatus(notReturned/returned+at), technicianId, serviceCenterId?
- **WarrantyStatus**: id, name, kind(ticketStatus|productStatus), active, creator
- **WarrantyReason**: id, name (lookup)
- **ServiceCenter**: id, name, address/contact
- **WarrantyCategory**: categoryId/productId, warrantyPeriod
- **ExtendedWarranty**: planId, productScope, duration, price
