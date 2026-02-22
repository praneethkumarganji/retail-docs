 # Order Details — Features & Implementation

This document describes the Order Details screen flows, APIs, UX notes, and implementation checklist.

Features

- Order header
  - order id, status, totals, customer summary, action buttons (capture, refund, fulfill)

- Timeline
  - payments, refunds, shipments, internal notes, external webhooks
  - filter by type, expand entries for details

- Fulfillment
  - create shipment, select carrier & service, print label, assign tracking
  - integration points: Shippo, EasyPost, carrier APIs

- Payments
  - capture/void/refund, retry failed payments, show payment method & gateway info

- Customer communication
  - send templated email/SMS, record outbound messages, view inbound messages

- Returns & refunds
  - initiate return, return labels, restocking workflows

- Attachments & docs
  - invoices, packing slips, shipping labels (PDFs)

APIs

- GET /bff/orders/:id
- POST /bff/orders/:id/fulfill
- POST /bff/orders/:id/notes
- POST /bff/orders/:id/refund

Permissions & Security

- Require orders.view for read, orders.update for actions, orders.refund for refunds
- Audit log events for sensitive actions

UX & Accessibility

- Keyboard shortcuts for common actions (capture, refund)
- Confirm dialogs for destructive actions, aria-live regions for timeline updates

Implementation checklist

1. Backend endpoints with tenant enforcement
2. Timeline component with virtualized list
3. Integrations for carriers & payment gateways with retry and error handling
4. Attachments storage (signed URLs) and PDF rendering
5. Unit and integration tests, Playwright flows for main actions

