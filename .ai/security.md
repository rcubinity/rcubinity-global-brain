# Security (baseline)

## Input and output

- **Validate and sanitize** all external inputs (HTTP bodies, query params, WebSocket payloads, file uploads).
- Encode output appropriately to mitigate **XSS** in web apps; use framework defaults for **CSRF** where applicable.

## Authentication and authorization

- Prefer industry-standard **token** flows (e.g. JWT with short lifetimes, refresh where used).
- **Passwords**: strong hashing (e.g. bcrypt/argon2) per project policy; never log secrets.
- Enforce **authorization** at the handler/service layer, not only in the UI.

## Transport and data

- **HTTPS** in production; secure cookies when used.
- **SQL/NoSQL injection**: use parameterized queries / ORM correctly.
- **Least privilege** for service accounts and DB users.

## Dependencies

- Keep dependencies updated; scan for known vulnerabilities as part of CI where possible.
