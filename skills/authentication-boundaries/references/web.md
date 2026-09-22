# Confidential BFF and web session

The BFF owns authorization-code flow with PKCE, exact registered redirect URIs, validated state and nonce, and provider issuer/audience/signature/expiry checks through a supported OIDC library. Keep access, refresh and ID tokens server-side. Never return them to frontend code or put them in browser storage, JavaScript-readable cookies, URLs or logs.

Give the browser only an opaque session identifier. For a production `__Host-` cookie use Secure, HttpOnly, Path=/ and no Domain; use the accepted SameSite policy (Lax for this profile where compatible with the callback). Cookie names and lifetimes remain local configuration. A missing session can be anonymous on public routes and unauthenticated on protected APIs.

After every successful callback establishing, resuming or reauthenticating access, issue a new session ID and invalidate the prior presented ID before completing the authenticated response. After successful recovery revoke all existing application sessions before creating one new session. Do not rotate solely because domain permissions changed; enforce current authority on the next protected operation. Enforce idle/absolute expiry and immediate server-side revocation.

Invalidate the local session and clear its cookie before attempting provider logout. Validate post-login/logout destinations against explicit same-origin paths. Failed provider logout does not restore local access.

Keep CSRF protection on cookie-authenticated state changes. A session-bound CSRF token is sent outside the session cookie. Reject invalid state-changing requests; safe methods do not mutate. Restrict CORS and validate origins; Fetch Metadata is additional defense, not replacement authorization.

Use 401 for unauthenticated and 403 for authenticated without current permission. Protect session/account/workspace/auth/logout/mutation responses with no-store (and private where applicable); never place them in shared public caches. Canonical public HTML stays anonymous in this profile.

Verification covers invalid state/nonce/redirects; new callback session IDs and rejection of old IDs; recovery revocation; expiry; local-first logout; changed domain authority without new authentication; CSRF rejection; 401/403; public/private response separation; and no tokens/secrets in browser data or logs.
