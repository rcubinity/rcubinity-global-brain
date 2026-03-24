# Frontend (Angular and similar SPAs)

Applies to Angular (and analogous) single-page apps that talk to a typed backend contract.

## Architecture

- Prefer **standalone components** when the project already uses them; match existing module/standalone style in the repo.
- Prefer **signals** (or the project’s established state pattern) over ad-hoc subscription chains in components.
- **Smart vs presentational**: keep templates thin; put orchestration and HTTP in **services**, not in components.
- Use **lazy loading** for feature areas when the app’s routing style supports it.
- **Responsive, mobile-first** layouts; respect **accessibility** (labels, focus, keyboard).

## API and data access (critical)

- **Single source of truth for HTTP routes**: a generated or hand-maintained **route catalog** (JSON/YAML) or OpenAPI-derived map — **do not hardcode URLs or HTTP methods** in components when the project uses named routes.
- Call the backend through a **central HTTP/API helper** (e.g. wrapper around `HttpClient`) using **route name keys** that match the server, not raw paths.
- **Typical flow**: build request object → set payload → subscribe to the request’s result stream **before** executing the request if the stack requires ordered subscription (follow the project’s existing pattern exactly).
- **File uploads**: use the stack’s pattern (`multipart`, `FormData`, interceptors) as already implemented in the repo.

## Models

- Map API payloads to **typed models** (classes or interfaces) with factory methods like `fromJson` / `createFrom` if that is the project convention.
- Keep **IDs and optional fields** aligned with backend DTOs.

## UI / UX

- Follow the design system already in the repo (e.g. Material, Bootstrap, or custom tokens); **do not introduce a second competing system** in the same feature without an explicit migration.
- Use **loading and error states** for async operations; avoid silent failures.

## When a project uses `@rcubinity/core-angular` (Angular)

- Treat **`@rcubinity/core-angular`** as the **canonical** layer for **`ApiService`**, route-keyed requests, loaders, and the HTTP patterns the app already configures (providers/interceptors). **Do not** bypass it with raw `HttpClient` in features unless the repo explicitly documents an exception.
- Follow the project’s rule: subscribe to the API call’s **`subject` before `exe()`** when using `ApiService`.
- Keep **route names** aligned with the backend catalog (`dump/default-routes.json` or equivalent); no hardcoded URLs.

## When a project uses a shared UI library

- Respect **peer dependency** versions and **public API** of internal packages; prefer extending existing primitives over duplicating behavior.
