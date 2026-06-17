# nhanh.vn ERP — Reverse-Engineering Documentation

Reconstruction documentation for the nhanh.vn multi-channel retail / POS / ERP platform, captured for a rebuild project.

- Platform: Angular SPA, GET = page shell + POST = JSON data
- Response envelope: { code, errorCode, messages, data }
- Business context: businessId=137541, 3 depots (PHANH, PHANH 2, PHANH 3)

## Documents

| File | Module |
|------|--------|
| 00-architecture.md | System architecture & API patterns |
| 01-sitemap.md | Full sitemap (235 paths, 21 modules) |
| 02-products.md | Products, variants, IMEI, batches, inventory |
| 03-orders.md | Order management lifecycle |
| 04-pos.md | Point of Sale |
| 05-inventory.md | Stock / import-export / transfer |
| 06-customers.md | Customers, groups, loyalty |
| 07-accounting.md | Double-entry accounting & debts |
| 08-promotions.md | Discounts & points |
| 09-warranty.md | Warranty history |
| 10-channels.md | E-commerce channel integrations |
| 11-website.md | Website CMS |
| 12-settings.md | Store settings & RBAC |
| 13-reports.md | Reports |
| 14-database-schema.md | Consolidated relational schema |
