# Cart — Features (Detailed)

This document expands Cart (basket) features into implementation steps, APIs, UX details, and data considerations.

1) Cart Line Items
- Purpose: show all items the customer intends to purchase.
- Features:
  - For each item: image, name, variant details, unit price, quantity, line total.
  - Ability to change quantity, remove item, or move to wishlist.
- APIs:
  - GET /bff/cart
  - POST /bff/cart/items
  - PATCH /bff/cart/items/:id
  - DELETE /bff/cart/items/:id

2) Pricing & Discounts
- Purpose: make pricing transparent and trustworthy.
- Features:
  - Subtotal, discounts (promotions and coupons), estimated tax, shipping estimate.
  - Clear "You save X" messaging where appropriate.
- Implementation:
  - Uses the same pricing engine as admin (Promotions, Loyalty, price lists).

3) Coupons & Loyalty
- Purpose: support discounting and loyalty redemption.
- Features:
  - Coupon/promo code input with immediate validation feedback.
  - Optional loyalty section: show available points and allow redemption (with constraints).
- APIs:
  - POST /bff/cart/apply-coupon
  - POST /bff/cart/apply-loyalty

4) Shipping Estimates
- Purpose: reduce surprises at checkout.
- Features:
  - Optional shipping estimator based on location (country/postcode).
  - Display of delivery time windows or badges (e.g., Next-day available).

5) Upsells & Cross-sells
- Purpose: increase basket value and discovery.
- Features:
  - Recommended add-ons or complementary products under or beside the cart.
- APIs:
  - GET /bff/cart/recommendations

6) Empty Cart & Edge States
- Purpose: handle no-items scenario gracefully.
- Features:
  - Friendly empty state with link back to Home and popular categories.
  - Persistence of cart across sessions where supported.

7) Guest vs Logged-in Behavior & Cross-device Persistence
- Purpose: define how carts behave for guests and signed-in customers.
- Features:
  - Guest carts stored locally; on login, merge guest cart into account cart with clear rules.
  - Logged-in carts persisted across devices and sessions.

8) Performance & Accessibility
- Purpose: keep cart interactions fast and inclusive.
- Implementation:
  - Optimistic UI for quantity changes and removals; rollback on failure.
  - Proper table/list semantics, focus management, and screen-reader labels.

9) Implementation Checklist
1. Implement cart BFF API with tenant/store scoping.
2. Build cart UI with line items, summary sidebar, coupons, and loyalty hooks.
3. Integrate recommendations and shipping estimator where applicable.
4. Add analytics events for cart updates and progression to checkout.

