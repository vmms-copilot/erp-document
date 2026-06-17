# Deep Module Documentation — Index

Page-by-page deep dives of nhanh.vn (multi-channel retail / POS / ERP), captured live against the test business (businessId=137541, depots PHANH / PHANH 2 / PHANH 3). Each doc records: every page in the module, all workflow tabs, all filter controls with their full enum/option sets, grid columns, action menus, business-logic formulas, real backend routes, and the entities surfaced.

**Methodology**: read-only exploration (no records created/modified). Where a page had filters but showed no data, filters were widened (e.g. date range → 12 months, or ?fromDate&toDate URL params) to surface real records, per the documentation mandate.

## Modules
- **products-deep.md** — catalog, items, categories, variants/attributes, IMEI, price log
- **orders-deep.md** — order list, create order, order detail (timeline/API webhooks), COD reconciliation
- **pos-deep.md** — retail/wholesale sell screen, bill list, returns, e-invoice, gift debt
- **inventory-deep.md** — stock in/out slips (full mode taxonomy), transfers, stocktake, suppliers
- **customers-deep.md** — customer list (RFM tabs), CRM care log, groups, levels
- **accounting-deep.md** — double-entry cash/bank, 246-account chart (TT200/133), receivables/payables aging
- **promotions-deep.md** — discounts (v1/v2), coupons, gifts, price lists, points, commission
- **warranty-deep.md** — repair/RMA ticket workflow, statuses, service centers
- **channels-deep.md** — marketplace integrations (Shopee/TikTok/Lazada/Tiki/Facebook), sync & reconciliation
- **website-deep.md** — storefront CMS: content/template vars, news, menu, banners, redirects, i18n
- **settings-deep.md** — staff & RBAC roles, store config toggles, notifications, payment, branches
- **reports-deep.md** — BI suite: revenue cube, product/customer/promotion reports & formulas

## Supplementary deep dives
- **marketplaces-deep.md** — per-marketplace field-level structure: Shopee/Lazada/Tiki/TikTok connection grids + Shopee order sync (state & lifecycle tabs) + Facebook/Meta (Business + Page mapping); OAuth token expiry & sync-flag model
- **reports-catalog.md** — full catalog of ~90 report endpoints across 14 families, with the structure of the financial statements (P&L & Balance Sheet line-code formulas), the stock-ledger report (async generation, 4 period-blocks), and the cashier shift-reconciliation formulas

See the repo root (00-architecture.md … 14-database-schema.md) for the breadth-first overview and consolidated schema. The schema doc now includes **Appendix A** (concrete enum/lookup values) and **Appendix B** (Mermaid ER diagram + subsystem boundaries).
# Deep Module Documentation — Index

Page-by-page deep dives of nhanh.vn (multi-channel retail / POS / ERP), captured live against the test business (businessId=137541, depots PHANH / PHANH 2 / PHANH 3). Each doc records: every page in the module, all workflow tabs, all filter controls **with their full enum/option sets**, grid columns, action menus, business-logic formulas, real backend routes, and the entities surfaced.

Methodology: read-only exploration (no records created/modified). Where a page had filters but showed no data, filters were widened (e.g. date range → 12 months, or `?fromDate&toDate` URL params) to surface real records, per the documentation mandate.

## Modules
- [products-deep.md](./products-deep.md) — catalog, items, categories, variants/attributes, IMEI, price log
- [orders-deep.md](./orders-deep.md) — order list, create order, order detail (timeline/API webhooks), COD reconciliation
- [pos-deep.md](./pos-deep.md) — retail/wholesale sell screen, bill list, returns, e-invoice, gift debt
- [inventory-deep.md](./inventory-deep.md) — stock in/out slips (full mode taxonomy), transfers, stocktake, suppliers
- [customers-deep.md](./customers-deep.md) — customer list (RFM tabs), CRM care log, groups, levels
- [accounting-deep.md](./accounting-deep.md) — double-entry cash/bank, 246-account chart (TT200/133), receivables/payables aging
- [promotions-deep.md](./promotions-deep.md) — discounts (v1/v2), coupons, gifts, price lists, points, commission
- [warranty-deep.md](./warranty-deep.md) — repair/RMA ticket workflow, statuses, service centers
- [channels-deep.md](./channels-deep.md) — marketplace integrations (Shopee/TikTok/Lazada/Tiki/Facebook), sync & reconciliation
- [website-deep.md](./website-deep.md) — storefront CMS: content/template vars, news, menu, banners, redirects, i18n
- [settings-deep.md](./settings-deep.md) — staff & RBAC roles, store config toggles, notifications, payment, branches
- [reports-deep.md](./reports-deep.md) — BI suite: revenue cube, product/customer/promotion reports & formulas

See the repo root (`00-architecture.md` … `14-database-schema.md`) for the breadth-first overview and consolidated schema.
