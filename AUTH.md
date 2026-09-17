# Authentication: JWT access tokens and sessions

## Recommended flow

1. Register with a normalized email and a password that meets a strong policy.
2. Verify the email before enabling sensitive account operations.
3. On login, issue a short-lived JWT access token and a long-lived opaque refresh token.
4. Store the refresh token in a `Secure`, `HttpOnly`, `SameSite=Lax` cookie scoped to the API.
5. Store only a hash of the refresh token in PostgreSQL.
6. Rotate the refresh token on every refresh. Reuse detection revokes the session family.
7. Keep access tokens in memory in the frontend; never store them in localStorage.
8. Logout revokes the current refresh session and clears the cookie.

## JWT requirements

- Use an asymmetric signing key (for example, RS256 or EdDSA) held outside source control.
- Validate signature, issuer, audience, subject, and expiration.
- Pin the accepted algorithm; never trust the token header to select one.
- Include only a stable user ID, session ID, issued-at time, and expiry.
- Rotate signing keys with a key ID and retain old public keys only for the required overlap period.

## Authorization

Use deny-by-default role and resource policies. A caregiver must have an explicit, revocable link and minimum permissions. Always check resource ownership server-side; never trust a user ID supplied by the client.

## Account protections

- Generic login and reset responses prevent account enumeration.
- Rate limit and progressively delay repeated failures.
- Require recent authentication for password, email, consent, and caregiver-permission changes.
- Provide session listing and revocation for users.
- Never log passwords, tokens, authorization headers, reset links, or sensitive profile data.
