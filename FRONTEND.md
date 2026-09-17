# Frontend: React + TypeScript + Tailwind CSS

## Goals

- Preserve the calm, accessible brain-wellness experience in the current static app.
- Use strict TypeScript and typed API clients.
- Keep sensitive information out of browser storage and logs.
- Meet WCAG 2.2 AA expectations: keyboard navigation, visible focus, semantic HTML, reduced motion, sufficient contrast, and screen-reader labels.

## Suggested structure

```text
frontend/
  src/
    app/                 # routing, providers, error boundary
    components/          # reusable presentational components
    features/            # dashboard, games, reminders, caregiver
    lib/                 # API client, query configuration, utilities
    hooks/
    types/
    styles/
```

## Standards

- Enable `strict` TypeScript, no implicit `any`, and exhaustive checks.
- Use React Router for routes and a query/cache library for server state.
- Keep server state separate from local UI state.
- Validate all external API responses at runtime before using them.
- Use Tailwind utility classes with a small shared design system; do not build user-controlled class names.
- Render user content as text by default. Never use `dangerouslySetInnerHTML` for untrusted content.
- Use an error boundary and clear, non-sensitive error messages.
- Use CSRF protection for cookie-authenticated mutations.
- Do not put access tokens, refresh tokens, medical details, or caregiver secrets in localStorage.
- Lazy-load games and non-critical screens; provide loading and offline/error states.

## Testing

- Unit test reducers, validation, and accessible components.
- Test keyboard-only navigation and screen-reader names.
- Add integration tests for login, logout, expired sessions, and authorization failures.
- Run type-checking and production builds in CI.
