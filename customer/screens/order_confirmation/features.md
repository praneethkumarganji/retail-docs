# Order Confirmation — Features (Detailed)

This document expands Order Confirmation features into implementation steps, APIs, UX details, and data considerations.

1) Confirmation Summary
- Purpose: reassure customers their order is received and provide a clear reference.
- Features:
  - Prominent confirmation message and order number.
  - Summary of items, prices, discounts, shipping, tax, and total.
  - Store context (for multi-store setups).
- APIs:
  - GET /bff/account/orders/:id (re-used from My Orders).

2) Next Actions
- Purpose: guide customers on what to do next.
- Features:
  - Buttons/links to Track Order and My Orders.
  - Highlight of points earned and link to Loyalty Wallet where applicable.
  - Prompt to continue shopping or explore key categories.

3) Notifications
- Purpose: set expectations for follow-up communication.
- Features:
  - Brief note that an email/SMS confirmation has been sent (with masked contact).
  - Optional "resend confirmation" flow for logged-in users.

4) Help & Policies
- Purpose: make support and policies easy to find at this critical moment.
- Features:
  - Inline links to returns policy, shipping information, and Help & Support.

5) Implementation Checklist
1. Route to Order Confirmation after successful checkout with order id.
2. Fetch and render order summary using existing account order endpoints.
3. Integrate loyalty earn display and deep links to My Orders, Track Order, and Help.
4. Add analytics for confirmation views and next actions taken.

