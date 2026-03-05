# Inventory — Features (Detailed)

This document expands Inventory features into implementation steps, APIs, UX details, and operational notes.

1) Inventory List & Location Views
- Purpose: view stock per SKU and per location with filters.
- Filters: location, category, low-stock threshold, vendor.
- APIs:
  - GET /bff/inventory/items?location=
  - GET /bff/inventory/summary?location=
- UX:
  - Table with column grouping per location, quick adjust inline, export.

2) Adjustments & Audit Log
- Purpose: manual adjustments with audit trail for compliance.
- APIs:
  - POST /bff/inventory/adjust
  - GET /bff/inventory/adjustments?sku=
- Implementation:
  - Record who, why, delta, and resulting stock; support rollback.

3) Transfers & Stock Movements
- Purpose: transfer stock between locations and track transit.
- APIs:
  - POST /bff/inventory/transfer
  - GET /bff/inventory/transfers?status=
- UX:
  - Create transfer wizard, track ETA, receive confirmation.

4) Reorder Suggestions & Purchase Orders
- Purpose: suggest POs based on sales velocity and safety stock.
- Implementation:
  - Compute reorder points using sales forecasts; surface suggested PO lines.
  - Integrate with supplier and PO creation flows.

5) Stocktakes & Cycle Counts
- Purpose: periodic count workflows with reconciliation and variance reports.
- APIs:
  - POST /bff/inventory/stocktake (start)
  - POST /bff/inventory/stocktake/:id/submit

6) Integrations & Receiving
- Purpose: link with suppliers, PO, receiving workflows, and ERP systems.
- Implementation:
  - Webhooks for PO receipts, barcode scanning integration.

7) Store Pickup & BOPIS Support
- Purpose: support inventory views and operations for buy-online-pickup-in-store (BOPIS).
- Features:
  - Flag locations as pickup-eligible and expose pickup stock availability.
  - Integration with Orders fulfillment flows for pickup-ready and picked-up statuses.
- Integration:
  - Stores screen defines which locations offer pickup for each store.
  - Checkout uses pickup availability when presenting delivery options.

8) Performance & Data Model
- Use efficient indexes on (tenant_id, sku, location_id); paginate and cache read-heavy endpoints.

9) Permissions & RBAC
- Roles: inventory.read, inventory.write, inventory.transfer
- Enforcement: BFF validates tenant scope and permissions.

10) Observability & Alerts
- Monitor sync failures, low-stock trends, and transfer exceptions.

11) Implementation Checklist
1. Create inventory folder with index and features files.
2. Implement APIs and background workers for transfers and reorder suggestions.
3. Build UI components (table, transfer wizard, stocktake flows).
4. Add tests and monitoring dashboards.

