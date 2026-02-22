 # Storefront Product — Features & Implementation

This document describes the Storefront Product page flows, SEO, performance, and implementation notes.

Features

- Media gallery & previews
  - zoom, video, 360-preview, lazy load

- Variant selection & availability
  - show stock per location, estimated delivery, disable purchasable options

- Buy box & cart actions
  - add to cart, quantity, wishlist, share, prefetch add-to-cart endpoints

- SEO & performance
  - SSR + ISR for cacheability, structured data (JSON-LD), preconnect to CDN

APIs

- GET /bff/storefront/product/:slug
- POST /bff/cart/add

Implementation checklist

1. Implement SSR with ISR and caching headers
2. Build accessible media gallery and variant selector
3. Prefetch and optimize add-to-cart flow
4. Tests: SEO snapshot, E2E purchase flow

