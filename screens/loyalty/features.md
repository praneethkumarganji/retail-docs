# Loyalty — Features (Detailed)

This document expands Loyalty features into implementation steps, APIs, UX details, and data considerations.

1) Programs & Configurations
- Purpose: define one or more loyalty programs per tenant.
- Features:
  - Program list with status, member counts, and key metrics.
  - Program types: points-based, tiered, subscription-based memberships.
- APIs:
  - GET /bff/loyalty/programs
  - POST /bff/loyalty/programs
  - PATCH /bff/loyalty/programs/:id
- UX:
  - Clear display of eligibility rules, earning rules, and benefits per program.

2) Earning Rules
- Purpose: control how customers earn points or benefits.
- Features:
  - Rules based on order value, products/categories, channels, and behaviors (sign-up, referrals).
  - Multipliers by tier or campaign.
- APIs:
  - GET /bff/loyalty/programs/:id/rules
  - POST /bff/loyalty/programs/:id/rules
- Implementation:
  - Evaluated at order placement and other key events via the BFF.

3) Redemption & Benefits
- Purpose: control how members redeem points and access benefits.
- Features:
  - Configure redemption options (discounts, free shipping, gifts, vouchers).
  - Minimum balances, step sizes, and caps.
- APIs:
  - POST /bff/loyalty/redeem
- Integration:
  - Hooks into Checkout and Promotions for applying loyalty discounts.
  - Optionally issue store credit or gift-card-like vouchers as redemption outcomes.

4) Tiers & Memberships
- Purpose: support tiered loyalty (e.g., Silver/Gold/Platinum) and memberships.
- Features:
  - Tier definitions with thresholds, perks, and expiry rules.
  - Automatic upgrades/downgrades based on activity.
- APIs:
  - GET /bff/loyalty/tiers
  - POST /bff/loyalty/tiers

5) Members & Activity
- Purpose: view and manage individual members.
- Features:
  - Member search with filters (tier, points, activity, segment).
  - Member profile: points balance, history, benefits, linked CRM profile.
- APIs:
  - GET /bff/loyalty/members?filters...
  - GET /bff/loyalty/members/:id
- UX:
  - Inline actions for manual adjustments (with audit), issuing rewards, and fixing edge cases.

6) Expiry & Notifications
- Purpose: handle expiry of points and benefits and communicate proactively.
- Features:
  - Configurable expiry policies and grace periods.
  - Notification hooks (email/SMS/push) before major expiries.

7) Analytics & Reporting
- Purpose: understand loyalty program performance.
- Features:
  - Metrics: enrollment, active members, redemption rates, incremental revenue, churn impact.
  - Deep links into Analytics for segments and funnels by loyalty membership.

8) Security, Permissions & Observability
- Roles:
  - loyalty.read, loyalty.write, loyalty.admin
- Enforcement:
  - BFF enforces tenant scopes and roles.
- Observability:
  - Track rule evaluations, redemptions, and manual adjustments; surface anomalies.

9) Implementation Checklist
1. Implement loyalty data model (programs, tiers, rules, members, ledgers).
2. Build admin UI for programs, earning/redemption rules, and member profiles.
3. Integrate with Orders, Checkout, Promotions, and CRM.
4. Implement reporting in Analytics for loyalty KPIs.
5. Add tests for earning, redemption, and expiry workflows.

