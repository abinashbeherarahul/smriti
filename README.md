# Smriti full-stack blueprint

Smriti includes a React/Vite browser application and a Node.js/Express backend. The documents in this folder define the production architecture and security requirements for the full-stack application.

## Stack

- [Frontend](./FRONTEND.md): React, TypeScript, and Tailwind CSS
- [Backend](./BACKEND.md): Node.js and Express
- [Database](./DATABASE.md): PostgreSQL
- [API](./API.md): REST
- [Authentication](./AUTH.md): JWT access tokens with rotating refresh sessions
- [Version control](./VERSION_CONTROL.md): Git and GitHub
- [Security](./SECURITY.md): defense-in-depth requirements

## Recommended implementation order

1. Create the React/TypeScript frontend while preserving accessibility and the current user flows.
2. Create the Express API with validation, structured errors, logging, and health checks.
3. Add PostgreSQL migrations and repositories.
4. Add authentication and role-based authorization.
5. Replace localStorage persistence with authenticated API calls.
6. Add CI, dependency scanning, secret scanning, and deployment protections.

The React/Vite application is the active frontend. The backend requires PostgreSQL and uses the following commands from the project root:

```text
copy .env.example .env
npm install
npm run backend:migrate
npm run backend
```

Run `npm run dev` in a second terminal for the Vite frontend. The API listens on `http://localhost:4000` by default.
