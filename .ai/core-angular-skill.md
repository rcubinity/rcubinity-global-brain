# `@rcubinity/core-angular` skill for Cursor

Use this when Angular/Ionic apps depend on `@rcubinity/core-angular` and you want Cursor to follow the package's native request/state/auth/model patterns instead of building parallel wrappers.

Source baseline:
- package: `@rcubinity/core-angular` (`core-angular/package.json`)
- public exports: `core-angular/src/public-api.ts`

## Goal

- Keep API calls route-keyed (`default-routes` style), not URL-hardcoded.
- Reuse `ApiService` + `ApiCall` flow for all HTTP features.
- Use `coreSignal` for persistent reactive state where applicable.
- Preserve auth/session conventions (`AuthService`, interceptor, token flow).
- Keep model hydration consistent with `Model` utilities and decorators.

## Rule 1: never bypass the library for standard app calls

Before writing new frontend network/state/auth code:

1. Check if `@rcubinity/core-angular` already exports the capability.
2. If yes, use/extend that pattern in app feature services.
3. Only use raw `HttpClient` directly when explicitly justified by project docs.

## Deep reference: key classes/functions and expected usage

### `ApiService` (`lib/server/api.service.ts`)

Primary responsibilities:
- Stores backend server definitions in a persistent `coreSignal` (`servers`).
- Fetches backend route maps from each backend `routesUrl`.
- Creates request objects via `apiCall(apiDotPath, keyName)`.

Important functions:
- `init()`:
  - loads configured backends (`CORE_ANGULAR_CONFIG.backends`),
  - fetches route catalog from `${baseUrl}${routesUrl}`,
  - stores `{ baseUrl, routesUrl, routes }` in `servers`.
- `addBackend(...)` / `removeBackend(...)`: runtime backend registry management.
- `fetchBackendRoutes(url)`: expects backend response envelope `{ status: "OK", data }`.

Why it matters:
- Route lookup in `ApiCall` depends on `servers()[key].routes`.
- If routes are not loaded, `apiCall("x.y.z")` cannot resolve endpoint metadata.

### `ApiCall` + `RequestLoader` (`lib/server/ApiCall.ts`)

`ApiCall` is the canonical request lifecycle object.

Core behavior:
- Route metadata resolution:
  - `loadApiObject()` resolves route definition from route map using `apiDotPath`.
  - includes retry loop; throws if route key never appears.
- Validation:
  - `isValid()` executes `paramsValidator` / `dataValidator` (Joi) when available.
- Execution:
  - `exe()` starts async metadata load then request execution.
  - `exeAsync()` allows explicit await-first flow.
  - `executeRequest()` performs HTTP call, progress handling, response mapping, errors.
- Upload mode:
  - `formData = true` converts payload (including `File` / arrays) to `FormData`.
- Request metadata:
  - includes headers like `requestName` and `silent`.

`RequestLoader`:
- `start()` toggles loading state to `true`.
- `stop()` defers reset briefly to avoid UI flicker.

Critical consumption pattern:
1. Create call: `const apiCall = apiService.apiCall("module.action")`
2. Set `apiCall.data`, `apiCall.params`, `apiCall.loader`, `apiCall.formData` as needed
3. Subscribe to `apiCall.subject` **before** execute
4. Execute with `apiCall.exe()`

Why subscribe first:
- `subject` is where response/error events are observed.
- late subscription can miss early emissions and break feature behavior.

### `coreSignal()` (`lib/hooks/coreSignal.ts`)

A signal wrapper with:
- `setValue()` updates signal + optional IndexedDB persistence.
- `subject` (`BehaviorSubject`) bridge for observable consumers.
- optional bootstrapping from IndexedDB with transform.
- path-based update helper `updateValue(path, newValue)`.

Why it matters:
- Many shared services rely on this as a stable app-level state primitive.
- Keeps signal state and local persistence synchronized in one place.

### `CORE_ANGULAR_CONFIG` (`lib/config.ts`)

Injection token for library configuration:
- defines backend endpoints map (`backends`)
- supports debug and cache options

Why it matters:
- `ApiService`, base components, and auth flows inject this token.
- Misconfigured backends break route resolution globally.

### `AuthService` (`lib/auth/auth.service.ts`)

Main flow:
- `login(credentials, loader)`:
  - builds login `ApiCall`,
  - wires `subject` listener,
  - executes call,
  - on success updates user/token state.
- `logout()`:
  - runs lifecycle hooks,
  - clears cookies/local storage keys,
  - dispatches logout hooks.

Extension points:
- hook constants (`AUTH_HOOK`) for pre/post login/logout integration.
- override-friendly methods: `prepareLoginCall`, `beforeLogout`, `afterLogout`, etc.

Why it matters:
- Centralizes session transitions; features should not duplicate token/user write logic.

### `AuthInterceptor` (`lib/interceptor/auth.interceptor.ts`)

- Adds `Authorization: Bearer <token>` from localStorage when token exists.
- On `401`, triggers `authService.logout()` and normalizes session-expired behavior.

Why it matters:
- Keeps auth header and session expiry behavior centralized.

### `Model<T>` (`lib/models/Model.ts`) + decorators

`Model<T>` provides:
- `set(data)` recursive hydration,
- `create(...)` / `createFromArray(...)` factories,
- local persistence helpers (`saveLocal`, `loadFromLocal`).

Decorators:
- `ClassDecorator(() => ChildClass)` maps nested object fields.
- `ArrayClassDecorator(() => ChildClass)` (exported) maps nested arrays.

Why it matters:
- Prevents ad-hoc payload-to-model mapping across features.
- Preserves class methods and nested model types after hydration.

## Feature-service template (preferred)

1. Inject `ApiService` in a feature service.
2. Create route-keyed `ApiCall`.
3. Assign `data`/`params` and optional `RequestLoader`.
4. Subscribe to `apiCall.subject` first.
5. Execute via `apiCall.exe()`.
6. In component, consume the service call and react to `apiCall.response.status`.

## Common mistakes to avoid

- Hardcoding API URLs/methods in components or feature services.
- Calling `exe()` before attaching `subject` subscriptions.
- Duplicating auth header/401 logic outside interceptor/auth service.
- Building custom state persistence wrappers instead of `coreSignal`.
- Parsing nested models manually when `Model` decorators can do it.

## Definition of done for `core-angular`-aligned work

- HTTP calls use `ApiService`/`ApiCall` and route keys.
- Subscription order (`subject` before `exe`) is respected.
- `RequestLoader` used where UX needs loading feedback.
- Upload paths use `formData = true` and proper payload shape.
- Auth-sensitive routes rely on interceptor/session flow, not custom token plumbing.
- Model mapping uses `Model` + decorators where nested structures exist.

## Scope note

This skill covers the main foundation of `@rcubinity/core-angular`. Additional exports (routing animations, directives, notifications, socket helpers, route-reuse strategy, dependency manager) should be used by extending existing exported APIs rather than introducing parallel feature-specific replacements.
