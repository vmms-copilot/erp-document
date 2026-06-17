# Accounting Module (Kế toán) — Deep Documentation

Module path: `/accounting/*` · Menu: "Kế toán" · businessId=137541
This is a full double-entry accounting subsystem built on the Vietnamese standard chart of accounts (TT 200/133), linked to source documents (orders, POS bills, stock slips).

Menu routes (captured from nav DOM):
- Thu chi tiền mặt (Cash in/out) → `/accounting/transaction/cash` ; create `/accounting/transaction/addcash`
- Thu chi ngân hàng (Bank in/out) → `/accounting/transaction/bank` ; create `/accounting/transaction/addbank`
- Tổng hợp thu chi (Cashbook) → `/accounting/transaction/cashbook`
- Bút toán / Kế toán (Journal entries) → `/accounting/transaction/index`
- Phát sinh đối ứng (Reciprocal/contra postings) → `/accounting/transaction/reciprocalaccount`
- Nhật ký chung (General journal) → `/accounting/transaction/logbook`
- Lịch sử (History) → `/accounting/transaction/logs`
- Tài khoản kế toán (Chart of accounts) → `/accounting/account/index`
- Công nợ khách hàng (Receivables) → `/accounting/debts/customer`
- Công nợ nhà cung cấp (Payables) → `/accounting/debts/supplier`
- Công nợ Sàn TMĐT (Marketplace debt) → `/accounting/debts/merchant`
- Công nợ NVBH (Sales-staff debt) → `/accounting/debts/salemandate`
- Công nợ theo đầu tài khoản → `/accounting/debts/byaccount`
- Nhập công nợ đầu kỳ (Opening balances) → `/accounting/debts/add`

---

## 1. Cash In/Out — `/accounting/transaction/cash`

A double-entry cash ledger. Filters: Kho hàng, ID, Kiểu ngày (Date type: Ngày giao dịch / transaction date), Khoảng ngày (range), Chứng từ (has-document: Có / Không), ID chứng từ, Số tiền.

### Columns
ID | Ngày, Loại (direction arrow), **Tài khoản** (account, e.g. `11111 Quỹ tiền cửa hàng`), **Tài khoản đối ứng** (contra account, e.g. `131 Phải thu khách hàng`), Đối tượng (party, e.g. `KH.9953715 Pamela`), **Chứng từ** (source doc, links to slip e.g. "Phiếu XNK: 14542796"), **Thu** (debit/in), **Chi** (credit/out), Người tạo. Buttons: Lập phiếu thu chi (create receipt/payment voucher), Xuất dữ liệu, In phiếu đã chọn.

Each cash receipt auto-posts a balanced entry (e.g. Dr 11111 / Cr 131) tied to its originating POS bill / order.

## 2. Bank In/Out — `/accounting/transaction/bank`
Same structure as cash but against bank-deposit accounts (112x). Create via `/accounting/transaction/addbank`.

---

## 3. Chart of Accounts — `/accounting/account/index`

**Full Vietnamese standard chart of accounts** (TT 200/133): 246 accounts, hierarchical parent→child by code. Filters: ID Tài khoản, Loại. Columns: #, ID, **Code**, **Tên**, Kho hàng (depot binding for store-scoped accounts), Tình trạng (active), Người tạo. Add via Thêm mới.

### Account tree highlights (code — name)
- 111 Tiền mặt → 1111 → **11111 Quỹ tiền cửa hàng**, **11112 Quỹ trả góp**, **11113 Quỹ quẽt thẻ** (custom store sub-funds)
- 112 Tiền gửi ngân hàng → 1121 (depot-bound, e.g. PHANH)
- 131 Phải thu khách hàng (Receivables) · 138 Phải thu khác
- 133 Thuế GTGT được khấu trừ (VAT input credit)
- 156 Hàng hóa → 1561 Giá mua, 1562 Chi phí thu mua → **15621 Chi phí vận chuyển** (custom)
- 211/213/214 Fixed assets & depreciation
- 331 Phải trả người bán (Payables) · 333 Thuế phải nộp (33311 VAT output, 3334 CIT, 3335 PIT...)
- 334 Phải trả người lao động · 338 Phải trả khác (3383 BHXH, 3384 BHYT, 3386 BHTN)
- 411 Vốn chủ sở hữu · 421 Lợi nhuận chưa phân phối
- 511 Doanh thu → 5111 bán hàng hóa · 521 Giảm trừ DT (5211 chiết khấu TM, 5212 hàng trả lại, 5213 giảm giá)
- 611 Mua hàng · 632 Giá vốn hàng bán (COGS)
- 641 Chi phí bán hàng → 6415 bảo hành, **64181 Chi phí Ads** (custom) · 642 Chi phí QLDN
- 711 Thu nhập khác · 811 Chi phí khác · 821 Chi phí thuế TNDN · **911 Xác định kết quả kinh doanh** (P&L close)
- Custom user accounts seen: 4567 MUSIC, 5678 CENTER, 6789 TP Bank 0042, 98765 ABC, 12345 ENGLISH, 12346 COURSE

---

## 4. Receivables (Customer Debt) — `/accounting/debts/customer`

Debt-aging ledger. Aging tabs (with counts): Tất cả · Hạn thanh toán (Due, 128) · Nợ quá hạn (Overdue, 127) · Hạn hôm nay (Due today) · Hạn 7 ngày tới (Due in 7d) · Hạn trên 7 ngày (Due >7d).
Filters: ID, Thời gian, Khách hàng, Loại khách hàng, Công nợ.

### Columns (explicit balance formula)
Khách hàng, SĐT, **Số dư đầu kỳ** [Nợ=4 / Có=5], **Phát sinh trong kỳ** [Ghi nợ=6 / Ghi có=7], **Số dư cuối kỳ**: Nợ[Phải thu] = 4+6-5-7 ; Có[Phải trả] = 5+7-4-6. Buttons: Xuất dữ liệu, Tính tổng phải thu khách hàng (recompute receivables).

Parallel pages: **/supplier** (payables, account 331), **/merchant** (marketplace platform settlements), **/salemandate** (sales-staff cash held), **/byaccount** (debt grouped by GL account), **/add** (opening-balance entry).

---

## API / route summary (Accounting)
- `GET /accounting/transaction/cash|bank` — cash/bank ledgers (double-entry)
- `GET /accounting/transaction/cashbook|logbook|index` — cashbook / general journal / journal entries
- `GET /accounting/account/index` — chart of accounts (246 accounts)
- `GET /accounting/debts/customer|supplier|merchant|salemandate|byaccount` — debt aging

## Entities surfaced
- **Account (GL)**: id, code, name, parentCode, depotId?, active, creator (hierarchical TT200/133 tree)
- **Transaction (voucher)**: id, date, direction(thu/chi), accountCode, contraAccountCode, partyType+partyId, sourceDocType+sourceDocId, debit, credit, depotId, creator
- **Debt (receivable/payable)**: partyId, partyType, openingDr, openingCr, periodDr, periodCr, closingDr(=4+6-5-7), closingCr(=5+7-4-6), dueDate, agingBucket
