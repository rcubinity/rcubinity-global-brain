# Core NestJS skill (`core/*`) for Cursor

Use for repos with a shared in-repo `core/` layer.

## Key primitives

- `success()` / `failure()` for stable response envelopes.
- `CoreController()` for shared controller metadata.
- `RequestGet/Post/Put/Delete()` for route metadata + validation wiring.
- `JoiValidationInterceptor` for centralized validation.
- `JwtAuthGuard` + strategy for auth context.
- `RequirePermissions` + `PermissionGuard` for declarative authorization.
- `RouteService` for metadata-based route map extraction.

## Implementation checklist

1. Reuse `core/...` imports first.
2. Use shared decorators and guards.
3. Keep route name/prefix/path aligned with route catalog.
4. Return consistent `success`/`failure` payloads.
