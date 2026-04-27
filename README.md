# CI/CD Actions

Small reusable GitHub Actions workflows for the `dev-lukas` projects.

Repository-specific tests, smoke checks, integration stacks, and runtime
assertions belong in the caller repository. These workflows only provide the
generic execution primitives:

- `python-checks.yml`: set up Python, install optional requirements, and run
  caller-provided checks.
- `node-checks.yml`: set up Node, install dependencies, and run caller-provided
  typecheck, lint, test, build, and audit commands.
- `ansible-checks.yml`: install Ansible dependencies and run caller-provided
  YAML lint, Ansible lint, and syntax checks.
- `docker-build.yml`: build caller-provided image tags and optionally run
  caller-provided container tests and smoke commands.
- `docker-publish.yml`: log in to a registry and push caller-provided image tags.
- `compose-deploy.yml`: deploy an allowlisted set of Docker Compose services over
  SSH.

## Deploys

`compose-deploy.yml` requires:

- `deploy_services`: whitespace, comma, or newline-separated services to deploy.
- `allowed_services`: whitespace, comma, or newline-separated allowlist.
- `deploy_path`: remote Docker Compose project directory.
- `environment`: GitHub environment name, defaulting to `production`.
- Secrets: `VPS_HOST`, `VPS_USER`, and `VPS_SSH_KEY`.

The deploy job rejects unknown or unsafe service names before opening SSH, then
runs:

```sh
docker compose pull <services>
docker compose up -d --no-deps <services>
```

## Versioning

Call workflows with `@main` while iterating. Once stable, tag releases such as
`v1` and update callers to `@v1`.
