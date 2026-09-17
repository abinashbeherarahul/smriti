# Backend: Node.js + Express

## Suggested structure

```text
backend/
  src/
    app.ts                 # Express app without listening
    server.ts              # process startup and graceful shutdown
    config/                # validated environment configuration
    middleware/            # auth, errors, rate limits, request IDs
    modules/               # feature routes, services, repositories, schemas
    db/                    # pool, migrations, transactions
    observability/         # structured logs and metrics
```

## Runtime requirements

- Use a maintained Node.js LTS release.
- Use TypeScript with strict compiler settings.
- Validate environment variables at startup and fail closed when required values are missing.
- Keep `app` separate from `server` so API tests do not open a port.
- Handle `SIGTERM` and `SIGINT` with graceful HTTP and database shutdown.
- Set secure HTTP headers, a strict CORS allowlist, request size limits, and request timeouts.
- Use parameterized SQL through a trusted PostgreSQL driver or query builder.
- Centralize error handling. Return stable error codes and never expose stack traces or secrets.
- Assign a request ID and emit structured logs without tokens, passwords, or sensitive health data.
- Apply endpoint-specific rate limits, especially to login, refresh, password reset, and messaging.

## Service boundaries

Routes should only parse input and map responses. Business rules belong in services, and database access belongs in repositories. Every authenticated request must check both identity and authorization; authentication alone is not permission.

## Operational endpoints

- `GET /health/live`: process is running; no dependency details.
- `GET /health/ready`: dependency readiness for internal monitoring only.
- `GET /api/v1/me`: authenticated user summary.

Run migrations before marking the service ready. Do not expose database errors to clients.
