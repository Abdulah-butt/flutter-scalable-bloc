# Cubit lifecycle and async safety

## Owned resources

If a Cubit creates or receives ownership of a `TextEditingController`, `ScrollController`, `FocusNode`, stream subscription, timer, debounce, socket listener, or similar disposable resource, release it in `close()`. Cancel pending timers/subscriptions before calling `super.close()`.

Do not dispose resources owned by a widget, shared store, or service.

## Async state correctness

- Emit a clear loading/error/success state for visible operations while preserving existing useful content during refresh where appropriate.
- Prevent concurrent load-more calls.
- When a query, filter, route parameter, or selection changes, ensure an older response cannot overwrite the latest state. Use a request generation/token, cancellation, or the app's existing equivalent.
- Debounce only input-driven work that benefits from it, such as search; cancel the pending debounce on `close()`.
- Preserve the last valid pagination cursor after a load-more failure so the user can retry.
- Check the established Cubit/package behavior before emitting after asynchronous work when the Cubit may have closed.
