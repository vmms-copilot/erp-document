# 01 — Sitemap

The application exposes ~235 distinct paths grouped into 21 top-level modules. Paths follow /{module}/{controller}/{action}. Listed below are the primary observed routes per module (representative, not exhaustive).

## 1. product — Products
- product/item/index — product list
- product/item/add — add product
- product/item/detail — product detail
- product/item/inventory — per-depot stock matrix
- product/item/storagetime — storage time tracking
- product/item/logprice — price-change audit log (1177 entries observed)
- product/category/index — categories (hierarchical, 63 observed)
- product/variant/index — attributes (Mua / Kich thuoc / Mau sac)
- product/brand/index — brands
- product/imei/index — IMEI / serial registry (620 observed)
- product/batch/index — batch / lot tracking

## 2. order — Orders
- order/manage/index — order list (status tabs)
- order/manage/add — create order
- order/manage/checkduplicate — duplicate detection
- order/packing — packing
- order/sendingcarrier — handoff to carrier
- order/complain — complaints
- order/payment/index — order payments
- order/deleted — deleted orders

## 3. pos / bill — Point of Sale
- bill/index — POS bill list
- bill/add — new sale (depot-select modal, F-key shortcuts)
- return/index — returns
- invoice/index — invoices
- debt/bill — POS debt

## 4. inventory (imexport) — Stock
- bill/index — import/export bill list
- bill/import — create import (type enum: supplier / other)
- check/index — stocktake
- transfer/index — inter-depot transfer
- archive/index — archive
- position/index — bin/shelf positions
- product/damaged — damaged goods

## 5. customer — Customers
- code/customerlist — customer list
- care/index — customer care
- group/index — customer groups (VIP1/2/3)
- store/level — loyalty tiers

## 6. accounting — Accounting
- transaction/index — journal (double-entry)
- transaction/addcash — cash entry
- debts/customer — customer debts

## 7. promotion — Promotions
- setting/discount — discounts
- setting/point — loyalty points config

## 8. warranty — Warranty
- history/index — warranty history

## 9. channel — E-commerce channels
- ecommerce/manage/setting — Shopee / Tiktok / Lazada / Tiki / Facebook integrations

## 10. website — Website CMS
- content/index — key-value content store

## 11. store — Settings & RBAC
- store/sale — master business rules
- store/user — users / roles / departments / 2FA (RBAC)

## 12. report — Reports
- report/revenue/index — revenue report
- (79 report variants total)

## Other modules
finance, supplier, marketing, voucher/gift, pricelist, commission, forecast, requirement and settings sub-pages follow the same list/form/report patterns described in 00-architecture.md.
