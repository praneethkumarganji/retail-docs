 # Product Details — Features & Implementation

This document describes the Product Details editor flows, APIs, UX notes, and implementation checklist.

Features

- Header & actions
  - product title, SKU, status, publish button, save draft

- Media gallery
  - upload, alt-text, primary image selection, CDN upload monitoring, image transformations

- Pricing & inventory
  - base price, compare-at price, sale schedules, multi-currency preview, stock per location

- Variants & attributes
  - add/remove variants, attribute mapping, SKU generation

- Descriptions & SEO
  - WYSIWYG + markdown, structured data (JSON-LD), social preview

- Activity log & preview
  - change history, who changed what, preview on tenant domain with draft token

APIs

- GET /bff/products/:id
- PATCH /bff/products/:id
- POST /bff/products/:id/media
- POST /bff/products/:id/variants

Permissions & Security

- product.view for read, product.write for edits, product.publish for publishing

UX & Accessibility

- Keyboard shortcuts for save/publish, accessible forms and labels, image alt-text requirement

Implementation checklist

1. Build editor panels and tabs (details, variants, pricing, SEO)
2. Media uploader with chunked uploads and signed URLs
3. Variant management UI and SKU mapping
4. Preview integration with storefront (draft token)
5. Tests: unit, integration, Playwright for main flows

