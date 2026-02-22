# Frontend Architecture & Implementation Guide

Generated from Omni-Channel Retail Platform architecture. This document captures the complete frontend architecture, plugin model, developer workflows, testing, security, deployment, and a phased implementation plan.

--- 

## Table of Contents
- Overview
- High-level Architecture
- Technology Stack
- Monorepo Layout
- Plugin Model & Runtime Contract
- Host Responsibilities & SDK
- Routing, Data, and Styling Strategies
- Security & Tenant Isolation
- Versioning, Publishing & Compatibility
- Development Workflow & DX
- Testing Strategy
- CI/CD and Release Flow
- Observability & Error Handling
- Accessibility, i18n & Theming
- Rollout Roadmap & Milestones
- Risks & Mitigations
- Quick Start Checklist

---

## Overview

The frontend is a pluggable, extensible shell (host) built with Next.js + React + TypeScript that dynamically loads self-contained feature plugins (micro-frontends). The goals are: scalability, tenant isolation, strong developer experience, and a secure plugin ecosystem (marketplace-ready).

## High-level Architecture

- Host (Next.js shell)
  - SSR/ISR for public storefront pages for SEO
  - CSR for admin consoles and plugin UIs
  - Provides global concerns: auth, routing, design system, telemetry, plugin loader
- Plugin runtime
  - Micro-frontends (Module Federation or dynamic imports)
  - Each plugin bundles UI, routes, and optional backend adapters
- BFF / API Gateway
  - Single entrypoint for frontend calls; enforces tenant scoping & aggregates backend services
- Shared packages
  - Design system, shared-api, plugin-sdk, shared-auth, types
- CDN, cache, observability pipeline (RUM, logs, traces)

## Technology Stack (recommended)

- Framework: React 18 + TypeScript
- Meta-framework: Next.js (SSR/ISR)
- Bundler & MFE: Webpack Module Federation (or Vite + federation plugin)
- Monorepo tooling: Turborepo or pnpm workspaces
- State: React Query (server state); Zustand or Redux Toolkit (client state)
- Styling: Tailwind CSS + design-system components
- Auth: OIDC PKCE (Keycloak)
- Testing: Jest, Testing Library, Playwright, Chromatic/Percy
- Observability: Sentry (errors), OpenTelemetry/Datadog (traces & metrics)

## Monorepo Layout (recommended)

```
/apps
  /host                # Next.js shell (admin + plugin host)
  /storefront          # Optional dedicated storefront app
/packages
  /design-system
  /plugin-sdk
  /shared-api
  /shared-auth
  /plugins/*           # Example: inventory, orders, crm, social
/scripts
  plugin-generator.js
```

Keep CI pipelines per package with workspace-aware build caching via Turborepo.

## Plugin Model & Runtime Contract

Plugin manifest (manifest.json):

```json
{
  "id": "inventory",
  "version": "0.1.0",
  "hostCompatibility": ">=1.0.0 <2.0.0",
  "entryUrl": "https://plugins.mycdn.com/inventory/remoteEntry.js",
  "routes": ["/admin/inventory"],
  "permissions": ["inventory.read","inventory.write"],
  "signature": "hmac:..."
}
```

Plugin TypeScript interface (example):

```ts
interface Plugin {
  id: string;
  version: string;
  routes: string[];
  permissions: string[];
  featureFlags?: string[];
  mount(hostSdk: HostSDK): Promise<UnmountFn>;
}
```

Lifecycle:
- Discover → Validate → Load → Mount → Unmount

Security validations before mount:
- Manifest signature & checksum
- hostCompatibility semver check
- Required permissions vs tenant role claims

## Host Responsibilities & SDK

Host exposes a minimal, controlled HostSDK to plugins. Only surface the APIs necessary for plugin functionality:

- auth
  - getUser(): User
  - getToken(): Promise<string>
- router
  - push(path: string), replace(), registerRoute(meta)
- api
  - request<T>(options): Promise<T> (calls BFF)
- telemetry
  - track(event)
- i18n
  - t(key, vars)
- theme
  - getTokens(), onThemeChange(cb)
- permissions
  - has(permission: string): boolean

Example mount usage:

```ts
export async function mount(hostSdk) {
  const root = document.createElement('div');
  document.getElementById('plugin-root')!.appendChild(root);
  ReactDOM.render(<App sdk={hostSdk}/>, root);
  return () => ReactDOM.unmountComponentAtNode(root);
}
```

## Routing, Data & Styling

Routing:
- Host maintains canonical routing (Next.js). Plugins register routes via SDK on load.
- Host enforces RBAC & tenant checks before allowing route navigation.
- For storefront pages, use Next.js pages with server rendering. Admin plugin routes can be client-rendered.

Data:
- All plugin API calls go through the BFF (shared-api package) which enforces tenant scoping.
- Use React Query for caching & deduplication. Share query keys conventions across host & plugins.
- For real-time flows, use WebSockets or SSE proxied through BFF where appropriate.

Styling:
- Use a shared design-system with tokens (Tailwind config + component library).
- Plugins should prefer component tokens; if full isolation is required, use CSS Modules or Shadow DOM wrapper.
- Mark common libraries as shared in Module Federation to avoid multiple React copies.

## Security & Tenant Isolation

Plugin sandboxing:
- Validate and sign plugin artifacts prior to publishing.
- Treat third-party plugins as untrusted: restrict SDK surface, optionally use iframe sandbox for 3rd-party code.
- Enforce CSP headers and strict Content-Security-Policy.

Permissions:
- Plugins declare required permissions in manifest; host checks permission claims from tokens.
- For sensitive operations, BFF performs server-side permission checks.

Tenant separation:
- BFF enforces tenant_id on requests. Host attaches tenant context using secure tokens; never rely on client input for tenant scoping.

## Versioning, Publishing & Compatibility

Registry:
- Simple registry: static host (S3/CloudFront) for remoteEntry and manifest, plus an index.json for discovery.
- Prefer immutably-published artifacts with HMAC or GPG signatures.

Compatibility:
- Host declares hostCompatibility range in manifest consumers.
- Maintain stable plugin-sdk with clear deprecation policy and migration guides.

Publishing CI:
- Lint, unit tests, build artifact, sign, publish to registry.
- Optionally run integration smoke tests against a staging host.

## Development Workflow & Developer Experience

Scaffolding:
- Provide a plugin-generator CLI that scaffolds package, manifest, mount boilerplate, Storybook stories, and tests.

Local dev:
- Host runs with Module Federation remotes pointing to local plugin dev servers.
- HMR for rapid iteration; Storybook for isolated component dev.

Developer tools:
- ESLint, Prettier, TypeScript strict mode, commit lint.
- Storybook + Chromatic for visual reviews.

## Testing Strategy

Unit:
- Jest + Testing Library for components and hooks.

Integration:
- Mount plugin with a stubbed HostSDK in RTL to verify lifecycle & interactions.

E2E:
- Playwright tests that run host with a set of real-built plugins to verify flows (onboarding, product create → social publish → order).

Visual:
- Chromatic/Percy for design-system and plugin UI regression.

Contract:
- Schema validation for manifest.json and mount contract tests (host & plugin integration).

## CI/CD & Release Flow

Host:
- CI: lint → test → build → SSR checks → performance budgets.
- Deploy: Canary → Progressive rollout → Production.
- Host can optionally poll registry for plugin updates or use webhook triggers.

Plugin:
- CI: lint → test → build → sign → publish.
- Deploy: publish artifact to registry; optionally trigger host staging smoke tests.

Feature flags & rollouts:
- Use feature flagging service to toggle plugin features per tenant and to perform gradual rollouts.

## Observability & Error Handling

- Instrument host & plugins with RUM (LCP, FID, CLS) and custom telemetry.
- Capture errors per-plugin namespace in Sentry; include plugin id/version in events.
- Trace BFF + backend calls using OpenTelemetry; ensure plugin load failures are traced and alerted.
- Monitoring alerts for plugin load time, injection errors, and runtime exceptions.

## Accessibility, i18n & Theming

Accessibility:
- Design system enforces WCAG 2.1 AA as the baseline.
- Plugins must include accessibility tests (axe) and keyboard navigation checks.

i18n:
- Host provides i18n context and locale. Plugins register translation keys and supply translation files.
- Support RTL locales and date/number localization via host-provided utilities.

Theming:
- Host exposes theme tokens. Tenants can override tokens; plugins must read tokens rather than hardcode colors/spacing.

## Rollout Roadmap & Milestones

Phase 0 — Preparation (2–3 weeks)
- Initialize monorepo, CI templates, design-system skeleton, minimal plugin-sdk, sample plugin scaffold.

Phase 1 — Core Shell + Reference Plugin (6–10 weeks)
- Build Next.js host shell, plugin loader (Module Federation), design-system, shared-api, BFF stub, and Inventory plugin reference.

Phase 2 — Admin UX & Core Plugins (8–12 weeks)
- Implement Orders, CRM, Social plugins; plugin registry; signed publishing; Keycloak integration; developer UX improvements.

Phase 3 — Marketplace & Scale (12+ weeks)
- Marketplace UI, billing for paid plugins, plugin lifecycle management, AI features & heavy-worker integrations, multi-region optimizations.

## Risks & Mitigations

- Shared-lib compatibility:
  - Mitigation: strict semver, peer-deps, compatibility shims, migration guides.
- Malicious or buggy plugins:
  - Mitigation: signatures, permission model, optional iframe sandbox, monitoring and rapid revoke.
- Performance impact from many plugins:
  - Mitigation: on-demand loading, resource quotas, lazy loading, performance budgets.

## Quick Start Checklist (first 2 weeks)

1. Initialize monorepo (Turborepo/pnpm).
2. Create Next.js host app with minimal BFF.
3. Create design-system package + Storybook.
4. Implement plugin-sdk (auth, router, api, telemetry).
5. Scaffold example plugin (inventory) and validate mount flow.
6. Add CI skeleton for lint/test/build/publish.

---
## Host SDK, Mount Examples & Sequence

Here's a concise HostSDK spec, plugin mount/unmount example, host loader pseudocode, and a small sequence flow.

HostSDK (TypeScript):

```ts
export interface HostSDK {
  auth: {
    getToken(): Promise<string>;
    getUser(): User;
  };
  router: {
    push(path: string): void;
    registerRoute(meta: RouteMeta): void;
  };
  api: {
    request<T>(opts: RequestOptions): Promise<T>;
  };
  telemetry: {
    track(event: string, props?: Record<string, any>): void;
  };
  i18n: {
    t(key: string, vars?: Record<string, any>): string;
  };
  theme: {
    getTokens(): Record<string, string>;
    onThemeChange(cb: (tokens: Record<string, string>) => void): () => void;
  };
  permissions: {
    has(permission: string): boolean;
  };
}
```

Plugin mount / unmount example:

```ts
// plugin-entry.ts
import React from "react";
import ReactDOM from "react-dom";
import { HostSDK } from "./host-sdk";

export async function mount(hostSdk: HostSDK): Promise<() => void> {
  const root = document.createElement("div");
  root.id = `plugin-root-${Math.random().toString(36).slice(2, 8)}`;
  const container = document.getElementById("plugins-container") || document.body;
  container.appendChild(root);

  ReactDOM.render(<PluginApp sdk={hostSdk} />, root);

  // return cleanup/unmount function
  return () => {
    // plugin must remove listeners/timers it created as well
    ReactDOM.unmountComponentAtNode(root);
    root.remove();
  };
}
```

Host-side loader (minimal pseudocode):

```ts
async function loadAndMountPlugin(manifestUrl: string, hostSdk: HostSDK) {
  const manifest = await fetch(manifestUrl).then(r => r.json());
  validateSignatureAndCompatibility(manifest);

  // dynamic import of remote bundle (manifest.entryUrl)
  const remote = await import(/* webpackIgnore: true */ manifest.entryUrl);
  // convention: remote exposes `mount`
  const unmount = await remote.mount(hostSdk);

  // store unmount for later (e.g., on route change / plugin removal)
  pluginRegistry[manifest.id] = { manifest, unmount };
}
```

Small sequence flow:

```
[User] -> [Host]: navigate /admin/inventory
[Host] -> [Registry]: fetch manifest.json
[Registry] -> [Host]: manifest (entryUrl, perms, signature)
[Host]: validate signature & permissions
[Host] -> [Plugin Bundle]: fetch entryUrl
[Host] -> [Plugin]: call mount(hostSdk)
[Plugin] -> [BFF]: api.request(...)  // tenant-scoped data
[Plugin] -> [Host]: telemetry.track(...)
```

Notes / best practices:
- Plugin must return an unmount function and clean up listeners/timers.
- Validate manifest signature + hostCompatibility before loading.
- Use host.api for all server calls so tenant & permission checks run server-side.
- Share core libs (React, design-system, react-query) to avoid bundle duplication.
- For 3rd-party plugins, consider iframe sandboxing and stricter SDK surface.

## Benefits of the plugin-based approach

- Independent deploys: teams ship features as autonomous plugins with separate CI/CD.
- Incremental upgrades: host and plugins can evolve independently within compatibility ranges.
- Faster developer DX: smaller focused repos, Storybook-driven development, HMR for plugins.
- Scalability: load only required plugins per tenant/route, reducing initial bundle sizes.
- Multi-tenant customization: tenant-specific plugin enablement and per-tenant feature flagging.
- Marketplace readiness: plugins are packaged and versioned for a plugin marketplace model.
- Fault isolation: buggy plugin failures can be sandboxed/unloaded without bringing down host.
- Security & governance: signed artifacts and limited HostSDK surface reduce attack surface.

---
## Tenant example — diagram & end-to-end examples

Below are a compact sequence diagram and runnable examples showing how tenant context is resolved, enforced, and used across Host → Plugin → BFF → Services.

Sequence diagram (text):

```
Browser                          Host (Next.js)                Plugin Registry
   |                                  |                                |
   | GET https://acme.myretail.com/    |                                |
   |---------------------------------> |                                |
   |                                  | resolve tenant for domain      |
   |                                  | tenantId = t-acme              |
   |                                  |                                |
   |                                  | fetch product via BFF          |
   |                                  |-------------------------------->|
   |                                  |                                |
   |                                  |        manifest discovery      |
   |                                  |<-------------------------------|
   |                                  |                                |
   | <--- rendered HTML (tenant t-acme) |                                |

Admin Flow (plugin load)
Browser -> Host: navigate /admin/inventory
Host -> Auth: validate JWT (contains tenant_id = t-acme)
Host -> Tenant Service: is inventory enabled for t-acme?
Host -> Registry: fetch manifest (if enabled)
Host -> Plugin Bundle: fetch entryUrl
Host -> Plugin: mount(hostSdk)
Plugin -> BFF: api.request('/inventory/items')  // Authorization: Bearer <JWT>
BFF -> validate token, extract tenant_id = t-acme, enforce permissions
BFF -> DB: SELECT * FROM inventory_items WHERE tenant_id = 't-acme'
BFF -> Plugin -> returns tenant-scoped data

Example 1 — Next.js server-side rendering (getServerSideProps)

```ts
// pages/product/[slug].tsx
export async function getServerSideProps({ req, params }) {
  const domain = req.headers.host; // acme.myretail.com
  const tenant = await fetchTenantForDomain(domain); // internal Tenant Service
  const product = await fetch(`https://bff.internal/storefront/product/${params.slug}`, {
    headers: { 'x-tenant-id': tenant.id } // or use service auth token
  }).then(r => r.json());

  return { props: { product, tenant } };
}
```

Example 2 — curl to BFF from plugin (client includes JWT)

```bash
# Plugin frontend calls BFF to list items
curl -H "Authorization: Bearer $JWT" \
     "https://api.myretail.com/bff/inventory/items"
```

Example 3 — Minimal Express BFF that enforces tenant from JWT

```js
// bff/index.js (minimal)
import express from 'express';
import jwt from 'jsonwebtoken';
import { queryDb } from './db';

const app = express();
const JWT_PUBLIC_KEY = process.env.JWT_PUBLIC_KEY;

function authMiddleware(req, res, next) {
  const auth = req.headers.authorization?.split(' ')[1];
  if (!auth) return res.status(401).send('Missing token');
  try {
    const payload = jwt.verify(auth, JWT_PUBLIC_KEY);
    req.user = payload; // payload.tenant_id expected
    next();
  } catch (e) {
    return res.status(401).send('Invalid token');
  }
}

app.use('/inventory', authMiddleware, async (req, res) => {
  const tenantId = req.user.tenant_id;
  // server-side enforcement: always include tenant filter
  const items = await queryDb('SELECT id,name,qty FROM inventory_items WHERE tenant_id = $1', [tenantId]);
  res.json({ items });
});

app.listen(8080);
```

Cache key note (ISR/CDN):

```
cacheKey = `tenant:${tenantId}:page:${pageType}:${resourceId}`
```
Ensure CDN and edge caches vary by tenant (domain or key) to avoid cross-tenant leaks.

Security reminders:
- Never trust client-submitted tenant_id. Derive tenant from domain mapping or validated JWT.
- Plugins must call BFF (not direct services) so server-side tenant enforcement always occurs.
- Validate plugin manifests and signatures before loading.

---

## Appendices

A. Example plugin manifest schema (JSON Schema)

B. Plugin SDK surface (TypeScript definitions)

C. Example mount/unmount snippets and host route registration examples

--- 

End of document.

