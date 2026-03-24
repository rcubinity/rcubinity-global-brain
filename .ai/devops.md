# DevOps and delivery

## Containers

- Prefer **multi-stage** Docker builds; minimal runtime images.
- **Do not run** application processes as **root** in containers unless the platform requires it and it is documented.

## Configuration

- **Environment variables** for environment-specific values; never commit secrets.
- Separate **build-time** vs **runtime** config clearly.

## CI/CD

- **Lint + test + build** (as applicable) on merge requests; fail fast on main branch protections.
- Version **artifacts** and **lockfiles** reproducibly.

## Observability

- Health checks for orchestrators (`/health` or equivalent) where services are exposed to load balancers.
