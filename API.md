# API: REST

## Conventions

- Base path: `/api/v1`
- JSON request and response bodies with `Content-Type: application/json`
- Resource names are plural nouns and use predictable HTTP methods.
- Every response includes a request ID header: `X-Request-Id`.
- Validate body, query, and path parameters at the boundary.
- Use pagination with bounded `limit` values for collections.
- Use ISO 8601 UTC timestamps and stable UUID identifiers.

## Initial endpoints

| Method | Path | Purpose | Auth |
| --- | --- | --- | --- |
| POST | `/auth/register` | Create an account | Public, rate limited |
| POST | `/auth/login` | Start a session | Public, rate limited |
| POST | `/auth/refresh` | Rotate a refresh session | Refresh cookie |
| POST | `/auth/logout` | Revoke the current session | Authenticated |
| GET | `/me` | Read the current profile | Authenticated |
| GET/PATCH | `/reminders` | List and update reminders | Owner |
| GET/PUT | `/games/:gameId/progress` | Read or save progress | Owner |
| GET/POST | `/caregiver-links` | Manage explicit caregiver access | Owner |

## Error format

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "The request could not be processed.",
    "requestId": "request-id"
  }
}
```

Messages must be safe for users. Do not return SQL errors, stack traces, account-existence clues, or authorization details.

## Security behavior

- Require `Authorization: Bearer <access-token>` for protected endpoints.
- Enforce ownership or caregiver permission at the service layer for every resource.
- Require an `Idempotency-Key` for retryable state-changing operations where duplicate work is harmful.
- Use strict CORS origins, `Cache-Control: no-store` for authenticated responses, and request body limits.
- Version breaking changes and publish an OpenAPI contract generated from validated schemas.
