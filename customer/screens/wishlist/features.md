# Wishlist — Features (Detailed)

This document expands Wishlist features into implementation steps, APIs, UX details, and data considerations.

1) Saved Items Management
- Purpose: let customers save and revisit products.
- Features:
  - Add/remove from wishlist from PDP, search, and category.
  - Single default wishlist; optional multiple named lists in future.
- APIs:
  - GET /bff/account/wishlist
  - POST /bff/account/wishlist/items
  - DELETE /bff/account/wishlist/items/:id

2) Wishlist Page
- Purpose: provide a dedicated place to review saved items.
- Features:
  - Grid/list of saved products with image, name, price, stock status, and badges.
  - Actions: add to cart, remove item.

3) Cross-device Persistence
- Purpose: keep wishlist consistent across devices.
- Features:
  - For logged-in users, wishlist tied to account.
  - For guests, optional local storage with merge on login.

4) Guest vs Logged-in Behavior
- Purpose: clarify how wishlists behave for different user states.
- Features:
  - Guests can create a local wishlist that is stored in the browser.
  - On login/registration, guest wishlist is merged into the account wishlist using clear precedence rules.

5) Notifications & Promotions
- Purpose: encourage conversions from wishlist.
- Features:
  - Optional notifications for price drops or back-in-stock.
  - Member-only promotions targeted to wishlist items.

6) Implementation Checklist
1. Implement wishlist endpoints with account scoping and optional guest support.
2. Integrate wishlist toggles into PDP, search, and category product cards.
3. Build wishlist page with add-to-cart and remove actions.
4. Add analytics for wishlist interactions and conversion rates.

