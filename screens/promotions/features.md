# Promotions — Features (Detailed)

This document expands Promotions features into implementation steps, APIs, UX details, and data considerations.

1) Promotions List & Management
- Purpose: centralize all sales, discounts, and campaigns across channels.
- Features:
  - List view of all promotions with filters (status, type, channel, date range).
  - Quick stats: redemptions, attributed revenue, margin impact.
- APIs:
  - GET /bff/promotions?filters...
  - GET /bff/promotions/:id
  - POST /bff/promotions
  - PATCH /bff/promotions/:id
- UX:
  - Server-side pagination, saved filters, pinned “important” promotions.
  - Badges for scheduled/active/expired, channel icons.

2) Promotion Types & Rules
- Purpose: support a wide variety of campaign structures.
- Types:
  - Product-level: percentage/amount off, scheduled sale price.
  - Cart-level: spend X get Y off, free shipping thresholds.
  - BOGO/bundles: buy N get M free/discounted, bundle pricing.
  - Segment-based: promos limited to specific CRM segments.
  - Channel-based: web/app/social-only promotions.
- Implementation:
  - Rule builder UI with conditions (segment, channel, order total, product/category).
  - Priority and stacking model (which promotions can combine).
- APIs:
  - POST /bff/promotions/evaluate (for BFF pricing engine).

3) Coupons & Campaigns
- Purpose: manage coupon codes and their constraints.
- Features:
  - Single-code and bulk-code campaigns.
  - Constraints: min order value, eligible products/categories, channel, segment.
  - Usage limits: per-code, per-customer, global caps, validity windows.
- APIs:
  - GET /bff/coupons?filters...
  - POST /bff/coupons
  - POST /bff/coupons/bulk-generate
- UX:
  - Show usage counters (used / total), last redemption time.
  - Export codes for external channels.

4) Targeting & Segmentation
- Purpose: align promos with CRM segments and analytics.
- Implementation:
  - Reference CRM segments by id, with preview of segment size.
  - Support geo, device, channel, new/returning customer flags.
- APIs:
  - GET /bff/crm/segments (for selector)

5) Analytics & Attribution
- Purpose: close the loop on promo performance.
- Features:
  - Metrics: revenue, margin, average order value uplift, return rate per promo.
  - Deep links into Analytics dashboards filtered by promotion/coupon.
- Implementation:
  - Ensure orders carry promotionId/couponCode for attribution.

6) Security & Permissions
- Roles:
  - promotions.read, promotions.write, promotions.publish
- Enforcement:
  - Host enforces UI visibility; BFF enforces per-tenant permissions.

7) Observability & Auditing
- Purpose: track changes and troubleshoot mis-applied discounts.
- Implementation:
  - Audit log of promotion changes (who edited what, when).
  - Telemetry around promotion evaluation failures and overrides.

8) Implementation Checklist
1. Create promotions UI (list, editor, rule builder, coupon tab).
2. Implement BFF endpoints for promotions and coupons with tenant scoping.
3. Integrate pricing engine evaluation into Products and Checkout flows.
4. Wire Analytics to use promotion and coupon identifiers.
5. Add E2E tests for promo application and stacking rules.

