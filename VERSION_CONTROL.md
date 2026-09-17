# Version control: Git + GitHub

## Repository rules

- Keep `main` protected and require pull requests, review, and passing CI.
- Use short-lived branches named `feature/`, `fix/`, `chore/`, or `docs/`.
- Keep commits focused and written in the imperative mood.
- Never commit `.env` files, private keys, database dumps, credentials, or production data.
- Add a sanitized `.env.example` with variable names only.
- Review diffs before every commit and enable signed commits where practical.

## Pull request checks

Require type-checking, tests, linting, dependency audit, secret scanning, migration validation, and a production build. Review authorization, data exposure, logging, and migration safety for every backend change.

## GitHub protections

- Use least-privilege `GITHUB_TOKEN` permissions.
- Pin third-party GitHub Actions to trusted commit SHAs where practical.
- Protect environments and require approval for production deployment.
- Store deployment credentials as short-lived OIDC credentials or encrypted repository/environment secrets.
- Enable Dependabot updates, code scanning, secret scanning, and branch protection.
