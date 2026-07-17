# Flutter Mobile Checklist

> Flutter-specific companion to the general [Mobile Checklist](mobile.md).
> Covers Flutter 3.x+, Dart 3.x+. Ecosystem: Riverpod 3, BLoC 9, go_router, Dio, freezed, drift.
> Last updated: 2026-07-17

---

## 1. Project Setup & Tooling

- [ ] **Flutter SDK pinned** — Use `fvm` (Flutter Version Management) or `.flutter-version` file. Team and CI must use the same Flutter version. `fvm use 3.x.x` → generates `.fvmrc`.
- [ ] **Dart strict mode** — `analysis_options.yaml` with strict settings:

```yaml
analyzer:
  language:
    strict-casts: true
    strict-raw-types: true
    strict-inference: true
  errors:
    missing_return: error
    dead_code: warning
```

- [ ] **Linting** — `very_good_analysis` (VGV) for opinionated rules, or `flutter_lints` (official). Add custom rules for team conventions. CI blocks on lint failures.
- [ ] **Project structure: feature-first** — Not layer-first (`/controllers`, `/models`, `/services`). Each feature is self-contained. Shared code in `core/`.
- [ ] **Flavors / environments** — Dev, staging, prod via `--dart-define-from-file=config/dev.json` or `flutter_flavorizr`. Separate bundle IDs, app names, API URLs per flavor.
- [ ] **Code generation** — `build_runner` + `freezed` (models), `json_serializable` (JSON), `riverpod_generator` (@riverpod). Run: `dart run build_runner build --delete-conflicting-outputs`.
- [ ] **Monorepo with Melos** — If multi-package (shared code, design system, feature packages). `melos bootstrap`, `melos run test --no-select`. Publish internal packages to private pub server or use path dependencies.

## 2. Architecture & Project Structure

- [ ] **Clean Architecture / Feature-first** — Domain layer has no Flutter imports. Data layer implements domain interfaces. Presentation depends on domain, never on data directly.

```
lib/
├── core/           ← shared utilities, constants, theme, DI
├── features/
│   ├── auth/
│   │   ├── data/       ← repositories impl, data sources, models
│   │   ├── domain/     ← entities, repository interfaces, use cases
│   │   └── presentation/ ← screens, widgets, state (BLoC/Riverpod)
│   ├── home/
│   └── settings/
└── app.dart        ← MaterialApp, router, providers
```

- [ ] **Dependency injection** — Riverpod's built-in DI (providers ARE the DI container) or `get_it` + `injectable` (code-gen). Riverpod preferred — compile-time safe, no service locator.
- [ ] **Repository pattern** — Domain defines the interface, data implements it:

```dart
// domain/repositories/auth_repository.dart
abstract class AuthRepository {
  Future<User> login(String email, String password);
  Future<void> logout();
}

// data/repositories/auth_repository_impl.dart
class AuthRepositoryImpl implements AuthRepository {
  final ApiClient _client;
  final SecureStorage _storage;
  // ...
}
```

- [ ] **Use cases optional** — Skip if service layer is thin (just delegates to repository). Use when business logic spans multiple repositories or requires orchestration.

## 3. State Management

- [ ] **Riverpod 3 (recommended)** — Compile-time safe, no `BuildContext` needed for reading state, code-gen with `@riverpod`. Ideal for 2026 projects.

```dart
@riverpod
class UserProfile extends _$UserProfile {
  @override
  Future<User> build(String userId) async {
    final repo = ref.watch(userRepositoryProvider);
    return repo.getUser(userId);
  }

  Future<void> updateName(String name) async {
    state = const AsyncLoading();
    state = await AsyncValue.guard(() =>
      ref.read(userRepositoryProvider).updateUser(name),
    );
  }
}

// In widget:
final userAsync = ref.watch(userProfileProvider(userId));
userAsync.when(
  data: (user) => Text(user.name),
  loading: () => const CircularProgressIndicator(),
  error: (e, st) => Text('Error: $e'),
);
```

- [ ] **BLoC 9 (alternative)** — Event-driven, strict separation, excellent for large teams with enforced patterns:

```dart
// Events
sealed class AuthEvent {}
class LoginRequested extends AuthEvent {
  final String email, password;
  LoginRequested(this.email, this.password);
}

// States
sealed class AuthState {}
class AuthInitial extends AuthState {}
class AuthLoading extends AuthState {}
class AuthSuccess extends AuthState { final User user; ... }
class AuthFailure extends AuthState { final String message; ... }

// BLoC
class AuthBloc extends Bloc<AuthEvent, AuthState> {
  AuthBloc(this._repo) : super(AuthInitial()) {
    on<LoginRequested>(_onLogin);
  }
  Future<void> _onLogin(LoginRequested event, Emitter<AuthState> emit) async {
    emit(AuthLoading());
    try {
      final user = await _repo.login(event.email, event.password);
      emit(AuthSuccess(user));
    } catch (e) {
      emit(AuthFailure(e.toString()));
    }
  }
}
```

- [ ] **Local/ephemeral state** — `ValueNotifier`, `useState` (flutter_hooks), or `StatefulWidget` for UI-only state (toggle, animation, form input). Don't globalize what's local.
- [ ] **Never put server state in global state** — Use query/cache pattern: Riverpod's `AsyncValue` or BLoC with repository caching. Server data should be fetched, cached, invalidated — not manually managed.

## 4. Navigation & Routing

- [ ] **go_router** — Declarative, type-safe routes, deep linking support, nested navigation, ShellRoute for bottom tabs.

```dart
final router = GoRouter(
  initialLocation: '/home',
  redirect: (context, state) {
    final isLoggedIn = ref.read(authStateProvider).isAuthenticated;
    final isLoginRoute = state.matchedLocation == '/login';
    if (!isLoggedIn && !isLoginRoute) return '/login';
    if (isLoggedIn && isLoginRoute) return '/home';
    return null;
  },
  routes: [
    GoRoute(path: '/login', builder: (_, __) => const LoginScreen()),
    ShellRoute(
      builder: (_, __, child) => MainShell(child: child),
      routes: [
        GoRoute(path: '/home', builder: (_, __) => const HomeScreen()),
        GoRoute(path: '/profile', builder: (_, __) => const ProfileScreen()),
        GoRoute(
          path: '/orders/:id',
          builder: (_, state) => OrderDetailScreen(
            id: state.pathParameters['id']!,
          ),
        ),
      ],
    ),
  ],
);
```

- [ ] **Route guards** — Auth redirect in `GoRouter.redirect`. Role-based access. One place to enforce.
- [ ] **Deep linking** — Configure `AndroidManifest.xml` (intent-filter) + `Info.plist` (Associated Domains) + router handles the path. Test on both platforms with `adb shell am start` and `xcrun simctl openurl`.
- [ ] **Bottom navigation with ShellRoute** — Each tab maintains its own stack. Tab state preserved on switch. `StatefulShellRoute` for persistent tab state.
- [ ] **Modal & dialog routes** — Use `GoRoute` with `pageBuilder` returning `MaterialPage` or `DialogPage`. Back button dismisses properly.

## 5. Networking & API

- [ ] **Dio** — HTTP client with interceptors, retry, cancel tokens, logging. Don't use raw `http` package for real apps.
- [ ] **Retrofit + Dio** — Type-safe API clients via code generation:

```dart
@RestApi(baseUrl: '')
abstract class ApiClient {
  factory ApiClient(Dio dio) = _ApiClient;

  @GET('/users/{id}')
  Future<UserDto> getUser(@Path('id') String id);

  @POST('/auth/login')
  Future<TokenDto> login(@Body() LoginRequest request);
}
```

- [ ] **Interceptors** — Auth token injection, refresh token flow, error mapping:

```dart
class AuthInterceptor extends Interceptor {
  @override
  void onRequest(RequestOptions options, RequestInterceptorHandler handler) {
    final token = _storage.getAccessToken();
    if (token != null) {
      options.headers['Authorization'] = 'Bearer $token';
    }
    handler.next(options);
  }

  @override
  void onError(DioException err, ErrorInterceptorHandler handler) async {
    if (err.response?.statusCode == 401) {
      // Refresh token flow, retry original request
      final newToken = await _refreshToken();
      if (newToken != null) {
        err.requestOptions.headers['Authorization'] = 'Bearer $newToken';
        final response = await _dio.fetch(err.requestOptions);
        return handler.resolve(response);
      }
    }
    handler.next(err);
  }
}
```

- [ ] **Connectivity detection** — `connectivity_plus`. Show offline banner when no network. Queue mutations for retry.
- [ ] **API models with freezed** — Immutable, union types for success/error, `fromJson`/`toJson` generated:

```dart
@freezed
class ApiResult<T> with _$ApiResult<T> {
  const factory ApiResult.success(T data) = _Success;
  const factory ApiResult.error(String message, {int? code}) = _Error;
}
```

## 6. Data & Storage

- [ ] **Drift** — Type-safe SQLite with reactive queries. For structured relational data. Supports migrations, DAOs, complex queries.
- [ ] **SharedPreferences** — Simple key-value (settings, flags). Not for sensitive data.
- [ ] **flutter_secure_storage** — Tokens, credentials. Uses Keychain (iOS) / Keystore (Android) under the hood.
- [ ] **Hive / Isar** — Fast NoSQL local storage for caching, non-relational data.
- [ ] **Cache strategy** — Cache API responses locally. Show cached data immediately, refresh in background (stale-while-revalidate). Riverpod's `keepAlive` + `ref.invalidate()` pattern.
- [ ] **Data migration** — Drift schema versioning with `MigrationStrategy`. Test upgrades from oldest supported version. Never lose user data.

## 7. UI & Theming

- [ ] **Material 3** — `useMaterial3: true` (default in Flutter 3.x). Custom color scheme from seed:

```dart
ThemeData(
  colorScheme: ColorScheme.fromSeed(
    seedColor: const Color(0xFF6750A4),
    brightness: Brightness.light,
  ),
  useMaterial3: true,
)
```

- [ ] **ThemeExtensions for custom tokens** — App-specific design tokens beyond Material:

```dart
class AppSpacing extends ThemeExtension<AppSpacing> {
  final double sm, md, lg;
  // copyWith, lerp...
}
// Access: Theme.of(context).extension<AppSpacing>()!.md
```

- [ ] **Responsive layouts** — `LayoutBuilder`, `MediaQuery`, or `responsive_framework`. Adaptive layouts for phone vs tablet. Not just stretched phone UI on iPad.
- [ ] **Platform-adaptive widgets** — Material on Android, Cupertino on iOS where it matters (date picker, alert dialog, scroll physics). Use `.adaptive` constructors.
- [ ] **Dark mode** — System-aware + user toggle. Persist preference in SharedPreferences. `ThemeMode.system` / `.light` / `.dark`.
- [ ] **Animations** — Prefer implicit (`AnimatedContainer`, `AnimatedSwitcher`, `AnimatedOpacity`) over explicit (`AnimationController`). 60fps. Profile in DevTools.

## 8. Testing

- [ ] **Unit tests** — Business logic, use cases, repositories. `mocktail` for mocks (not mockito — mocktail is null-safe, simpler):

```dart
class MockUserRepository extends Mock implements UserRepository {}

test('login returns user on success', () async {
  final repo = MockUserRepository();
  when(() => repo.login(any(), any())).thenAnswer((_) async => testUser);
  final result = await LoginUseCase(repo).execute('a@b.com', 'pass');
  expect(result, testUser);
});
```

- [ ] **Widget tests** — Screen rendering, interaction, state changes. No real API calls. `pumpWidget`, `find.byType`, `tester.tap`.
- [ ] **BLoC tests** — `bloc_test` package: given state → when event → then expected states:

```dart
blocTest<AuthBloc, AuthState>(
  'emits [loading, success] on login',
  build: () => AuthBloc(mockRepo),
  act: (bloc) => bloc.add(LoginRequested('a@b.com', 'pass')),
  expect: () => [AuthLoading(), AuthSuccess(testUser)],
);
```

- [ ] **Golden tests** — `golden_toolkit` for visual regression (screenshot comparison). Catch unintended UI changes.
- [ ] **Integration tests** — `integration_test` package for full app flows on emulator/device.
- [ ] **E2E on real devices** — Patrol (native interactions, permissions, notifications) or Maestro (YAML-based, cross-platform).
- [ ] **CI** — Unit + widget tests on every PR. Integration tests nightly. Golden tests on merge to main.

## 9. Platform Integration

- [ ] **Permissions** — `permission_handler`. Request at point of use, not on startup. Handle denied/permanently denied gracefully with rationale UI.
- [ ] **Push notifications** — `firebase_messaging` (FCM) + `flutter_local_notifications` for foreground display. Handle token refresh. Background message handler in separate isolate.
- [ ] **Deep linking** — `app_links` (replaces uni_links). Test on both platforms. Handle cold-start + background scenarios.
- [ ] **Biometrics** — `local_auth` for Face ID / fingerprint. Fallback to PIN. Use as convenience, not sole auth.
- [ ] **Camera, gallery, files** — `image_picker`, `file_picker`. Handle permission denied. Compress images before upload.
- [ ] **Platform channels** — `MethodChannel` only when no pub.dev package exists. Use Pigeon for type-safe platform communication.

## 10. Performance

- [ ] **Profile mode for perf testing** — `flutter run --profile`. Never measure performance in debug mode (JIT vs AOT difference is massive).
- [ ] **Flutter DevTools** — Widget rebuild count, frame rendering timeline, memory tab. Identify jank sources.
- [ ] **Avoid unnecessary rebuilds** — `const` constructors everywhere possible. Riverpod `select` for granular rebuilds. `BlocSelector` for BLoC. Split large widgets into smaller ones.
- [ ] **ListView.builder** — Lazy rendering for lists. Never `Column` or `ListView` with all children materialized. `SliverList` for complex scrolling layouts.
- [ ] **Image caching** — `cached_network_image`. Resize server-side. Use `Image.asset` for bundled images with correct resolution (`@2x`, `@3x`).
- [ ] **Reduce app size** — `--split-debug-info=build/symbols`, `--obfuscate`, tree-shake unused Material icons (`--no-tree-shake-icons` is the opposite — avoid it), deferred imports for large features.
- [ ] **Isolates for heavy computation** — `Isolate.run()` or `compute()` for JSON parsing, image processing, crypto. Don't block the UI thread. 16ms frame budget.

## 11. Security

- [ ] **flutter_secure_storage** — Tokens in Keychain (iOS) / Keystore (Android). Not SharedPreferences for secrets.
- [ ] **Certificate pinning** — Dio + custom `SecurityContext` or `dio_http2_adapter`. Pin public key hash, not certificate (easier rotation).
- [ ] **Obfuscation** — `--obfuscate --split-debug-info=build/symbols` on release builds. Makes reverse engineering harder.
- [ ] **No secrets in Dart code** — API keys via `--dart-define=API_KEY=xxx` injected at build time. Access: `const String.fromEnvironment('API_KEY')`. Never committed to source.
- [ ] **Root/jailbreak detection** — `flutter_jailbreak_detection` for banking/fintech apps. Decide policy: warn, degrade, or block.
- [ ] **ProGuard/R8** — Enabled for Android release builds. Shrink, obfuscate, optimize. Configure `proguard-rules.pro` for keep rules (Firebase, reflection-based libs).

## 12. Build & Release

- [ ] **Fastlane** — `match` for iOS signing (shared certs via Git repo). `supply` for Play Store uploads. Lanes for build + distribute.
- [ ] **Flavors** — Separate bundle IDs (`com.app.dev`, `com.app.staging`, `com.app.prod`), app names, launcher icons, API URLs. All installable simultaneously.
- [ ] **CI/CD** — GitHub Actions / Codemagic / Bitrise. Lint → test → build on every PR. Deploy on merge to main. Codemagic is Flutter-optimized (pre-warmed Flutter SDKs).
- [ ] **Code signing** — iOS: provisioning profiles via Fastlane `match`. Android: keystore stored in CI secrets (never in repo). Signing configured in `build.gradle`.
- [ ] **Versioning** — `pubspec.yaml` version: `1.2.3+45` (marketing+build). Automate build number in CI: `--build-number=$CI_BUILD_NUMBER`.
- [ ] **Beta distribution** — TestFlight (iOS), Firebase App Distribution (both). Internal testing → closed beta → production.
- [ ] **Staged rollout** — Google Play: 5% → 20% → 100%. Monitor crash rate at each stage. iOS phased release (7-day automatic rollout).

## 13. Packages & Dependencies

- [ ] **Pin critical versions** — `dio: 5.4.1` not `dio: ^5.4.1` for core dependencies. Use caret (`^`) for non-critical only.
- [ ] **Audit packages** — pub.dev scores, maintenance status, last publish date. No packages abandoned > 1 year. Check GitHub issues for unresolved critical bugs.
- [ ] **Minimize dependencies** — Don't add a package for something doable in 10 lines. Every dependency is a liability (breaking updates, abandoned, security vulnerabilities).
- [ ] **License compatibility** — MIT, BSD = fine. GPL = careful with proprietary apps (copyleft). Check transitive deps too.

---

## Quick Sanity Check Before Launch

- [ ] `analysis_options.yaml` has strict-casts + strict-inference enabled
- [ ] App runs on oldest supported OS version without crash
- [ ] `--dart-define` used for all environment-specific values (no hardcoded URLs/keys)
- [ ] `flutter_secure_storage` for tokens, not SharedPreferences
- [ ] Release build uses `--obfuscate --split-debug-info`
- [ ] ProGuard/R8 enabled for Android release
- [ ] `ListView.builder` for all dynamic lists (no `Column` with N children)
- [ ] `const` constructors on all stateless widgets possible
- [ ] Golden tests pass — no unintended visual regressions
- [ ] Deep links tested on cold-start (app killed → link opens correct screen)
- [ ] Push notification tap navigates correctly (background + killed states)
- [ ] Offline mode tested — cached data shown, mutations queued
- [ ] Dio interceptor handles 401 → refresh token → retry
- [ ] CI builds on every PR (lint + test + build). Integration tests nightly.
- [ ] `pubspec.lock` committed. CI and dev use same Flutter version (fvm).

---

## Package Quick Reference

| Category | Recommended | Alternative |
|----------|------------|-------------|
| **State** | `riverpod` + `riverpod_generator` | `flutter_bloc` |
| **Routing** | `go_router` | `auto_route` |
| **Network** | `dio` + `retrofit` | `http` (simple cases) |
| **Models** | `freezed` + `json_serializable` | `json_annotation` only |
| **Storage (SQL)** | `drift` | `sqflite` |
| **Storage (KV)** | `shared_preferences`, `flutter_secure_storage` | `hive` |
| **Testing** | `mocktail`, `bloc_test`, `golden_toolkit` | `mockito` |
| **E2E** | `patrol` | `maestro`, `integration_test` |
| **UI** | `responsive_framework`, `cached_network_image` | `flutter_screenutil` |
| **Platform** | `permission_handler`, `firebase_messaging` | `app_links`, `local_auth` |
| **Build** | `fastlane`, `codemagic` | `bitrise`, GitHub Actions |
| **Code Gen** | `build_runner`, `riverpod_generator` | `injectable` (for get_it) |
