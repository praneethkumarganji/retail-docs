# Stores — Features (Detailed)

This document expands multi-store and multi-brand features into implementation steps, APIs, UX details, and data considerations.

1) Stores Registry
- Purpose: define and manage all stores within a tenant.
- Features:
  - Stores list with name, code, brand, domain(s), default locale, currency, status.
  - Create, edit, archive stores with validation (unique codes/domains).
- APIs:
  - GET /bff/stores
  - GET /bff/stores/:id
  - POST /bff/stores
  - PATCH /bff/stores/:id
- Implementation:
  - Each store has a stable store_id and belongs to a tenant_id.

2) Domains & Routing
- Purpose: map hostnames to the correct tenant and store.
- Features:
  - Configure primary and alternate domains per store.
  - Preview and test routing (resolve host to tenant/store).
- APIs:
  - GET /bff/stores/:id/domains
  - POST /bff/stores/:id/domains
- Implementation:
  - BFF and edge layer use domain mapping to derive tenant_id and store_id for requests.

3) Branding & Theme
- Purpose: control brand-specific look & feel per store.
- Features:
  - Logo, color palette, typography, layout variant per store.
  - Links into theme tokens used by storefront and Content.
- Integration:
  - Settings > Tenant branding provides global defaults; Stores override per store.

4) Locales & Currency
- Purpose: set supported languages and currencies per store.
- Features:
  - Default locale and currency, plus additional supported locales.
  - Fallback rules for content and pricing when locale is missing.
- APIs:
  - GET /bff/stores/:id/locales
  - POST /bff/stores/:id/locales

5) Catalog & Pricing per Store
- Purpose: control which products and prices are available in each store.
- Features:
  - Link to product visibility and price lists scoped by store_id.
  - High-level controls: "Store uses price list X", "Hidden collections".
- Integration:
  - Products screen uses store context and store-product mapping for visibility and pricing.

6) Checkout, Payments, Shipping & Taxes per Store
- Purpose: configure transactional behavior for each store.
- Features:
  - Which payment methods are enabled per store.
  - Shipping zones/methods mapping for the store’s regions.
  - Tax profile selection (from Settings > Taxes).
- Implementation:
  - Checkout and BFF always operate with tenant_id + store_id context.

7) Programs & Promotions per Store
- Purpose: align loyalty and promotions with specific stores/brands.
- Features:
  - Flags showing which loyalty programs and promotions are active for each store.
  - Quick links to Loyalty and Promotions filtered to the selected store.

8) Analytics & Reporting by Store
- Purpose: support store-level performance tracking.
- Features:
  - Ensure orders, revenue, returns, and support tickets carry store_id.
  - Analytics and Billing & Settlements can filter and group by store.

9) Security, Permissions & Observability
- Roles:
  - stores.read, stores.write, stores.admin
- Enforcement:
  - BFF enforces tenant and store scoping; only admins can create/retire stores.
- Observability:
  - Audit log for store configuration changes, domain changes, and routing issues.

10) Implementation Checklist
1. Introduce store_id in the core data model and BFF context (tenant_id + store_id).
2. Implement store registry, domains, and routing resolution.
3. Integrate store context into Products, Content, Promotions, Checkout, Loyalty, and Analytics.
4. Update admin UI with a store selector and store-aware filters.
5. Add tests for multi-store routing, configuration overrides, and reporting.

