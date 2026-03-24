# DevOps and delivery

- Prefer multi-stage container builds and non-root runtime.
- Use env vars for runtime config; never commit secrets.
- CI should run lint/test/build gates for protected branches.
- Keep lockfiles/artifacts reproducible.
- Add health checks for deploy targets.
