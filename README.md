# CI/CD Actions

Reusable GitHub Actions workflows for the `dev-lukas` projects.

The project repositories keep only thin caller workflows. Common CI/CD behavior
lives here:

- Node static site checks, Docker build, HTTP smoke test, registry push, optional
  Docker Compose deploy.
- Python service checks, Docker build, HTTP smoke test, registry push, optional
  Docker Compose deploy.
- FirePhenix ephemeral stack smoke tests with backend, website, MariaDB, and
  Valkey.
- Ansible syntax and lint checks for infrastructure repositories.

## Versioning

Call workflows with `@main` while iterating. Once stable, tag releases such as
`v1` and update callers to `@v1`.

## Required Secrets

Docker image workflows expect these repository or organization secrets:

- `REGISTRY_PASSWORD`
- `VPS_HOST`
- `VPS_USER`
- `VPS_SSH_KEY`

Deploy steps only use the VPS secrets when the caller passes `deploy: true`.
