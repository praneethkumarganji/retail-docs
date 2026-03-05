 # Dashboard — Admin Landing (Overview)

This folder contains the Dashboard screen specification and detailed feature documentation.

ASCII Layout (detailed)

```
+-----------------------------------------------------------------------------------------------+
| HEADER: [Logo]  [Search ▾]  [Tenant: Acme ▼]  [Quick Actions: +Create]  [Notifications 🔔] [User] |
+-----------------------------------------------------------------------------------------------+
| NAV (left)                                | MAIN: Top Area (Key Performance Indicators + Quick Actions)               |
| ----------------------------------------- | -------------------------------------------------- |
| [Dashboard]                               | Key Performance Indicator bar: [Sales $12.4k ▲] [Orders 128 ▼] [Conversion 4.2%]|
| [Products]                                |         [Average Order Value $96]  [Top SKU: Blue Jacket]           |
| [Orders]                                  | Quick Actions: [Create Product] [Create Campaign]   |
| [Inventory]                               | Date/Period selector | timezone | refresh | export   |
| [CRM]                                     | -------------------------------------------------- |
| [Promotions]                              | ROW 1 (Widgets):                                   |
| [Social]                                  |  ┌──────────────────────┬─────────────────────────┐ |
| [Analytics]                               |  │ Sales Chart (2/3)    │ Recent Orders (1/3)     │ |
| [Settings]                                |  │ Low-stock     │ Campaign Performance           │ |
|                                           |  │ - period selector     │ - list, inline actions  │ |
|                                           |  └──────────────────────┴─────────────────────────┘ |
|                                           | -------------------------------------------------- |
|                                           | ROW 2 (Widgets):                                   |
|                                           |  ┌───────────────┬────────────────────────────────┐ |
|                                           |  │ Low-stock     │ Campaign Performance           │ |
|                                           |  │ Alerts (1/2)  │ - impressions, clicks, conversions   │ |
|                                           |  └───────────────┴────────────────────────────────┘ |
|                                           | -------------------------------------------------- |
|                                           | ROW 3 (Plugin Region - full-width):               |
|                                           |  ┌────────────────────────────────────────────────┐ |
|                                           |  │ Plugin Panel: promotions / custom dashboard     │ |
|                                           |  │ (plugins mount cards or full-width components)  │ |
|                                           |  └────────────────────────────────────────────────┘ |
|                                           | -------------------------------------------------- |
|                                           | ACTIVITY FEED (chronological, collapsible)       |
|                                           | - system events, social comments, sync statuses   |
|                                           | -------------------------------------------------- |
|                                           | FOOTER (tenant info, version)                     |
+-----------------------------------------------------------------------------------------------+
| RIGHT RAIL: Shortcuts | Support Chat | Saved Views | Help Articles | System Status          |
+-----------------------------------------------------------------------------------------------+
```

Shortcuts

:- [Full features doc](./features.md)
:- [Screens index](../index.md)

Navigation (clickable)

:- [Screens index](../index.md)
:- [Products](../products/index.md)
:- [Product Details](../product_details.md)
:- [Orders](./orders.md)
:- [Order Details](./order_details.md)
:- [Inventory](./inventory/index.md)
:- [CRM](./crm/index.md)
:- [Social](../social/index.md)
:- [Analytics](../analytics/index.md)
:- [Settings](./settings/index.md)
:- [Plugin Marketplace](../plugin_marketplace.md)
:- [Onboarding](../onboarding.md)
:- [Checkout](../checkout/index.md)
:- [Storefront Landing](../storefront_landing.md)
:- [Storefront Product](../storefront_product.md)

 