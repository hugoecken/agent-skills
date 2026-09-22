# Native public client

Register a separate public OIDC client. Use authorization code with PKCE and the system browser, with exact redirect registration and verified callback handling. A public client cannot keep a client secret. Do not reuse the web BFF cookie model, inject web tokens into a WebView or embed an authentication browser for convenience.

Use the platform-supported secure storage boundary for retained credentials/tokens under the accepted session design. Do not use ordinary unencrypted preferences, app logs, URLs or a general client-state store as credential storage. Keep provider/client details in configuration and adapters, not feature/domain models.

Own pending auth requests, state/nonce/code-verifier lifecycle and cancellation. Reject mismatched/replayed callbacks and unsafe destinations. Coordinate refresh with one owner, bounded retries and explicit terminal expiration; do not create competing refresh loops. Validate API tokens at the receiving resource boundary and enforce current domain authority there.

Logout removes local authentication state and protected cached data even if provider revocation/logout fails, following the accepted remote revocation policy. Test cancellation, background/resume, supported deep links, expired/rejected tokens, refresh failure, safe storage lifecycle and account switching on the actual platform where needed.
