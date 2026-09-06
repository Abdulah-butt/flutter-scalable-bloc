# Network, session, and reliable mutations

## Network owns transport policy

Keep base URL selection, timeouts, JSON/form-data encoding, response unwrapping, status-to-error mapping, and connectivity fallback inside the network implementation. Repositories call the network abstraction and map successful payloads to domain entities; Cubits receive domain results or established app errors only.

Define application-facing API error/status values in one network location. Do not parse `DioException`, status codes, or backend response envelopes in Cubits or widgets.

## Interceptors and session ownership

- Attach authentication and active account/organization context through interceptors, not individual endpoints or feature code.
- Handle token refresh and an expired session once at the network/bootstrap boundary. The recovery path may clear authenticated state and route to the app's entry flow; individual Cubits must not duplicate it.
- Keep tokens and other secrets in secure storage, never request models, logs, route parameters, or the application database.
- Enable verbose request/response logs only in safe development builds and redact credentials and personally identifiable data.

## Retryable mutations

For create, submit, payment, or other operations where retrying could duplicate a server-side action, pass a generated idempotency key through the use case and repository to the request. Reuse that key only for a retry of the same logical action; create a new key for a new action.

Do not silently retry a mutation unless the operation is idempotent and the product behavior is clear. Never report a remote write as successful when the device is offline.
