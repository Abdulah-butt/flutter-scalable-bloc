# Required project structure and request convention

Use this layout for a scalable app. Keep folders and file responsibilities consistent; add a new category only when it has a real architectural purpose.

```text
lib/
├── core/                         # app-wide theme, routes, utilities, extensions, alerts, enums
├── network/
│   ├── request_model/             # one typed request class per API operation
│   ├── dio/                       # Dio-backed network implementation
│   ├── interceptors/              # auth, tenant/context, logging interceptors
│   ├── api_endpoint.dart          # centralized endpoint constants
│   ├── network_repository.dart    # network abstraction
│   └── binary_network_repository.dart
├── domain/
│   ├── entities/                  # framework-free business objects
│   ├── repositories/              # repository contracts, grouped by concern
│   ├── stores/                    # state shared by independent features
│   └── usecases/                  # one named use case per application action
├── data/
│   ├── models/                    # API/DB models with fromJson and toDomain
│   ├── local/                     # database setup and local persistence support
│   └── repositories/              # concrete repository implementations, grouped by concern
│       └── <concern>/<name>_imp.dart
├── services/
│   └── <capability>/              # contract plus platform/plugin implementation
├── presentation/
│   ├── pages/<area>/<feature>/    # page, cubit, state, navigator, initial params, local widgets
│   ├── sheets/
│   ├── widgets/
│   └── view_data/
└── service_locator/               # dependency registrations by concern and feature
```

## Required request path

For every API operation, create a typed request class in `lib/network/request_model/`. Name it after the action, for example `create_training_request.dart`, and provide a `toJson()` method that normalizes only its own API payload.

```text
Widget/page
  -> FeatureCubit
  -> CreateTrainingUseCase
  -> TrainingRepository              (domain contract)
  -> TrainingRepositoryImp           (data implementation)
  -> NetworkRepository
  -> DioNetworkRepository
  -> API endpoint
```

Return data in reverse. `TrainingRepositoryImp` maps JSON through a data model (for example `TrainingJson.fromJson(...).toDomain()`) before returning a domain entity. A Cubit emits only feature state; it must not expose JSON, DTOs, Dio types, or request infrastructure to the UI.

## File rules

- `network/request_model/` is mandatory for API request payloads. Do not build request maps in widgets, Cubits, use cases, or repository implementations.
- `network/api_endpoint.dart` owns endpoint paths. Do not place endpoint strings elsewhere.
- Domain repository contracts return entities and accept typed request models; never return raw maps or data models.
- Each domain use case is a named class with `execute(...)`, taking the repository contract through its constructor.
- Concrete repositories live in `data/repositories/<concern>/` and use the `_imp.dart` suffix. They depend on `NetworkRepository`, not directly on `Dio`.
- Name the implementation class with the matching `Imp` suffix, for example `TrainingRepositoryImp`.
- Data models belong in `data/models/`; each has parsing and a `toDomain()` conversion when it represents a domain entity.
- Cubits live beside the page they power and receive use cases, services, stores, navigator, and UI helpers through constructors. Do not use a service locator inside a Cubit.
- Register contracts to implementations in `service_locator/`; inject the contract everywhere else.
