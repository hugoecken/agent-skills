---
name: nextjs-separate-backend
description: "Implement or review Next.js App Router web features with a separate business backend and BFF, including rendering, caching, routing, shadcn and semantic styling."
---

# Next.js with a separate backend

This skill selects a backend-separated web architecture. It is not a universal prescription for every Next.js application. Read the repository's accepted architecture, product/design sources, Next version, app routes, generator configuration and `components.json`. Keep product-specific publication, access and lifecycle rules local.

## Responsibilities

- `app` owns routing, layouts, metadata, loading/error boundaries and route-level composition.
- Product features own screens, feature components, API adaptation, forms, schemas, coherent hooks and meaningful models. Shared UI owns stable domain-neutral primitives.
- Keep browser traffic at the BFF/public API boundary. Never call internal services from the browser.
- Next.js owns rendering; the separate backend owns business behavior. Keep product authorization there, with no direct database access or second business API. Read the project’s accepted identity/session architecture for OAuth, cookies and session ownership rather than inventing a portable authentication profile. Do not add product-mutating Server Actions.
- A technical route handler can serve an explicit non-product need; name that need and preserve the backend boundary. Do not create route handlers merely to proxy an already suitable BFF.

Use narrow client boundaries. Render public content and SEO metadata on the server through static generation, explicit revalidation or request rendering according to freshness. Build connected workspaces with client React and TanStack Query when that matches their interaction; do not force their product state through Server Components. Use the Next router as the navigation authority.

## Public content and caching

Derive canonical content and metadata from the same public source. Whether a public page keeps anonymous HTML independent of a session, and how account controls render, comes from the project's accepted architecture; do not impose one product's publication model on every site. Preserve product-defined visibility, publication and discovery semantics rather than inferring them from route names.

Choose revalidation and invalidation deliberately. Update metadata and content consistently. Never put session, account, workspace, auth or mutation responses in public/shared caches. Apply the pinned Next version's actual cache behavior; do not rely on defaults from another version. Private request state must not enter a shared cached function.

## Features, forms and UI

Keep feature state, forms, effects and type ownership consistent with the shared React conventions. At the Next boundary, product mutations call the generated public backend/BFF client; do not route them through an extra application API in Next. A platform-only change does not require rereading unrelated form or state policies.

Use approved library components and shadcn primitives. `components.json` owns aliases, base, icon family and Tailwind configuration. Style with semantic tokens, preserve accessible HTML, keyboard access, visible focus, readable contrast and touch targets. Cover accepted loading, empty, validation, permission and failure states; implement responsive changes from the design authority.

Use existing workspace targets for generation/typechecks/builds. Add a library only for a stable owner with actual consumers or a real enforceable boundary. No global barrels, speculative render engines, parallel design system or generated-code edits.

## Evidence

Check route semantics, private/public data separation, hydration, metadata consistency and affected responsive states. Run generation/typecheck when boundaries change, relevant tests and the production Next build for routing, rendering or server/client changes. Use a real framework/browser proof for async server rendering, redirects or cache semantics that unit tests cannot reproduce. Inspect the final diff and ignored outputs. Do not claim visual approval from a passing build.

Mechanics: [Next.js App Router documentation](https://nextjs.org/docs/app). Separate business ownership is a project profile selected by this pack.
