# Track Order — Features (Detailed)

This document expands Track Order features into implementation steps, APIs, UX details, and data considerations.

1) Public Tracking Form
- Purpose: allow both guests and signed-in customers to track orders.
- Features:
  - Inputs for order number and email/phone used at purchase.
  - Validation and clear error messages for mismatches.
- APIs:
  - GET /bff/track-order?orderNumber=...&contact=...

2) Status & Timeline
- Purpose: show clear, up-to-date status of the order.
- Features:
  - High-level status label and icon.
  - Timeline of events (placed, packed, shipped, out for delivery, delivered, return started).
- APIs:
  - GET /bff/account/orders/:id (or dedicated tracking endpoint) including shipment events.

3) Shipment Details & Carrier Links
- Purpose: provide precise delivery information.
- Features:
  - Shipment carrier, tracking number, and deep link to carrier tracking page.
  - Estimated delivery date/time window; updates as carrier events arrive.

4) Account Integration
- Purpose: encourage account usage while respecting guest flows.
- Features:
  - If user is authenticated, link through to full Order Details in My Orders.
  - Gentle prompts to create an account or sign in to see full history for guests.

5) Implementation Checklist
1. Implement tracking endpoint(s) that validate order number and contact info.
2. Build tracking UI with status, timeline, and shipment details.
3. Integrate with My Orders and Help & Support for escalations.
4. Add analytics for tracking lookups and repeated checks.

