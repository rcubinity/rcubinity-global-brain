# Python services (APIs, workers, gateways)

Applies to Python microservices, Socket.IO/ASGI apps, CLIs, and on-device gateways.

## Structure

- Separate **transport** (HTTP, WebSocket, MQTT) from **domain logic** where possible.
- **Configuration**: load from environment variables; document required vars in `README` or `.env.example`.
- **Logging**: structured or consistent format; include correlation/request ids when the stack supports it.

## Networking

- For **Socket.IO** or similar, follow existing **auth**, **heartbeat**, and **reconnect** patterns in the repo.
- Validate **client identity** (tokens, device keys) before processing payloads.

## Data

- Use the project’s **DB/Redis** clients; avoid blocking the event loop in async servers.
- For numerical pipelines (e.g. positioning, signal processing), keep **pure math** testable and **I/O** at the edges.

## Security

- No secrets in source control; use env and secret managers as appropriate.
