# Backend (NestJS and similar)

Applies to NestJS (and similar layered HTTP services).

## Shared in-repo `core/` (NestJS pattern)

Many Rcubinity Nest apps keep a **colocated `core/`** directory (path-mapped as `"core/*"`) with decorators, response helpers (`success` / `failure`), JWT/guards, notifications, sockets, and other cross-cutting modules.

- **Prefer importing** from `core/...` whenever the capability already exists there; **do not** fork duplicate helpers or HTTP decorators in `src/`.
- **Add** new shared behavior **inside `core/`** when it is org-wide; reserve `src/` for app-specific features.

## Layering

- **Modular** structure: feature modules with clear boundaries.
- **Controllers**: thin — validation wiring, auth guards, delegation to services; avoid large business logic blocks in handlers.
- **Services**: business rules, orchestration, transactions.
- **Data access**: repositories / ORM layer; keep persistence details out of controllers.
- Use **dependency injection** consistently; avoid static singletons for request-scoped data.

## Validation and contracts

- **Validate inputs** with the stack’s chosen approach (e.g. `class-validator` DTOs, **Joi**, Zod). Keep **DTOs and runtime validation** aligned where both exist.
- Prefer **consistent response shapes** (e.g. `{ data, message }` / `{ error }`) as defined in the project — do not invent a new envelope per endpoint.

## Routing and API documentation

- If the org uses a **route catalog** shared with frontends, **handler metadata** (path, method, route name/id) must stay **in sync** with that catalog so clients can call by **name**, not guessed URLs.
- If the project uses **custom decorators** for HTTP metadata, follow existing files in the repo; do not mix plain `@Controller` style and custom style in the same module without a reason.

## Cross-cutting

- **Auth**: apply guards at controller or method level as the project does (JWT, roles, permissions).
- **Errors**: return typed failures or use exception filters as established; avoid leaking stack traces to clients in production.
- **Side effects**: document DB writes, queues, and external calls in code comments or **colocated context docs** (see `context-and-docs.md`).

## Optional: real-time

- For WebSockets/gateways, follow the project’s **namespace and auth** patterns; keep transport concerns separate from domain logic where the codebase already does.
