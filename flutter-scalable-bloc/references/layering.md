# Layers and decision rules

## Default dependency direction

`presentation -> domain <- data`

Infrastructure is accessed through contracts: repositories describe data access; services describe device or third-party capabilities. Concrete implementations are registered at startup through the app's dependency-injection container.

## Put work in the narrowest correct layer

| Need | Place it in |
| --- | --- |
| Rendering, interaction wiring, local controller lifetime | Page or feature widget |
| Loading state, validation, orchestration, one screen's behavior | Feature Cubit |
| Reusable business operation or policy | Domain use case |
| Business object independent of JSON/database/UI | Domain entity |
| Data-access API that the domain needs | Domain repository contract |
| HTTP/database implementation and DTO conversion | Data repository and models |
| Request execution, headers, retries, endpoint definitions | Network layer |
| Location, secure storage, notifications, pickers, connectivity, files | Service contract plus implementation |
| State shared by independent features | Domain store |

## Rules that prevent architectural bloat

- Do not make a use case that only forwards one repository method with no reusable policy.
- Do not create a global store for one page's state.
- Do not expose a DTO, Drift row, Dio response, plugin type, or `BuildContext` from a domain contract.
- Do not make a generic repository with untyped `Map` payloads when a feature-specific contract is clearer.
- If a platform integration can change by operating system or vendor, depend on a service contract rather than the plugin directly.

## Dependency injection

Register an abstraction once with a concrete implementation at bootstrap. Keep registrations grouped by concern: infrastructure/services, repositories, stores, use cases, then feature Cubits. Initialization that must complete before use—database open, secure storage restore, persisted store hydration—must finish before the app depends on it.
