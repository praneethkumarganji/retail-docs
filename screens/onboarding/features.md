 # Onboarding & Tenant Setup — Features & Implementation

This document describes detailed onboarding flows, APIs, UX, and implementation notes.

Features

- Stepper flow
  - Account: email, password, organization name
  - Plan selection: free/trial/paid, billing integration
  - Domain mapping: subdomain or custom domain, DNS verification
  - Branding: logo, color tokens, theme selection
  - Seed data: optional demo products, sample orders, sample content
  - Invite teammates: roles and permissions (Admin, Manager, Viewer)

- Health checks & diagnostics
  - DNS check, webhooks test, API connectivity

- Enable core plugins
  - Inventory, Orders, Storefront, Social — toggle per-tenant

APIs

- POST /bff/tenant/create — create tenant + owner user
- POST /bff/tenant/configure — domain, theme, plugins
- POST /bff/tenant/seed — seed demo data
- GET /bff/tenant/health — run checks (dns, webhook)

Security & Permissions

- Restrict onboarding.manage to authorized system users and first-time tenant admins
- Validate domain ownership via tokenized DNS records / HTTP verification

UX Notes

- Allow skipping seed data for experienced users
- Show clear progress and ability to resume
- Provide sample data preview before seeding

Observability & Telemetry

- Track onboarding steps completion, failure reasons, time to complete
- Log domain verification status and webhook test results

Implementation checklist

1. Implement backend endpoints with tenant isolation
2. Build stepper UI with validation and resumable state
3. Integrate billing provider & webhooks
4. Seed demo data job with idempotency
5. Add telemetry events and health dashboard

