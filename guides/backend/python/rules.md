# Python services (APIs, workers, gateways)

- Separate transport (HTTP/WebSocket/MQTT) from domain logic where possible.
- Load configuration from environment variables; document required vars.
- Use consistent structured logging and avoid sensitive logs.
- Validate client identity and payloads on all ingress paths.
- Keep computational pipelines testable and isolate I/O at boundaries.
