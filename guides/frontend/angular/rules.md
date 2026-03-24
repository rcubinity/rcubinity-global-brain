# Frontend (Angular and similar SPAs)

Applies to Angular (and analogous) single-page apps that talk to typed backend contracts.

## Architecture

- Prefer standalone components when repo already uses them.
- Prefer signals or the established state pattern over ad-hoc subscription chains.
- Keep templates thin; put orchestration and HTTP logic in services.
- Use lazy loading where routing style supports it.

## API and data access

- Do not hardcode URLs/methods when route keys/catalog are available.
- Use the central API helper and route-name keys.
- Subscribe to request subjects before executing calls where stack requires it.
- Use existing upload patterns (`FormData`, interceptors, multipart) in repo.

## `@rcubinity/core-angular`

- Treat `@rcubinity/core-angular` as canonical for `ApiService`, route-keyed requests, loaders, and configured HTTP/auth wiring.
- Do not bypass with raw `HttpClient` in feature code unless repo docs explicitly allow an exception.
