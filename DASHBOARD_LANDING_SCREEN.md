# Dashboard / Landing Screen — Detailed Design

Purpose
- The dashboard is the primary landing screen for tenant admins and operators after login. It surfaces high-value KPIs, actionable items, and quick navigation to plugins (Inventory, Orders, CRM, Social, Analytics).

Audience & Roles
- Tenant Admin: full access to all dashboard tiles and settings.
- Store Manager: limited access (orders, inventory, fulfillment).
- Marketing: access to social & campaign tiles.

Design Goals
- Glanceable: key metrics visible above the fold.
- Actionable: each widget offers a clear next action (view details, resolve, create).
- Extensible: plugin regions allow plugins to register widgets.
- Performant: lazy-load non-critical widgets; cache KPI results.
- Accessible & responsive: keyboard-first, screen-reader friendly, mobile-optimized.

Primary Sections
- Global header: tenant switcher, search, notifications, user menu.
- Left navigation: primary app sections (Dashboard, Products, Orders, CRM, Social, Analytics, Settings).
- Main content:
  - Top KPI bar (sales, orders, conversion, AOV)
  - Activity feed / notifications
  - Widgets grid (cards): Sales chart, Recent orders, Low-stock items, Campaign performance, Tasks
  - Plugin region: area where plugins can add a card or full-width panel
- Right rail (optional): Quick actions, shortcuts, help, chat support.

ASCII Layout (desktop)

```
+----------------------------------------------------------------------------------+
| HEADER: Tenant logo | Search bar | Tenant selector | Notifications | Avatar     |
+----------------------------------------------------------------------------------+
| NAV (left) |                       MAIN (content)                                  |
| ---------- | +------------------------------------------------------------------+  |
| Dashboard  | | KPI BAR: [Sales ▲] [Orders ▼] [Conversion %] [AOV]               |  |
| Products   | |------------------------------------------------------------------|  |
| Orders     | | ROW 1:  | Sales Chart (2/3)             | Recent Orders (1/3)   |  |
| CRM        | |--------+--------------------------------+----------------------+  |
| Social     | | ROW 2:  | Low Stock Items (1/2)         | Campaign Perf (1/2)   |  |
| Analytics  | |--------+--------------------------------+----------------------+  |
| Settings   | | ROW 3:  | Plugin Region (full-width) — plugins mount cards/panels   |  |
|            | |------------------------------------------------------------------|  |
|            | | ACTIVITY FEED (below widgets) — comments, syncs, alerts         |  |
+----------------------------------------------------------------------------------+
| FOOTER (legal / version)                                                         |
+----------------------------------------------------------------------------------+
```

Mobile (stacked)
- Header (compact) → KPI bar (horiz scroll) → Widgets (stacked single-column) → Activity feed → Footer.

Widget Types & Behavior
- KPI Card
  - Data: numeric value, sparkline, delta vs period.
  - Interaction: tap/click opens detail modal or analytics drill-down.
  - Data cadence: short TTL (30s–2m) for near-real-time; fallback cached value on load.
- Chart Card
  - Data: time-series (sales by day), interactive hover, export CSV.
  - Render: lightweight SVG/Canvas; lazy-load heavy charting library after shell load.
- Table Card (Recent orders)
  - Columns: order id, customer, amount, status, action.
  - Interaction: quick actions inline (view, refund, contact customer).
  - Pagination: cursor-based where possible.
- Alert / Low-stock
  - Priority color: red/yellow/neutral; CTA to create purchase order or view product.
- Plugin Card
  - Plugin registers a card via HostSDK (registerWidget) with mount point and metadata.
  - Host enforces permissions and slot placement before calling plugin.mountWidget(hostSdk, container).

Plugin Regions & SDK Hooks
- Regions:
  - widget-grid: grid slots for cards
  - full-width-panel: for dashboards or deep plugin UIs
  - right-rail: compact widgets/actions
- HostSDK widget API (example)

```ts
interface WidgetRegistration {
  id: string;
  title: string;
  size?: 'small'|'medium'|'large'|'full';
  requiredPermissions?: string[];
  mount(container: HTMLElement, hostSdk: HostSDK): Promise<UnmountFn>;
}
// hostSdk.registerWidget(widgetRegistration)
```

Data Sources & Backing APIs
- BFF endpoints (examples):
  - GET /bff/dashboard/kpis?period=24h&tenant=...
  - GET /bff/dashboard/recent-orders?limit=10
  - GET /bff/dashboard/low-stock?threshold=5
  - GET /bff/plugins/registry?tenant=...
- Data flow:
  - Host requests aggregated KPIs from BFF which composes services (orders, inventory, analytics).
  - Plugins request data via hostSdk.api which proxies to BFF.

Performance Recommendations
- SSR shell: render header, nav, and skeletons server-side for FCP; hydrate on client.
- KPI bar: hydrate quickly with small JSON payload from BFF; avoid heavy charts in critical path.
- Lazy-load heavy widgets and third-party libs (charting, mapping) after hydration.
- Use placeholders & skeleton loaders; optimistic UI for quick actions.
- CDN + edge caching for static assets; tenant-aware cache keys for dynamic content.

Telemetry & Events
- Trackable events (suggestion):
  - dashboard.view (tenant, user, timestamp)
  - widget.loaded (widgetId, pluginId?, loadTime)
  - widget.action (widgetId, action, metadata)
  - kpi.refresh (duration, source: socket|poll)
- Error reporting: namespace errors with widget/plugin id for faster triage.

Accessibility
- Keyboard navigation: ensure skip-to-content, focus-visible on cards, and logical tab order.
- Screen reader: aria-labels for KPIs and widget regions; role="region" with aria-roledescription.
- Color contrast: follow WCAG AA; do not rely on color alone for status.
- Responsive: ensure single-column keyboard flow on mobile.

Security & Permissions
- Plugins must declare requiredPermissions and the host must verify them before mounting.
- Sanitize any HTML/content the plugin provides to dashboard panels.
- Limit hostSdk surface for third-party plugins; consider iframe sandbox for untrusted code.

Example React Component Structure (suggested)

```tsx
// apps/host/src/pages/dashboard.tsx
export default function DashboardPage() {
  return (
    <Shell>
      <Sidebar />
      <Header />
      <Main>
        <KpiBar />
        <WidgetGrid />
        <ActivityFeed />
      </Main>
      <RightRail />
    </Shell>
  );
}
```

WidgetGrid internals
- Grid layout using CSS grid with responsive breakpoints.
- Host queries registry for registered widgets and renders mount points where plugins can mount.

Rollout & A/B ideas
- Rollout plugins incrementally per-tenant using feature flags.
- A/B test KPI placements and CTA labels to optimize conversion.

Checklist before launch
1. KPI endpoints instrumented and cached.
2. HostSDK widget registration and permissions enforced.
3. Basic plugin example mounted in widget-grid.
4. Accessibility audit completed for keyboard & screen readers.
5. Performance budget set (LCP, TTFB, hydration).

Navigation
- Screens index: ./screens/index.md
- Dashboard spec: ./screens/dashboard.md
- Products: ./screens/products.md
- Product Details: ./screens/product_details.md
- Orders: ./screens/orders.md
- Inventory: ./screens/inventory.md
- CRM: ./screens/crm.md
- Social: ./screens/social/index.md
- Analytics: ./screens/analytics.md
- Settings: ./screens/settings.md
- Plugin Marketplace: ./screens/plugin_marketplace.md
- Onboarding: ./screens/onboarding.md
- Checkout: ./screens/checkout.md
- Storefront Landing: ./screens/storefront_landing.md
- Storefront Product: ./screens/storefront_product.md

End of document.

