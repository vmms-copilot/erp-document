# 12 — Settings & RBAC

Business-wide configuration and access control.

## store/sale — Master business rules
- Central settings that govern selling behavior. Observed groups: sale/order defaults, pricing/VAT defaults, depot defaults, printing/receipt templates, numbering formats (order/bill/invoice codes), payment methods, shipping/carrier defaults, rounding rules.
- These defaults are inherited by POS, orders, and inventory flows.

## store/user — Users & RBAC
- Manage staff accounts, roles, departments, and depot scoping.
- User fields: name, login/email, phone, role(s), department, assigned depot(s), status, optional 2FA.
- Roles: named permission sets controlling access to modules/actions (view/create/edit/delete per feature).
- Departments: organizational grouping.
- Depot scoping: a user can be limited to specific depots.
- 2FA: optional two-factor for login.

### Business logic
- Permission checks gate every module/action; depot scope filters data a user can see/act on.
- Account creation, password setting, and 2FA enrollment are performed by the owner/user directly (not automated).
- Tenant isolation by businessId underlies all access.

## Entities (for schema)
- User (id, businessId, name, email, phone, departmentId, status, twoFactorEnabled)
- Role (id, businessId, name, permissionsJson)
- UserRole (userId, roleId)
- Department (id, businessId, name)
- UserDepot (userId, depotId)
- Depot (id, businessId, name, address)
- Setting (id, businessId, group, key, value)
