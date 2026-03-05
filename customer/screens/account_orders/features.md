# My Orders — Features (Detailed)

This document expands My Orders features into implementation steps, APIs, UX details, and data considerations.

1) Orders List
- Purpose: give customers a clear history of their purchases.
- Features:
  - List of orders with order number, date, store (if multi-store), total, and status.
  - Filters for time range, status (e.g., processing, shipped, delivered, cancelled).
- APIs:
  - GET /bff/account/orders?filters...

2) Order Details
- Purpose: show all information for a specific order.
- Features:
  - Item list with images, names, variants, prices, quantities, and totals.
  - Shipping address, billing address, payment method summary.
  - Delivery status and tracking information.
- APIs:
  - GET /bff/account/orders/:id

3) Post-purchase Actions
- Purpose: support common follow-up actions.
- Features:
  - Reorder selected items or entire order.
  - Download or view invoice (integrated with Billing & Settlements).
  - Request a return or exchange (integrated with Returns).
  - Contact support with order pre-filled context (integrated with Support).

4) Notifications & Status Updates
- Purpose: keep customers informed about order progress.
- Features:
  - Clear status indicators for key milestones (placed, shipped, out for delivery, delivered).
  - Optional subscription to delivery notifications (email/SMS/push).

5) Guest vs Logged-in Access
- Purpose: support both guest and account-based order access.
- Features:
  - Guest orders accessible via Track Order and/or magic-link flows using order+email/phone.
  - Encouragement to create an account or link past guest orders after authentication.

6) Security & Privacy
- Purpose: protect order data.
- Features:
  - My Orders available only to authenticated users.
  - No exposure of full payment details, only masked summaries.

7) Implementation Checklist
1. Implement account orders endpoints in BFF with tenant/store and auth scoping.
2. Build orders list and detail views with responsive layouts.
3. Integrate with Returns, Billing & Settlements, and Support for actions.
4. Add analytics for order views, reorders, and return requests.

