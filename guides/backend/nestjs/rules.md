# Backend (NestJS and similar)

## Shared in-repo `core/` pattern

- Prefer imports from `core/...` when capability already exists.
- Add reusable cross-cutting behavior in `core/`.
- Keep `src/` focused on app-specific features.

## Layering

- Controllers: thin (validation/auth wiring + delegation).
- Services: business rules/orchestration.
- Data access: repositories/ORM in service/data layers, not controllers.

## Contracts

- Keep DTOs and runtime validation aligned.
- Keep route metadata/name keys aligned with frontend route catalogs.
- Keep response envelopes consistent within each project.
