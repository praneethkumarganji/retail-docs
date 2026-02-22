# Analytics — Features (Detailed)

This document expands Analytics features into implementation steps, APIs, UX details, and data considerations.

1) Pre-built Reports & Exports
- Purpose: provide common business reports (sales, returns, channel performance).
- APIs:
  - GET /bff/analytics/reports?reportId=&params
  - POST /bff/analytics/export (background job)

2) Funnel Builder & Cohort Analysis
- Purpose: create and analyze funnels and cohorts to identify drop-offs and lifetime value.
- Implementation:
  - Event storage and aggregation pipelines; provide UI for step definitions and time windows.

3) Custom Dashboards & Widgets
- Purpose: compose dashboards from widget library (host + plugins).
- Implementation:
  - Widget SDK and dashboard persistence per-tenant.

4) Attribution & Revenue Linking
- Purpose: link social and channel activity to orders via UTM, coupon codes, and deep links.
- Data pipeline:
  - Event bus ingestion → metrics service → aggregated datasets.

5) Data Freshness & Retention
- Purpose: balance near-real-time vs batched pipelines; provide freshness indicators.
- Implementation:
  - Streaming pipeline for recent events; batch re-processing for historical updates.

6) Security & Access Control
- Roles: analytics.view, analytics.manage
- Enforcement: BFF and dataset-level access controls.

7) Implementation Checklist
1. Create analytics folder with index and features docs.
2. Implement report endpoints and streaming ingestion.
3. Build dashboards and widget SDK support.

