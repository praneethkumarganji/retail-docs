# Billing & Settlements — Features (Detailed)

This document expands Billing & Settlements features into implementation steps, APIs, UX details, and data considerations.

1) Billing Overview
- Purpose: provide a high-level view of store revenue and cash flows.
- Features:
  - KPIs for selected range: gross revenue, refunds, net, tax collected.
  - Breakdown by channel, payment method, and currency (if applicable).
- APIs:
  - GET /bff/finance/overview?from=...&to=...
- UX:
  - Date range presets (today, yesterday, last 7/30 days, custom).
  - Drill-down links into Invoices and Orders.

2) Invoices
- Purpose: manage invoices issued to end customers.
- Features:
  - Invoice list with filters (date, order, customer, status, channel).
  - Actions: view, download, resend, void (where allowed).
  - Invoice numbering and template configuration (moved to Settings > Taxes/Invoicing if desired).
- APIs:
  - GET /bff/finance/invoices?filters...
  - GET /bff/finance/invoices/:id
- Integration:
  - Deep link to Orders and Returns for the underlying transaction.

3) Settlements / Payouts (Optional)
- Purpose: track payouts from payment providers to the merchant.
- Features:
  - Settlement list with amount, date, status, payment provider.
  - Mapping to underlying transactions and fees.
- APIs:
  - GET /bff/finance/settlements?filters...

4) Gift Cards & Store Credit Visibility
- Purpose: track non-cash balances and their impact on billing.
- Features:
  - Views of gift card issuance, redemptions, and outstanding balances.
  - Store credit adjustments originating from returns or manual grants.
- Integration:
  - Links to Products (gift card definitions) and Returns (store credit resolutions).

5) Tax Visibility
- Purpose: surface tax amounts and patterns without owning full tax configuration.
- Features:
  - Tax collected per period by region and rate.
  - Export view for accounting or tax filing.
- Integration:
  - Detailed tax rule configuration remains under Settings > Taxes.

6) Reports & Exports
- Purpose: support finance and accounting workflows.
- Features:
  - Prebuilt reports (e.g., daily takings, monthly summary, tax summary).
  - CSV/Excel export with configurable columns.
- APIs:
  - GET /bff/finance/reports/:id/export

7) Security, Permissions & Observability
- Roles:
  - finance.read, finance.export
- Enforcement:
  - BFF enforces tenant and role-based access; sensitive data masked for lower-privilege roles.
- Observability:
  - Track export jobs, failed invoice lookups, and settlement sync issues.

8) Implementation Checklist
1. Implement finance data aggregation layer (orders, payments, refunds, taxes).
2. Build Billing & Settlements UI with overview, invoices, and optional settlements.
3. Integrate with Orders, Returns, and payment providers for reconciliation.
4. Add exports and reporting endpoints with background job handling.
5. Add analytics for finance usage and performance.

