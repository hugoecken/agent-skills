---
name: nextjs-separate-backend
description: "Implement or review Next.js App Router features backed by a separate business API, including rendering, caching, routing and server/client boundaries."
---

# Next.js with a separate backend

Use this skill when the project has selected a separate business backend. Check the Next.js version, routes, backend API and the project's rendering and access requirements.

## Responsibilities

- `app` owns routing, layouts, metadata, loading/error boundaries and route-level composition.
- Product features own screens, feature components, API adaptation, forms, schemas, coherent hooks and meaningful models. Shared UI owns stable domain-neutral primitives.
- Keep browser traffic at the backend's public API or the project's existing BFF. Never call internal services from the browser.
- Next.js owns rendering; the separate backend owns business behavior. Keep product authorization there, with no direct database access or second business API. Read the project’s accepted identity/session architecture for OAuth, cookies and session ownership rather than inventing a portable authentication profile. Do not add product-mutating Server Actions.
- A technical route handler can serve an explicit non-product need; name that need and preserve the backend boundary. Do not create route handlers merely to proxy an already suitable BFF.

Use narrow client boundaries and the Next router as navigation authority. Read data needed for server-rendered content and metadata through supported Next server mechanisms and the backend API. Select static generation, explicit revalidation or request rendering according to accepted freshness and the installed version.

Keep purely server-rendered reads on the server. When a client interaction refreshes data, use the feature's existing data-access path. If the project hydrates a client cache with private server data, scope it to the request. Give each displayed value one owner for subsequent refreshes.

## Public content and caching

Derive canonical content and metadata from the same public source. Whether a public page keeps anonymous HTML independent of a session, and how account controls render, comes from the project's accepted architecture; do not impose one product's publication model on every site. Preserve product-defined visibility, publication and discovery semantics rather than inferring them from route names.

Choose revalidation and invalidation deliberately. Update metadata and content consistently. Never put session, account, workspace, auth or mutation responses in public/shared caches. Apply the pinned Next version's actual cache behavior; do not rely on defaults from another version. Private request state must not enter a shared cached function.

## Features, forms and UI

Follow the project's React conventions for state, forms, effects and types. Submit product mutations to the backend through the project's API integration.

Reuse the project's components and styles. Preserve accessible HTML, keyboard access, visible focus, readable contrast and touch targets. Implement the required loading, empty, validation, permission and failure states and responsive layouts.

## Evidence

Check route semantics, private/public data separation, hydration, metadata consistency and affected responsive states. Run the relevant project checks and the production Next build for routing, rendering or server/client changes. Exercise the actual Next runtime for redirects, async server rendering or cache semantics. Inspect the affected layouts in the browser.

Check the installed version before choosing cache or prefetch APIs. See [Next.js data fetching](https://nextjs.org/docs/app/getting-started/fetching-data).
