# Security baseline

Security is a release requirement, not a later enhancement. The application handles account and caregiver data, so the team must threat-model before exposing the API publicly.

## Mandatory controls

- Enforce HTTPS and HSTS in production.
- Use secure headers, strict CORS, CSRF protection for cookie mutations, and content security policy.
- Validate and normalize every external input; use parameterized database queries.
- Apply authentication, authorization, rate limits, timeouts, body limits, and replay protection where appropriate.
- Hash passwords with Argon2id or strong bcrypt settings and use constant-time verification.
- Use short-lived access tokens and rotating, revocable refresh sessions.
- Keep secrets in a managed secret store. Never put secrets in Git, frontend bundles, logs, or error messages.
- Encrypt data in transit and at rest, restrict database access, and test encrypted backup restores.
- Minimize collected data, define retention periods, support export/deletion, and record consent changes.
- Add audit logging for authentication, permission, consent, and sensitive-data events. Protect logs from tampering and redact sensitive values.
- Run dependency, secret, static-analysis, and container/image scans in CI.
- Maintain an incident response plan, vulnerability reporting contact, and key/token rotation procedure.

## Threat-model checklist

Review account takeover, credential stuffing, session theft, CSRF, XSS, SQL injection, IDOR, SSRF, malicious uploads, dependency compromise, insider access, denial of service, and caregiver privilege escalation.

## Security verification

Before production, verify authorization with negative tests, run an API security scan against a staging environment, test refresh-token reuse detection, restore a backup, inspect client bundles for secrets, and complete an independent penetration test for sensitive workflows.
