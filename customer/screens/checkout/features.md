# Checkout — Features (Detailed)

This document expands customer Checkout features into implementation steps, APIs, UX details, and data considerations.

1) Stepper & Flow Control
- Purpose: guide customers through a clear, linear flow.
- Features:
  - Steps: Address → Delivery → Payment → Review/Confirmation.
  - Visual stepper with ability to go back and edit previous steps.

2) Address & Contact
- Purpose: collect necessary information for delivery and communication.
- Features:
  - Guest checkout (email + shipping address) and logged-in prefill.
  - Address validation and auto-complete where available.
- APIs:
  - GET /bff/checkout (prefill)
  - POST /bff/checkout/address

3) Delivery Options
- Purpose: present shipping choices and expectations.
- Features:
  - Shipping methods per store and region with prices and estimated delivery dates.
  - Icons/labels for express, eco, pickup-in-store if supported.
- APIs:
  - GET /bff/checkout/shipping-options
  - POST /bff/checkout/shipping-selection

4) Payment
- Purpose: securely capture payment details.
- Features:
  - Payment methods (cards, wallets, local methods) as configured per store.
  - Saved payment methods for logged-in users where PCI compliance allows.
  - Clear error states and retry paths.
- APIs:
  - POST /bff/checkout/payment-intent
  - POST /bff/checkout/confirm-payment

5) Order Summary & Promotions
- Purpose: keep pricing transparent and reinforce value.
- Features:
  - Always-visible summary with items, discounts, shipping, tax, and total.
  - Coupon and loyalty redemption review (from Cart or in Checkout).
  - Clear final confirmation of applied discounts and points used/earned.

6) Review & Place Order
- Purpose: final confirmation step.
- Features:
  - Compact summary of address, delivery method, payment method, and items.
  - Legal copy (terms, privacy, returns) with links.
  - Single primary action button (“Place order”, “Pay now”).

7) Error Handling & Recovery
- Purpose: handle failures without losing customer trust.
- Features:
  - Graceful handling of payment failures with guidance (try another method, contact support).
  - Idempotent order submission to avoid duplicates.

8) Notifications & Communication
- Purpose: set expectations for transactional messages.
- Features:
  - Confirm that an order confirmation email/SMS will be sent upon success.
  - Optional checkboxes for marketing messages kept clearly separate from transactional communications.

9) Security, Performance & Accessibility
- Purpose: build trust and ensure broad usability.
- Implementation:
  - Strict TLS, secure iframes or hosted fields for payment.
  - Fast responses, minimal blocking scripts, skeletons for summary updates.
  - Keyboard-friendly forms, proper labels, readable validation messages.

10) Implementation Checklist
1. Implement BFF checkout endpoints with tenant/store and cart integration.
2. Build stepper UI for address, delivery, payment, and review with validation.
3. Integrate payment providers and handle common failure scenarios.
4. Hook confirmation into Orders creation and redirect to order confirmation view.

