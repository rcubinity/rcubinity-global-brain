# `@rcubinity/core-angular` skill for Cursor

Use when Angular/Ionic apps depend on `@rcubinity/core-angular`.

## Core flow

1. Build request with `ApiService.apiCall(routeKey)`.
2. Set payload/params/loader/formData.
3. Subscribe to `apiCall.subject` before `apiCall.exe()`.
4. Execute and handle response via shared envelope patterns.

## Key building blocks

- `ApiService`: backend route-map loading and `ApiCall` creation.
- `ApiCall`: validation, execution, progress, response envelope handling.
- `coreSignal`: state + persistence helper.
- `AuthService` + interceptor: token/session behavior.
- `Model<T>` + decorators: payload hydration and nested model mapping.

## Done criteria

- Route-key API calls only.
- Correct subscription order.
- No duplicated auth/API plumbing outside core-angular path.
