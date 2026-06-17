# 00 — System Architecture & API Patterns

## Overview
nhanh.vn is a multi-channel retail / POS / ERP platform delivered as an Angular single-page application (SPA). The browser loads a page shell over GET, then fetches data over POST as JSON.

## Request / Response model
- Navigation (GET): returns the SPA shell HTML for a route, e.g. /{module}/{controller}/{action}?businessId=137541
- Data (POST): returns JSON for lists, details, form submits.
- Standard response envelope:

```json
{
  "code": 1,
  "errorCode": 0,
  "messages": [],
  "data": { }
}
```

- code: 1 = success, 0 = failure (observed).
- errorCode: business error code, 0 on success.
- messages: array of human-readable strings (Vietnamese).
- data: payload (object for detail, array/paged object for lists).

## Multi-tenancy & scoping
- Every route carries businessId (observed: 137541) as a query param — the tenant key.
- Account display name: "Khách test"; primary user: "Nguyễn Thị Phương Anh".
- Depots / warehouses (3): PHANH, PHANH 2, PHANH 3. Stock, imports, transfers, and POS bills are all scoped to a depot.

## URL structure
```
https://nhanh.vn/{module}/{controller}/{action}?businessId={tenant}
```
- 21 top-level modules; ~235 distinct paths (see 01-sitemap.md).

## Cross-cutting patterns
- Lists: server-side paged; filters posted as JSON; status/segment expressed as tabs backed by enums.
- Forms: custom Angular dropdown components (not native select) — values are set via component clicks.
- Audit: price changes and stock movements are logged (product price-change log had 1177 entries).
- Money: integer VND amounts (no decimals).
- i18n: UI is Vietnamese.

## Security notes (for rebuild)
- RBAC with roles, departments, depot scoping, and optional 2FA (see 12-settings.md).
- Tenant isolation must be enforced on every query by businessId.
