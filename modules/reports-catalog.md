# Reports — Full Catalog (Deep)

Complete route catalog of the Reporting subsystem (Báo cáo) on nhanh.vn, captured live from the nav menu (businessId 137541). ~90 report endpoints across 14 families. Reports are read models projected over the transactional tables (see 14-database-schema.md); they own no canonical state.

## How reports work
- Most reports share a common shell: a filter bar (Hiển thị / Ngày date-range / Kho hàng depot / Thương hiệu brand / Danh mục category) + **Lọc** (filter) button, an **Xuất dữ liệu** (export) and **In báo cáo** (print) action, and a **Tùy chỉnh hiển thị** column-picker.
- Heavier reports (e.g. Xuất nhập tồn) are **generated asynchronously**: clicking Lọc shows "Đang tạo báo cáo" with a progress bar, assigns a `reportId` (URL param), and stores the result under a **Báo cáo đã lưu** (saved reports) tab.
- **Empty by default**: opening a report with the default current-week range often shows an empty grid or "Xin vui lòng chọn các tiêu chí và bấm lọc". Widen the date range (or set criteria) and click Lọc to surface real data.
- Column headers carry **formula footnotes** in [n] notation (e.g. Doanh thu [13 = 10-11-12]).

---

## 1. Doanh thu (Revenue) — /report/revenue/*
| Route | Report |
|---|---|
| /report/revenue/index | Theo thời gian (by time) |
| /report/revenue/depot | Theo cửa hàng |
| /report/revenue/brand | Theo thương hiệu |
| /report/revenue/staff | Theo nhân viên |
| /report/revenue/department | Theo phòng ban |
| /report/revenue/category | Theo danh mục sản phẩm |
| /report/revenue/internalcategory | Theo danh mục nội bộ |
| /report/revenue/product | Theo sản phẩm |
| /report/revenue/supplier | Theo nhà cung cấp |
| /report/revenue/customer | Theo khách hàng |
| /report/revenue/inventoryrate | Tỷ suất doanh thu / tồn kho |

**Revenue grid columns** (by time): Đơn thành công [4], Bán lẻ [5], Bán sᢁ [6], Phí trả hàng [9], Chiết khấu [11], Tiêu điểm [12], **Doanh thu [13 = 10-11-12]**, Giá vốn [16], **Lợi nhuận [17]**.
Formulas: Lợi nhuận = Doanh thu đã trừ chiết khấu - Tổng giá vốn XNK + Phí trả hàng - Điểm đã dùng. Đơn đặt = orders in statuses Mới/Chờ xác nhận/Đang xác nhận/Đã xác nhận/Đổi kho/Đang đóng gói/Đã đóng gói/Chờ thu gom/Đang chuyển/Thành công.

## 2. Đơn hàng (Orders) — /report/order/*
| Route | Report |
|---|---|
| /report/order/salechannel | Theo kênh bán |
| /report/order/index | Đơn tạo |
| /report/order/success | Đơn thành công |
| /report/order/value | Theo giá trị đơn hàng |
| /report/order/category | Theo danh mục sản phẩm |
| /report/order/product | Theo sản phẩm |
| /report/order/status | Theo trạng thái |
| /report/order/location | Theo địa chỉ |
| /report/order/cancel | Lý do xử lý đơn hàng |
| /report/order/create | Nhân viên xử lý |
| /report/order/utm | Theo quảng cáo (UTM) |
| /report/order/verifyandtransfer | Tiền đối soát |

## 3. Bán lẻ (Retail / POS) — /report/retail/*
| Route | Report |
|---|---|
| /report/retail/index | Tổng quan |
| /report/retail/trafficsource | Theo nguồn khách hàng |
| /report/retail/cashier | Theo nhân viên |
| /report/retail/depot | Theo cửa hàng |
| /report/retail/creditmoney | Chi tiết quẹt thẻ |
| /report/retail/billvalue | Theo giá trị hóa đơn |
| /report/retail/customerbillrate | Tỷ lệ hóa đơn / khách vào cửa hàng |
| /report/retail/workshift | Báo cáo kết ca |

**Kết ca (shift reconciliation)** — 4 summary cards (Tổng hóa đơn / Tổng thu / Tổng chi phí / Chênh lệch), tabs Báo cáo + Chi phí, filters Thời gian + Kho hàng + Nhân viên. Cash-drawer formulas:
- Tiền mặt trong ca [12] = Tiền mặt [9] - (chi phí [10] + trả hàng bằng tiền mặt [11])
- Tiền mặt cuối ca [13] = Tiền đầu ca [5] + Tiền trong ca [12]
- Chênh lệch ca [15] = Tiền bàn giao thực tế [14] - Tiền mặt cuối ca [13]

## 4. Bán sᢁ (Wholesale) — /report/wholesale/*
| Route | Report |
|---|---|
| /report/wholesale/index | Tổng quan |

## 5. Kho hàng (Inventory) — /report/inventory/*
| Route | Report |
|---|---|
| /report/inventory/index | Xuất nhập tồn theo sản phẩm |
| /report/inventory/imex | Chi tiết sản phẩm XNK |
| /report/inventory/imextotal | Tổng XNK |
| /report/inventory/imexdepot | Tổng XNK theo cửa hàng |
| /report/inventory/supplier2 | Theo nhà cung cấp |
| /report/inventory/category | Danh mục sản phẩm |
| /report/inventory/eachdepot | Số lượng hàng tồn kho |
| /report/inventory/transfer | Chuyển kho chưa xác nhận |
| /report/inventory/imei | Theo IMEI |
| /report/inventory/depotinventorystatus | Theo trạng thái từng cửa hàng |
| /report/inventory/productinventorystatus | Theo trạng thái từng sản phẩm |
| /report/inventory/productbatchs | Theo lô hàng |
| /report/inventory/producttransfer | Chuyển kho theo sản phẩm |

**Xuất nhập tồn (stock ledger)** — async-generated (reportId, saved). Filters: Hiển thị / Ngày / Kiểu XNK / Kho hàng / Thương hiệu / **Giá vốn method** (Tính theo giá vốn cuối kỳ | trung bình trong kỳ) / Tồn hiện tại. Columns: Mã, Sản phẩm, Tồn hiện tại, then 4 period-blocks each with SL / Giá vốn / Thành tiền: **Tồn đầu kỳ → Nhập trong kỳ → Xuất trong kỳ → Tồn cuối kỳ**. Cost basis: "cuối kỳ" = last XNK cost in range (excludes transfer & IMEI-direct slips); "trung bình" = average over range; IMEI/đích danh products use last supplier import cost.

## 6. Sản phẩm (Products) — /report/product/*
| Route | Report |
|---|---|
| /report/product/index | Bán chạy nhất |
| /report/product/depot | Bán chạy theo cửa hàng |
| /report/product/salespeed | Tốc độ bán hàng |
| /report/product/salechannel | Theo kênh bán |
| /report/product/categorydetail | Theo danh mục và cửa hàng |
| /report/product/value | Theo khoảng giá |
| /report/product/date | Theo ngày |
| /report/product/salebyimei | Bán hàng theo IMEI |
| /report/product/attribute | Theo thuộc tính |

## 7. Bảo hành (Warranty) — /report/warranty/*
| Route | Report |
|---|---|
| /report/warranty/date | Doanh thu theo ngày |
| /report/warranty/depot | Doanh thu theo cửa hàng |
| /report/warranty/staff | Doanh thu theo nhân viên |
| /report/warranty/accessories | Doanh thu thay thế linh kiện |
| /report/warranty/status | Trạng thái theo cửa hàng |
| /report/warranty/staffstatus | Trạng thái theo nhân viên |
| /report/warranty/center | Theo trung tâm bảo hành |

## 8. Kế toán (Accounting reports) — /accounting/report/*
| Route | Report |
|---|---|
| /accounting/report/cash | Tổng hợp thu chi theo cửa hàng |
| /accounting/report/account | Tổng hợp theo tài khoản |
| /accounting/report/money | Tổng hợp tiền bán lẻ hàng ngày theo cửa hàng |
| /accounting/report/cashbook | Tổng hợp thu chi theo ngày |
| /accounting/report/businessresult | **Tổng hợp kết quả kinh doanh (P&L)** |
| /accounting/report/balancesheet | **Bảng cân đối kế toán (Balance Sheet)** |

**P&L (kết quả kinh doanh)** — 16 line items mapped to VN account codes with formulas: Doanh thu (511), Giảm trừ (521), DT thuần A=511+521, Giá vốn (632), Lợi nhuận gộp B=A+632, DT tài chính (515), CP tài chính (635), CP bán hàng (641), CP QLDN (642), LN thuần C=B+515+635+641+642, Thu nhập khác (711), CP khác (811), LN khác D=711+811, Tổng LN trước thuế E=C+D, CP thuế TNDN, **LN sau thuế F=E+821**. Columns: Chỉ tiêu / Giá trị / % doanh thu / Mô tả.

**Balance Sheet (cân đối kế toán)** — full TT200 form: Tài sản / Mã số / Mô tả (derivation) / Số kỳ này / Số kỳ trước. Line codes A.100 (Tài sản ngắn hạn = 110+120+130+140+150), 110 (Tiền = 111+112, Số Dư Nợ TK 111/112/113), 130 receivables (131 = Số Dư Nợ TK 131), etc. Each line maps to a ledger-balance expression over the chart of accounts.

## 9. Sổ kế toán hộ kinh doanh (Household-business books) — /report/household/*
| Route | Report |
|---|---|
| /report/household/s1a | Mẫu sổ S1a-HKD (DT ≤ 500 triệu) |
| /report/household/s2a | Mẫu sổ S2a-HKD (DT 500tr – 3 tỷ) |
| /report/household/s2b | Mẫu sổ S2b-HKD (DT > 3 tỷ) |
| /report/household/s2c | Mẫu sổ S2c-HKD (chi tiết doanh thu, chi phí) |
| /report/household/s2d | Mẫu sổ S2d-HKD (chi tiết vật liệu, dụng cụ, sản phẩm, hàng hóa) |

These are the statutory tax-bookkeeping templates for hộ kinh doanh (household businesses) per Vietnamese Circular 88/2021/TT-BTC, selected by revenue tier.

## 10. Khách hàng (Customers) — /report/customer/*
| Route | Report |
|---|---|
| /report/customer/index | Tổng quan |
| /report/customer/product | Theo sản phẩm |
| /report/customer/total | Tỷ lệ khách quay lại |
| /report/customer/level | Cấp độ khách hàng |
| /report/customer/group | Nhóm khách hàng |
| /report/customer/createnew | Khách tạo mới theo cửa hàng |
| /report/customer/frequencybought | Chu kỳ mua hàng |
| /report/customer/birthday | Sinh nhật khách hàng |

## 11. Khuyến mại (Promotions) — /report/promotion/*
| Route | Report |
|---|---|
| /report/promotion/discountrate | Tỉ lệ với doanh thu |
| /report/promotion/productgift | Quà tặng theo sản phẩm |
| /report/promotion/productdiscountrate | Theo sản phẩm |
| /report/promotion/bonusbydepot | Quà tặng theo cửa hàng |
| /report/promotion/pricelist | Doanh thu theo bảng giá |

## 12. Zalo — /report/zalo/*
| Route | Report |
|---|---|
| /report/zalo/historydate | Tin gửi theo ngày |
| /report/zalo/template | Tin gửi theo mẫu |
| /report/zalo/rating | Đánh giá của khách hàng |

## 13. SMS — /report/sms/*
| Route | Report |
|---|---|
| /report/sms/date | Tin gửi theo ngày |

## 14. Related ledgers (cross-listed under Kế toán & Sổ kế toán menus)
| Route | View |
|---|---|
| /accounting/transaction/cashbook | Tổng hợp thu chi |
| /accounting/debts/customer | Công nợ khách hàng |
| /accounting/debts/supplier | Công nợ nhà cung cấp |
| /accounting/debts/merchant | Công nợ sàn TMĐT |
| /accounting/debts/salemandate | Công nợ nhân viên bán hàng |
| /accounting/debts/byaccount | Công nợ theo đầu tài khoản |
| /ecommerce/manage/reportads | Tiền quảng cáo |

---

## Rebuild notes for the reporting subsystem
- Implement reports as **read models / materialized views** over the OLTP tables; never let a report mutate canonical state.
- Provide a shared **async report-generation queue** (returns a reportId, persists results to a "saved reports" store) for heavy aggregations like stock ledger.
- The financial statements (P&L, Balance Sheet) are pure **projections over the chart of accounts + journal_line** — each line is a signed sum of account balances per the TT200 derivation strings shown in the Mô tả column.
- Household-business books (S1a/S2a-d) are statutory templates keyed by revenue tier (Circular 88/2021/TT-BTC).
- Every report respects: date-range, depot, brand, category, and (where relevant) cost-basis method and current-stock filter. Default ranges are narrow — widen to populate.
