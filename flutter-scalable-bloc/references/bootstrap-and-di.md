# Bootstrap and dependency injection

Use one `GetIt` instance, exposed by `service_locator/service_locator.dart`. Registration is composition-root work only: constructors receive dependencies; domain, data, services, Cubits, and widgets do not call the locator to find their own dependencies.

## Startup order

Keep `main.dart` short and ordered:

```text
WidgetsFlutterBinding.ensureInitialized()
  -> await ServiceLocator.initialize()
  -> initialize router
  -> register notification/deep-link callbacks using resolved services
  -> runApp
```

`ServiceLocator.initialize()` is a coordinator, not a long registration dump. It calls small registration modules in dependency order:

```text
app services
  -> local database/cache and awaited persistent prerequisites
  -> repositories and network implementation
  -> stores
  -> use cases
  -> notification/deep-link handlers
  -> feature navigators and Cubits
```

Await every prerequisite that must be ready before the UI or a dependent service can use it: database open/migrations, secure state restoration, cache hydration, connectivity initialization, or push-service initialization. Do not start the app while a required dependency is half initialized.

## Registration rules

- Register abstractions to concrete implementations in the composition root, for example `NetworkRepository -> DioNetworkRepository` and `TrainingRepository -> TrainingRepositoryImp`.
- When one concrete database serves several contracts, construct it once, await its initialization, then register that same instance against each applicable contract.
- Register shared, stateful infrastructure as a singleton: secure storage, database/cache, network client, connectivity, platform services, stores, app navigation, and long-lived notification handlers.
- Register stateless use cases as singletons when their dependencies are singleton and they retain no feature-specific state.
- Register a feature navigator immediately before its Cubit so the pair is easy to audit.
- Do not register a concrete implementation for direct use elsewhere when the contract is sufficient.
- Keep feature registrations grouped by area/flow in `app_cubits.dart`; keep imports and registrations ordered by the same concern.

## Cubit lifetime

Use `registerSingleton` for a Cubit only when its state is intentionally shared or should survive navigation, such as an app shell, dashboard, cart, inbox, or a single non-reentrant screen flow.

Use `registerFactory` for a Cubit and navigator when it has route-specific initial parameters, owns controllers/streams that must be disposed per route instance, may be opened more than once, or concurrent instances would otherwise share state incorrectly. A factory Cubit must be closed by the widget/provider that creates it.

Do not choose singleton merely to reduce object creation. Choose it only when the intended state lifetime is application-wide or deliberately persistent.

## Minimal pattern

```dart
class AppCubits {
  static void register() {
    getIt.registerSingleton<TrainingNavigator>(TrainingNavigator(getIt()));
    getIt.registerFactory<TrainingCubit>(
      () => TrainingCubit(navigator: getIt(), saveTrainingUseCase: getIt()),
    );
  }
}
```

The page resolves the Cubit at its composition boundary and provides it to the subtree. The Cubit itself never resolves dependencies from `GetIt`.
