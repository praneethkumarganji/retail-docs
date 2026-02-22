# Checkout — Features (Detailed)

This document expands Checkout features into implementation steps, APIs, UX details, and security considerations.

1) Cart & Review
- Purpose: present cart items, quantities, discounts, shipping estimate, taxes.
- APIs:
  - GET /bff/cart
  - POST /bff/cart/update
- UX:
  - Editable quantities, coupon entry, shipping estimator, cost breakdown.

2) Checkout Flow & Payment
- Purpose: complete order with payment and confirmation.
- Payment methods:
  - Card (Stripe), wallets, UPI, BNPL (configurable per tenant).
- APIs:
  - POST /bff/checkout/confirm
  - POST /bff/payments/authorize
  - Webhooks for payment updates
- Security:
  - PCI compliance via payment provider; never store raw card data.

3) Shipping & Rates
- Purpose: calculate shipping options, support pickup, rate shopping.
- APIs:
  - GET /bff/shipping/rates?address=
  - POST /bff/fulfillment/create_shipment

4) Taxes & Compliance
- Purpose: calculate taxes per jurisdiction; support tenant tax settings.
- Implementation:
  - Use external tax provider or built-in rules; cache rates for performance.

5) Guest Checkout & Accounts
- Support guest checkout with optional account creation on success.

6) Admin Order Creation & Preview
- Admin can create orders via admin UI with same checkout flow; preview storefront receipt.

7) Failure Handling & Idempotency
- Use idempotency keys for payment requests; surface meaningful errors to UI.

8) Accessibility & Localization
- Forms must be keyboard-accessible and localized for currency, locale, and RTL support.

9) Implementation Checklist
1. Create checkout folder and design components for forms and summaries.
2. Integrate payment providers and webhooks.
3. Add E2E tests for payment and confirmation flows.

