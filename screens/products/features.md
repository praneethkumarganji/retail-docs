# Products — Features (Detailed)

This document expands each Products feature into implementation steps, APIs, UX details, and accessibility requirements.

1) Product List & Filters
- Purpose: allow users to discover and manage catalog items efficiently.
- Filters: category, status, stock level, price range, tags, vendor, channel.
- APIs:
  - GET /bff/products?filters...
  - GET /bff/products/counts?filters
- UX:
  - Server-side pagination (cursor-based), column sorting, saved filter presets.
  - Keyboard shortcuts for quick actions; accessible table markup.
- Implementation:
  1. BFF accepts validated filter params and applies tenant scoping.
  2. Return items with minimal fields for list; hydrate details on demand.

2) Bulk Actions & Import/Export
- Purpose: perform mass updates and bulk data ingestion.
- Bulk actions:
  - publish/unpublish, price updates, tag updates, delete, export CSV.
- Import:
  - CSV/Excel upload with server-side validation, mapping UI, background import job.
- APIs:
  - POST /bff/products/import (returns job id)
  - GET /bff/products/import/:jobId/status
  - POST /bff/products/batch (bulk actions)
- UX:
  - Show import progress, errors, and per-row fix suggestions.

3) Product Editor & Variants
- Purpose: full product authoring experience including variants and SEO.
- Editor sections:
  - Details: title, description, attributes
  - Media: gallery, alt text, primary image
  - Pricing: base price, compare-at, multi-currency
  - Inventory: stock per location, SKU per variant
  - SEO: meta tags, structured data preview
- APIs:
  - GET /bff/products/:id
  - PATCH /bff/products/:id
  - POST /bff/products/:id/media
  - POST /bff/products/:id/variants
- UX:
  - Inline quick-edit and full editor modal; save drafts and publish flow.
  - Variant matrix editor for combinations.

4) Media Management & CDN
- Purpose: handle images and video efficiently and tenant-scoped.
- Upload:
  - Resumable uploads, client returns uploadId; server enqueues transcode if needed.
- CDN:
  - Store renditions and use signed URLs for private content.
- APIs:
  - POST /bff/products/:id/media (upload metadata)
  - GET /bff/media/:id/url

5) Pricing Rules & Promotions
- Purpose: manage sale pricing, tier pricing, and campaign attachments.
- Implementation:
  - Pricing engine evaluates active promotions and computes final price in BFF.
  - Provide preview in editor and API to attach promotions to products.

6) SEO & Structured Data
- Purpose: optimize product pages for search and social.
- Implementation:
  - Editor allows meta title/description, canonical URL, JSON-LD preview.
  - Server-side rendering (SSR) injects structured data for crawlers.

7) Drafts, Versioning & Audit
- Purpose: authoring workflow with history.
- Implementation:
  - Store draft versions and allow compare/rollback; provide audit log per product.

8) Performance & Caching
- List endpoints should return compact payloads; leverage CDN for media and ISR for storefront pages.

9) Permissions & RBAC
- Roles:
  - product.read, product.write, product.publish, product.import
- Enforcement:
  - Host checks UI-level permissions; BFF enforces on every request.

10) Accessibility
- Use accessible table patterns, keyboard navigation, aria labels for form fields, and ensure forms are screen-reader friendly.

11) Implementation Checklist
1. Create products folder and design tokens for table/list components.
2. Implement BFF list, counts, and import endpoints.
3. Implement editor UI with draft/publish flow and media uploader.
4. Add integration tests and accessibility checks.

Appendix: Sample product list response

```json
{
  "items": [
    { "id":"p-1","sku":"BJ-001","title":"Blue Jacket","price":79.99,"stock":12 }
  ],
  "nextCursor": "c-abc123"
}
```

