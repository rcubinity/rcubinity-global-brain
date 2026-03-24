# Core NestJS skill (`core/*`) for Cursor

Use this when a NestJS repo has a shared in-repo layer like `core/` (path alias `core/*`), and you want Cursor to implement features in the same style instead of duplicating framework code in `src/`.

## Goal

- Reuse the shared `core/` building blocks first.
- Keep controllers thin and consistent across modules.
- Preserve frontend/backend route-key parity using metadata-driven routes.
- Centralize auth, validation, and response envelopes.

## Rule 1: always check `core/` before writing new code

Before adding a helper, decorator, guard, or cross-cutting service in feature code:

1. Search `core/` for an existing implementation.
2. If found, import from `core/...` and follow its usage pattern.
3. If missing and the behavior is reusable, add it in `core/` (not one-off in a module).

## Deep reference: key functions and how to use them

### `success()` and `failure()` in `core/helpers/response.ts`

- `success(data, message, statusCode)` returns a normalized `{ status: "OK", data, message, statusCode }` payload.
- `failure(message, err, statusCode)` returns `{ status: "FAILED", data: err, message, statusCode }`.
- `failure()` also unwraps TypeORM duplicate key errors (`ER_DUP_ENTRY`) into a friendlier message.
- Why it matters: response contract stays stable for Angular/Ionic clients and avoids route-by-route envelope drift.

Use when:
- Returning service/controller outcomes in HTTP handlers.
- Converting low-level exceptions into consistent API failures.

Avoid:
- Returning mixed envelopes (`{ ok: true }`, raw entities, ad-hoc shapes) in the same module.

### `CoreController()` in `core/nestjs/decorators/controller.decorator.ts`

- Wraps Nest `@Controller` plus Swagger metadata (`ApiTags`, common 500 response).
- Adds controller-level metadata keys (`controllerTag`, `controllerDescription`).
- Optionally applies bearer auth + guard with `auth.enabled`.

Why it matters:
- Gives every controller common metadata and docs behavior.
- Reduces boilerplate and keeps auth behavior explicit.

Use when:
- Creating or refactoring controllers that should follow IRAS shared conventions.

### `RequestRoute()` and `RequestGet/Post/Put/Delete()` in `core/nestjs/decorators/request.decorator.ts`

- Attaches route metadata: `name`, `prefix`, `info`, validation descriptor.
- Adds Swagger operation/response docs.
- Injects `JoiValidationInterceptor` when `validation` schema exists.
- HTTP decorators (`RequestGet`, etc.) combine method + metadata in one call.

Critical fields:
- `name`: stable route key; must align with route catalog expected by frontend.
- `prefix`: grouping key that composes into nested route names.
- `info`: human-readable route intent.
- `validation`: Joi schema or `null`.

Why it matters:
- Route catalog and generated maps depend on this metadata.
- Missing or inconsistent names break client route-key calls.

### `JoiValidationInterceptor` in `core/nestjs/interceptors/JoiValidation.interceptor.ts`

- Builds a validation object from `{ body, params, query }`.
- Cleans null-prototype/empty structures via `removeNullPrototypeProperties`.
- Parses query string via `queryStringToObject`.
- Runs Joi schema validation and throws `BadRequestException` on error.

Why it matters:
- Validation stays centralized and uniform across routes.
- Avoids fragile per-handler manual validation.

### `CustomResponseInterceptor` in `core/nestjs/interceptors/CustomResponse.interceptor.ts`

- Reads `statusCode` from handler return payload and sets HTTP status accordingly.
- Supports response envelope-driven status control (`success(..., ..., 201)` etc.).

Why it matters:
- Keeps HTTP status and payload envelope synchronized without repeating `@HttpCode`.

### Auth primitives: `JwtAuthGuard` and `JwtStrategy`

- `JwtAuthGuard` is a thin `AuthGuard("jwt")` wrapper.
- `JwtStrategy.validate(payload)`:
  - supports multiple payload shapes,
  - decrypts user id (`decryptId`),
  - fetches user + relations,
  - returns enriched `request.user` context (`id`, `userType`, `schoolId`),
  - throws `UnauthorizedException` for invalid tokens.

Why it matters:
- Token parsing, identity hydration, and request context are centralized.
- Controllers/services can trust `request.user` shape once guard is applied.

### `CurrentUser` decorator in `core/nestjs/decorators/currentUser.decorator.ts`

- Returns `request.user` from execution context.
- Keeps handlers clean and avoids repetitive request plumbing.

### Permission model: `RequirePermissions()` + `PermissionGuard`

- `RequirePermissions(...keys)` stores required permissions in metadata.
- `PermissionGuard.canActivate()` reads metadata, checks authenticated user, delegates permission checks to `URPService`.

Why it matters:
- Authorization policy remains declarative at route level.
- Permission logic stays in URP instead of ad-hoc controller conditionals.

### Route discovery: `RouteService` in `core/nestjs/services/route.service.ts`

- Scans registered routes from adapter/router stack during module init.
- Extracts metadata (`name`, `prefix`, `info`, `validation`).
- Builds mapped/nested route objects via `getRoutesByModule()`.
- Normalizes keys using `camelCaseWithDot()`.

Why it matters:
- Powers route-map exports and keeps backend/frontend naming parity.
- Surfaces missing `name` metadata early (warning path).

### Utility functions used by request metadata and validation

- `camelCaseWithDot(input)` in `core/utils/string.util.ts`:
  normalizes dotted/hyphenated names into stable route-key style.
- `removeNullPrototypeProperties(obj)` in `core/utils/object.utility.ts`:
  strips empty/null-prototype request containers before Joi validation.
- `queryStringToObject(queryString)` in `core/utils/object.utility.ts`:
  converts query string into an object with numeric coercion where possible.

## Controller implementation template (preferred)

1. Decorate class with `@CoreController(...)`.
2. Decorate handlers with `@RequestGet/Post/Put/Delete(...)` and a stable `name`.
3. Add Joi schema to `validation` for request contracts.
4. Apply `@UseGuards(JwtAuthGuard, PermissionGuard)` where required.
5. Declare `@RequirePermissions(...)` for protected operations.
6. Use `success()` / `failure()` for consistent responses.

## Service implementation template (preferred)

- Keep orchestration/business logic in services.
- Keep controllers focused on request wiring and delegation.
- For reusable domain logic, extract shared pieces to `core/` where appropriate.

## Definition of done for `core`-aligned Nest work

- Route has `name` + `prefix` metadata where relevant.
- Joi validation exists and matches DTO/expected payload.
- Auth and permission guards are correctly applied.
- Responses use `success()` / `failure()` contract.
- No duplicated helper/decorator/guard that already exists in `core/`.
- If behavior changed materially, update colocated context docs where used.

## Scope note

This skill explains the foundational cross-cutting functions in `core/*` deeply. Domain modules (chat, notification, media, URP sub-services, storage, etc.) should follow the same principles: shared logic in `core/`, thin feature handlers in `src/`, stable route metadata, and explicit side effects.
