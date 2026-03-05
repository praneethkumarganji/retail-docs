# My Account — Features (Detailed)

This document expands My Account features into implementation steps, APIs, UX details, and data considerations.

1) Account Overview
- Purpose: provide a personal landing page for signed-in customers.
- Features:
  - Greeting, quick links to Orders, Loyalty Wallet, Addresses, and Support.
  - Snapshot: recent orders, points balance, key messages.

2) Profile & Contact Details
- Purpose: manage basic identity info.
- Features:
  - Name, email/phone, optional additional contact fields.
  - Verification status indicators where relevant.
- APIs:
  - GET /bff/account/profile
  - PATCH /bff/account/profile

3) Address Book
- Purpose: manage saved addresses for faster checkout.
- Features:
  - List of saved addresses, default shipping/billing markers.
  - Add/edit/delete addresses with validation.
- APIs:
  - GET /bff/account/addresses
  - POST /bff/account/addresses
  - PATCH /bff/account/addresses/:id
  - DELETE /bff/account/addresses/:id

4) Security
- Purpose: give customers control over account security.
- Features:
  - Change password flow.
  - Optional 2FA configuration if supported by platform.
- APIs:
  - POST /bff/account/change-password

5) Preferences & Consents
- Purpose: manage communication and personalization preferences.
- Features:
  - Email/SMS/push subscription toggles.
  - Marketing and tracking consent flags.
- Integration:
  - Sync with CRM consent records.

6) Implementation Checklist
1. Implement account profile, addresses, and security endpoints via BFF.
2. Build Account UI with left nav sections and responsive layout.
3. Integrate with CRM for consents and communication preferences.
4. Add analytics events for account updates and preference changes.

