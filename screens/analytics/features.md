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

5) Billing, Revenue & Margin Reports
- Purpose: provide store- and channel-level financial analytics.
- Features:
  - Reports for revenue, refunds, net, tax collected, and margin.
  - Dimensions: time, store, channel, payment method, promotion, loyalty program, price list.
- Integration:
  - Uses the same underlying data as Billing & Settlements, but read-only and dashboard-oriented.

6) Risk & Trust Analytics
- Purpose: monitor fraud and risk patterns across stores and channels.
- Features:
  - Metrics for fraud/chargeback rates, held vs approved orders, and review outcomes.
  - Breakdowns by store, payment method, geography, and device/channel.

7) Data Freshness & Retention
- Purpose: balance near-real-time vs batched pipelines; provide freshness indicators.
- Implementation:
  - Streaming pipeline for recent events; batch re-processing for historical updates.

8) Security & Access Control
- Roles: analytics.view, analytics.manage
- Enforcement: BFF and dataset-level access controls.

9) Implementation Checklist
1. Create analytics folder with index and features docs.
2. Implement report endpoints and streaming ingestion.
3. Build dashboards and widget SDK support.

