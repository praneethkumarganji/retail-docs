# Dashboard — Features (Detailed)

This document expands each Dashboard feature into implementation steps, APIs, UX details, data flow, and accessibility requirements.

1) Key Performance Indicator (KPI) Bar
- Purpose: Provide glanceable, up-to-the-minute metrics with drilldowns.
- Metrics: total sales (period), number of orders, conversion rate, average order value, top SKUs, revenue change vs prior period.
- Data cadence:
  - Server-provided snapshot on page load (fast JSON, < 10KB).
  - Real-time deltas via WebSocket/SSE or short polling (configurable).
- APIs:
  - GET /bff/dashboard/kpis?period=24h
  - GET /bff/dashboard/kpis?period=7d
- UX:
  - Each KPI shows value, sparkline (7/30-day), delta, and clickable drilldown.
  - Provide accessible labels (aria) and keyboard focus targets.
- Implementation steps:
  1. BFF aggregates from Orders/Analytics services (ensure tenant filter).
  2. Return typed DTO with metrics and small sparkline data array.
  3. Client renders lightweight numeric card; load sparkline SVG on demand.

2) Widget Grid (Cards)
- Purpose: Flexible, extensible card grid hosting host and plugin-provided widgets.
- Widget sizing: small (1x1), medium (2x1), large (2x2), full-width.
- Registration & lifecycle:
  - hostSdk.registerWidget({ id, title, size, requiredPermissions, mount })
  - mount(container, hostSdk) should return unmount hook.
- APIs:
  - GET /bff/dashboard/widgets?tenant=
  - POST /bff/dashboard/widgets/order (for widget actions e.g., reorder)
- UX:
  - Drag-and-drop to rearrange (persist per-tenant or per-user).
  - Lazy-load widget code and data when in viewport.
- Implementation steps:
  1. Host queries registry and user preferences for widget layout.
  2. Render grid with placeholders; load widget bundles on mount.
  3. Enforce permission checks before mounting plugin widgets.

3) Plugin Region & Plugin Widgets
- Purpose: Allow plugins to extend the Dashboard with cards or full panels.
- Security:
  - Plugins must declare required permissions; host verifies against user token.
  - Manifest must be signed and hostCompatibility-checked.
- Rendering:
  - Plugins mount into container nodes; must not leak global state.
  - Host shares limited SDK: auth, api, telemetry, i18n, theme, permissions, router.
- Implementation steps:
  1. Plugin publishes manifest to registry.
  2. Host fetches manifest, validates, loads remote bundle, calls mount(hostSdk).
  3. Plugin renders UI and uses hostSdk.api for BFF calls.

4) Activity Feed
- Purpose: Present chronological system and user events (orders, social comments, syncs).
- Data model:
  - Event: { id, type, actor, metadata, createdAt, tenantId }
- APIs:
  - GET /bff/dashboard/activity?limit=25&since=
  - POST /bff/dashboard/activity/:id/acknowledge
- UX:
  - Collapsible feed, filters by event type, inline actions (comment, acknowledge).
  - Provide deep links to related screens (order, product, conversation).
- Implementation steps:
  1. BFF composes events from services and event bus.
  2. UI subscribes to WebSocket for live updates; de-duplicate events client-side.

5) Quick Actions & Shortcuts
- Purpose: Provide one-click access to frequent tasks (create product, publish post).
- Implementation:
  - Host provides quick-action registry and permission checks.
  - hostSdk.router.push or open modals for actions.

6) Tenant Selector & Multi-store View
- Purpose: Let organizations with multiple stores switch tenant contexts.
- Security:
  - Validate tenant switch server-side; re-issue tenant-scoped tokens if needed.
- UX:
  - Tenant switcher in header; warn when switching contexts.
- Implementation steps:
  1. Fetch tenant list at login or on-demand.
  2. On switch, refresh minimal shell and re-fetch BFF data.

7) Search (Global)
- Purpose: Search products, orders, customers quickly from the header.
- APIs:
  - GET /bff/search?q=&scope=products,orders,customers&limit=10
- UX:
  - Autocomplete suggestions grouped by type.
  - Keyboard shortcut (/) to focus.

8) Accessibility & Internationalization
- Accessibility:
  - All interactive elements must have keyboard focus and aria-labels.
  - Regions: role="region" + aria-roledescription for KPI bar, widget grid, activity feed.
  - Ensure color contrast and provide non-color indicators for status.
- Internationalization:
  - Host provides i18n context and locale; widgets must supply translation keys.

9) Performance & Caching
- SSR: render header, nav, and skeletons server-side to improve FCP.
- Cache:
  - CDN + edge caching for static assets.
  - BFF should provide cache-control and use tenant-aware cache keys.
- Lazy-loading:
  - Defer loading heavy chart libraries until widget is visible.

10) Telemetry & Observability
- Events:
  - dashboard.view, widget.loaded, kpi.refresh, widget.action
- Traces:
  - Trace BFF requests and plugin mount latency; tag with tenantId and pluginId.

11) Security & RBAC
- Enforcement:
  - Host checks frontend permissions but BFF enforces on every API call.
  - Plugins must declare permissions and be validated by host.

12) Implementation Checklist (step-by-step)
1. Create Dashboard folder and design-system components for KPI cards and widgets.
2. Implement BFF endpoints for KPIs, widgets, and activity feed (tenant-scoped).
3. Implement HostSDK widget registration and plugin sandboxing.
4. Implement dashboard page skeleton SSR and client hydration.
5. Build example host widgets (Sales chart, Recent orders, Low-stock).
6. Add telemetry, accessibility checks, and E2E tests.

Appendix: Sample KPI API response

```json
{
  "tenantId": "t-acme",
  "period": "24h",
  "metrics": {
    "sales": { "value": 12400, "delta": 0.12, "sparkline": [120,300,450,...] },
    "orders": { "value": 128, "delta": -0.05 },
    "conversion": { "value": 0.042, "delta": 0.01 },
    "aov": { "value": 96 }
  }
}
```

