# Database: PostgreSQL

## Data principles

PostgreSQL is the source of truth for accounts, caregiver relationships, reminders, game progress, audit events, and consent records. The browser should only cache non-sensitive UI state.

## Suggested core tables

- `users`: UUID primary key, normalized email, password hash, role, status, timestamps
- `refresh_sessions`: hashed refresh-token identifier, user, device metadata, expiry, revocation time
- `caregiver_links`: explicit user-to-caregiver relationship and permission scope
- `reminders`: owner, schedule, completion state, and timestamps
- `game_progress`: user, game, score/progress, timestamps
- `consents`: user, consent type, version, granted/revoked timestamps
- `audit_events`: actor, action, target, request ID, and timestamp

## Rules

- Use UUID identifiers and UTC `timestamptz` values.
- Store passwords only as slow, salted Argon2id (or bcrypt with a strong cost) hashes.
- Never store raw refresh tokens; store a hash or opaque session identifier.
- Add foreign keys, unique constraints, check constraints, and indexes based on query patterns.
- Use migrations tracked in Git. Never edit production schema manually.
- Use least-privilege database roles: migration role, application role, and read-only reporting role.
- Encrypt connections with TLS and require encrypted backups.
- Use transactions for relationship changes, authentication session rotation, and multi-record updates.
- Treat deletion and retention as product requirements; support account export and deletion workflows.
- Redact or minimize sensitive data in audit events. Do not store passwords or tokens.

## Backups and recovery

Use encrypted, automated backups, test restores regularly, and document a recovery point objective and recovery time objective before production launch.
