# Data, pagination, and offline behavior

## Mapping and contracts

- Domain entities express business concepts.
- DTOs map transport payloads to domain entities.
- Local persistence models remain in data/infrastructure.
- Repositories expose domain-oriented operations and hide whether data came from network, cache, or local storage.

## Cursor pagination

When an API returns an opaque cursor:

- Send the cursor unchanged; never decode, manufacture, or calculate it from counts.
- Omit it for the first page and refresh.
- Prevent overlapping load-more requests.
- Ignore stale responses after a query/filter changes.
- Deduplicate appended items by stable ID.
- Preserve an existing cursor after a load-more error so the user can retry.

## Offline-first is a product decision

Do not add caching or queued writes because they sound robust. Define the promise first.

For a read-offline application:

- Cache successful reads behind repository contracts.
- Keep authenticated data, cache keys, and organization/account scope separated.
- Communicate offline or stale data without blocking useful cached content.
- Keep mutations online-only until a real outbox design exists.
- Never claim a remote mutation succeeded while offline.

Queued writes require stable operation IDs, idempotency, retries, conflict resolution, durable state, and clear user-visible sync status. Treat them as a dedicated feature, not Cubit-side convenience logic.

## Local schema evolution

Treat database schema versions and cache-envelope versions as contracts. Every incompatible schema change needs a migration and test. Optional corrupt or obsolete cache records should degrade safely rather than block startup. Keep authentication tokens in secure storage rather than application databases.
