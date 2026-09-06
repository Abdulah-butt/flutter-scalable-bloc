# Layers and decision rules

## Default dependency direction

`presentation -> domain <- data`

Infrastructure is accessed through contracts: repositories describe data access; services describe device or third-party capabilities. Concrete implementations are registered at startup through the app's dependency-injection container.

## Feature communication contract

For a non-trivial user action, keep the call path explicit:

```text
Page / Widget
  -> Feature Cubit or BLoC
  -> Use case (when it owns reusable business policy)
  -> Repository contract (domain)
  -> Repository implementation (data)
  -> Remote/local data source, network client, or platform service
```

The result comes back in reverse, as domain entities or typed failures. The Cubit maps that result into its immutable UI state, and the page renders the state. Dependencies only point to the right; data depends on domain to implement its contracts, while domain never imports data or presentation.

### Boundary responsibilities

| Boundary | May do | Must not do |
| --- | --- | --- |
| Widget/page | Capture input, render state, trigger Cubit commands | Business decisions, direct repository/service calls |
| Cubit/BLoC | Coordinate one feature, validate UI input, emit loading/success/error state | JSON mapping, endpoint selection, database/plugin access |
| Use case | Reusable product rule, multi-repository/service workflow, permission/policy decision | UI state or Flutter dependencies |
| Repository contract | Define domain-oriented reads/writes and return entities/results | Refer to Dio, DTOs, SQL, or plugin classes |
| Repository implementation | Choose source, map DTOs/rows to entities, apply cache strategy | Expose transport/persistence types upward |
| Data source/service | Execute HTTP, database, or device operation | Contain feature UI state or product orchestration |

Name the concrete class after the contract, such as `TrainingRepository` and `TrainingRepositoryImpl`; inject the contract into use cases and Cubits, never the implementation.

Do not force a use case for a simple one-to-one repository call. When one is not justified, use `Cubit -> Repository contract -> Repository implementation`; retain the same dependency rules.

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
