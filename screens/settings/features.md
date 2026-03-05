# Settings — Features (Detailed)

This document expands Settings features into implementation steps, APIs, UX details, and administrative concerns.

1) Tenant Settings & Branding
- Purpose: configure tenant identity, domain mapping, theme tokens, and branding.
- APIs:
  - GET /bff/tenant/settings
  - POST /bff/tenant/settings
- UX:
  - Domain mapping wizard, theme token editor, logo uploader.

2) Billing & Plans (Platform Subscription)
- Purpose: manage the tenant's subscription to the platform (plans, payment method, platform invoices).
- APIs:
  - GET /bff/billing/invoices           # platform invoices only
  - POST /bff/billing/update-method

3) Users & Roles
- Purpose: invite users, assign roles, manage permissions.
- APIs:
  - GET /bff/admin/users
  - POST /bff/admin/users/invite

4) API Keys & Webhooks
- Purpose: issue API keys, manage webhook endpoints, rotate secrets.
- Security:
  - Store keys encrypted, show partial key, require rotation on compromise.

5) Storefront & Channels
- Purpose: configure global storefront and channel behavior that stores can inherit or override.
- Features:
  - Channel definitions (web, app, marketplaces) with default capabilities.
  - Global navigation/menu presets and footer structures.
  - Search and recommendations configuration at a global level.
- APIs:
  - GET /bff/settings/channels
  - POST /bff/settings/channels

6) Taxes Configuration
- Purpose: define tax regions, rates, rules, and profiles used by stores and checkout.
- Features:
  - Tax regions and rates with valid-from/valid-to.
  - Rules per product/category and exemptions.
  - Named tax profiles that stores can attach to.
- APIs:
  - GET /bff/settings/taxes
  - POST /bff/settings/taxes

7) Shipping Configuration
- Purpose: define shipping zones, methods, carrier mappings, and rate rules.
- Features:
  - Zones and methods per region, with base pricing rules.
  - Reusable shipping profiles assignable to stores.
- APIs:
  - GET /bff/settings/shipping
  - POST /bff/settings/shipping

8) Feature Flags & Rollouts
- Purpose: toggle features per-tenant and rollout changes.
- Integrations:
  - Connect to FeatureFlagService; support percentage rollouts and tenant targeting.

9) Security & SSO
- Purpose: configure SSO/OIDC, password policies, session management.
- APIs:
  - POST /bff/admin/sso/configure

10) Audit & Compliance
- Purpose: show audit logs and support data export and retention settings.

11) Implementation Checklist
1. Create settings folder and panels UI.
2. Implement BFF endpoints and admin APIs.
3. Add RBAC checks and audit logging.

