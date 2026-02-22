# CRM — Features (Detailed)

This document expands CRM features into implementation steps, APIs, UX details, and data considerations.

1) Customer List & Profile
- Purpose: unified customer profiles with orders, lifetime value, interactions, and segmentation.
- APIs:
  - GET /bff/crm/customers?filters
  - GET /bff/crm/customers/:id
- UX:
  - Search, filters, quick actions (email, SMS, orders).
  - Profile shows orders, segments, notes, consents.

2) Segments & Targeting
- Purpose: create dynamic/static segments for campaigns and analytics.
- Implementation:
  - Dynamic segments based on events (orders, page views) via query builder.
  - Store segment snapshots for export and campaign targeting.

3) Campaign Integration
- Purpose: send to email/SMS/social channels and measure performance.
- APIs:
  - POST /bff/crm/campaigns
  - GET /bff/crm/segments/:id

4) Consent & GDPR Tools
- Purpose: manage consents, data access, erasure requests.
- Implementation:
  - Track consent records, provide export, implement erasure workflow.

5) Integration with Orders & Conversations
- Purpose: link CRM profiles to orders and social conversations for full context.
- Implementation:
  - Conversation and order events include customer identifiers to enrich profiles.

6) Security & Permissions
- Roles: crm.read, crm.write, crm.export
- Enforcement via BFF and host checks.

7) Data Model & Performance
- Use denormalized views for LTV and aggregated metrics; background jobs to maintain aggregates.

8) Implementation Checklist
1. Create crm folder and UI components for list/profile.
2. Implement BFF CRM endpoints and integration with event bus.
3. Add data retention and GDPR controls.

