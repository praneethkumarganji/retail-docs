# Loyalty Wallet — Features (Detailed)

This document expands Loyalty Wallet features into implementation steps, APIs, UX details, and data considerations.

1) Summary Card
- Purpose: quickly show the value of the loyalty program to the customer.
- Features:
  - Current tier, points balance, and near-term expiries.
  - Key benefits of current tier (e.g., free shipping, birthday reward).
- APIs:
  - GET /bff/account/loyalty/summary

2) Earning Rules (Customer View)
- Purpose: explain how to earn points and level up.
- Features:
  - Simple list of earning actions (spend, signup, referrals, campaigns).
  - Link to full program terms (from Loyalty admin config).

3) Redemption Options
- Purpose: show how points can be used.
- Features:
  - Available redemption options (e.g., voucher, percent off, free shipping).
  - Indicate minimum balances and steps (e.g., redeem in blocks of 100).
- APIs:
  - GET /bff/account/loyalty/redemptions

4) Activity History
- Purpose: provide transparency and build trust.
- Features:
  - Ledger of points earned and spent with date, description, and running balance.
- APIs:
  - GET /bff/account/loyalty/activity?filters...

5) Offers & Member-Only Promotions
- Purpose: increase engagement and reward loyalty tiers.
- Features:
  - List of active member-only offers, personalized where available.
  - Clear indication of which offers are tied to tier vs general loyalty.

6) Integration with Checkout & Cart
- Purpose: connect loyalty to actual purchases.
- Features:
  - Ability to redeem points in cart/checkout.
  - Post-purchase confirmation of points earned on order confirmation and My Orders.

7) Implementation Checklist
1. Integrate Loyalty Wallet UI with Loyalty program BFF endpoints.
2. Build summary, earning, redemption, and activity views with responsive design.
3. Wire offers to Promotions and program tiers.
4. Add analytics for loyalty views and redemption usage.

