# 11 — Website CMS

Lightweight content management for the storefront/landing website tied to the business.

## content/index — Content store
- Key-value content management: each entry has a key/slug and a value (text/HTML/JSON), used to render storefront sections (banners, about, policies, contact, SEO meta, etc.).
- Fields: key, title, value (rich text / HTML), type, status (published/draft), updatedAt.

### Business logic
- The storefront reads published content entries by key to populate pages/blocks.
- Decouples editable copy from code; supports draft vs published.
- May include media references and SEO fields.

## Entities (for schema)
- WebContent (id, businessId, key, title, type, value, status, updatedAt)
- WebMedia (id, businessId, url, alt, type) — referenced media
