---
name: nextjs-separate-backend
description: "Implement or review Next.js App Router features backed by a separate business service. Covers the public API or BFF boundary, routing, rendering, caching and accessible UI."
---

# Next.js with a separate backend

Read the project's backend boundary, installed Next.js version and affected routes. Consult its design sources for UI changes and its API client configuration for backend integration. Keep publication, access and lifecycle rules with the project.

## Responsibilities

- `app` owns routing, layouts, metadata, loading/error boundaries and route-level composition.
- Product features own screens, feature components, API adaptation, forms, schemas, coherent hooks and meaningful models. Shared UI owns stable domain-neutral primitives.
- Keep browser traffic at the BFF/public API boundary. Never call internal services from the browser.
- Next.js owns rendering; the separate backend owns business behavior. Keep product authorization there, with no direct database access or second business API. Read the project’s accepted identity/session architecture for OAuth, cookies and session ownership rather than inventing a portable authentication profile. Do not add product-mutating Server Actions.
- A technical route handler can serve an explicit non-product need; name that need and preserve the backend boundary. Do not create route handlers merely to proxy an already suitable BFF.

Use narrow client boundaries and the Next router as navigation authority. Read data needed for server-rendered content and metadata through supported Next server mechanisms and the owning backend client. Select static generation, explicit revalidation or request rendering according to accepted freshness and the installed version. A server-only read does not require a client data cache.

Give client-managed remote data one owner. If the project uses TanStack Query, use its supported prefetching and hydration when client interactions benefit from initial server data. Isolate server-side Query clients per request. Keep private data isolated across identities and avoid independently refreshed server and client copies of the same displayed value.

## Public content and caching

Derive canonical content and metadata from the same public source. Whether a public page keeps anonymous HTML independent of a session, and how account controls render, comes from the project's accepted architecture; do not impose one product's publication model on every site. Preserve product-defined visibility, publication and discovery semantics rather than inferring them from route names.

Choose revalidation and invalidation deliberately. Update metadata and content consistently. Never put session, account, workspace, auth or mutation responses in public/shared caches. Apply the pinned Next version's actual cache behavior; do not rely on defaults from another version. Private request state must not enter a shared cached function.

## Features, forms and UI

Follow the application's existing React conventions for feature state, forms, effects and types. Send product mutations through its public backend/BFF client, using the generated client where configured.

Reuse the project's existing UI primitives and semantic tokens where available. If the project's component registry uses `components.json`, take the component base, aliases, icons and styling settings from that file. Preserve accessible HTML, keyboard access, visible focus, readable contrast and touch targets. Cover accepted loading, empty, validation, permission and failure states and implement the approved responsive behavior.

Use the project's existing typecheck and build commands, and its generation command where applicable. Add a library only for a stable owner with actual consumers or a real enforceable boundary. No global barrels, speculative render engines, parallel design system or generated-code edits.

## Evidence

Check route semantics, private/public data separation, hydration, metadata consistency and affected responsive states. Run relevant tests and typechecks, regenerate affected clients where configured, and run the production Next build for routing, rendering or server/client changes. Use a real framework/browser proof for async server rendering, redirects or cache semantics that unit tests cannot reproduce. Inspect the final diff and ignored outputs. Do not claim visual approval from a passing build.

Mechanics: [Next.js data fetching](https://nextjs.org/docs/app/getting-started/fetching-data) and [TanStack Query server rendering](https://tanstack.com/query/latest/docs/framework/react/guides/advanced-ssr). Check the installed versions before choosing cache, prefetch or hydration APIs; these links do not prescribe version-specific defaults. Separate business ownership is a project profile selected by this pack.
