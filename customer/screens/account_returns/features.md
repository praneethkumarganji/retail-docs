# My Returns — Features (Detailed)

This document expands customer returns and exchanges features into implementation steps, APIs, UX details, and data considerations.

1) Returns List
- Purpose: provide visibility into all return and exchange requests.
- Features:
  - List of RMAs with RMA number, related order, date, store, status, and type.
  - Filters for status (requested, approved, in transit, received, completed).
- APIs:
  - GET /bff/account/returns?filters...

2) Initiate Return/Exchange
- Purpose: let customers start a return or exchange from eligible orders.
- Entry points:
  - From My Orders (per-order "Request return / exchange" action).
  - From My Returns screen ("Start new return" linked to order selector).
- Features:
  - Item selection with per-item quantity.
  - Reason codes and optional comments; optional photo uploads.
  - Choice of resolution (refund, exchange, store credit) according to store policy.
- APIs:
  - POST /bff/account/returns (creates RMA request).

3) Status & Instructions
- Purpose: keep customers informed and guide them through the process.
- Features:
  - Timeline or status indicators for each return (requested → approved/denied → in transit → received → resolved).
  - Clear instructions: label download, drop-off locations, pickup scheduling where available.
- APIs:
  - GET /bff/account/returns/:id

4) Policy Integration
- Purpose: ensure returns respect store-specific rules and are clearly communicated.
- Features:
  - Eligibility checks based on order date, items, and store return policies.
  - Inline policy summaries and links to full Returns & Refunds policy.

5) Support Escalation
- Purpose: provide a path when something goes wrong.
- Features:
  - Option to contact support from a return detail view, with context pre-filled.
- Integration:
  - Opens a Support ticket linked to the RMA and original order.

6) Implementation Checklist
1. Implement account returns endpoints and link to the admin Returns screen (RMA lifecycle).
2. Build My Returns list and detail views with responsive design.
3. Add initiation flows from My Orders and My Returns.
4. Integrate policy checks, instructions, and support escalation paths.

