---
name: authentication-boundaries
description: "Design or review OIDC, web BFF sessions, native PKCE, identity mapping, CSRF and resource authorization boundaries without moving product authority into the identity provider."
---

# Authentication boundaries

Read the accepted identity architecture, registered clients, session/runtime configuration and product authority. Provider names, hosts, cookie names, lifetimes and account lifecycle decisions belong to the project; do not copy them from another application. This skill does not authorize changing an identity provider or deploying authentication infrastructure.

## Identity and authority

Use standard OIDC integration where supported. Keep credentials, federation, authentication assurance and provider SSO with the identity provider. Map `(issuer, subject)` to a stable application account ID; email is not identity. Product roles, ownership, grants, memberships, entitlements and resource-state rules remain in the owning domain. Do not mirror them into provider roles/organizations or treat token claims as permanently current product authorization.

Extract verified identity at the security boundary and pass application values into use cases. Internal actor propagation requires authenticated service-to-service trust; never trust a browser-supplied actor header. Enforce current resource authorization in the service that owns the resource. Hidden UI and route guards are usability, not enforcement.

## Select the client profile

- For a separate-backend web application, read [web sessions](references/web.md): confidential BFF, server-held tokens and opaque browser cookie.
- For a native application, read [native authentication](references/native.md): separate public client, code flow with PKCE and system browser.

Do not blend web cookie ownership and native token storage. Use established supported libraries; do not implement token cryptography or protocol parsing ad hoc.

## Evidence

Exercise accepted/rejected callbacks, identity mapping, expiry, logout, current authorization, redirect validation and absence of secret leakage. Include CSRF and session rotation/recovery for web, and native redirect/cancellation/storage lifecycle for native. Use controlled provider/session-store integration when real boundary behavior matters. Report missing proof explicitly; no production identities or credential dumps.
