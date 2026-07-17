# Kotlin Android (Jetpack Compose) Checklist

> Kotlin Android + Jetpack Compose specific companion to the general [Mobile Checklist](mobile.md).
> Covers Kotlin 2.x, Jetpack Compose, Material 3, Hilt, Navigation Compose, Coroutines + Flow.
> Last updated: 2026-07-17

---

## 1. Project Setup & Tooling

- [ ] **Kotlin 2.x + Compose Compiler** — Compose Compiler is merged into the Kotlin compiler in 2.0+. No separate `composeCompiler` dependency. Just set `composeOptions` in `build.gradle.kts`. K2 compiler is the default — faster compilation, better type inference.
- [ ] **Gradle Version Catalog** — `gradle/libs.versions.toml` for all dependency versions. Single source of truth across modules:

```toml
[versions]
kotlin = "2.1.0"
compose-bom = "2026.01.00"
hilt = "2.52"
room = "2.7.0"

[libraries]
compose-bom = { group = "androidx.compose", name = "compose-bom", version.ref = "compose-bom" }
hilt-android = { group = "com.google.dagger", name = "hilt-android", version.ref = "hilt" }

[plugins]
kotlin-android = { id = "org.jetbrains.kotlin.android", version.ref = "kotlin" }
compose-compiler = { id = "org.jetbrains.kotlin.plugin.compose", version.ref = "kotlin" }
hilt = { id = "com.google.dagger.hilt.android", version.ref = "hilt" }
ksp = { id = "com.google.devtools.ksp", version = "2.1.0-1.0.29" }
```

- [ ] **Gradle Convention Plugins** — Reuse build logic across modules via `build-logic/` module. Define common Android config, Compose config, Hilt config as precompiled script plugins. No copy-pasting `build.gradle.kts` across 20 modules.
- [ ] **AGP latest stable** — Android Gradle Plugin. Check compatibility matrix with Kotlin and Compose versions.
- [ ] **Lint: detekt + compose-rules** — `detekt` for Kotlin code quality. `compose-rules` (Twitter/Mrmans0n) for Compose-specific lint (missing modifiers, unstable params, unnecessary recompositions). Add custom rules for team conventions.
- [ ] **Compiler warnings as errors** — `allWarningsAsErrors = true` in `kotlinOptions`. Catches nullability issues, deprecations, unused imports at compile time. The Kotlin equivalent of strict mode.
- [ ] **Build flavors** — Dev, staging, prod with different `applicationIdSuffix`, `buildConfigField` for API URLs, app names. All installable simultaneously:

```kotlin
flavorDimensions += "environment"
productFlavors {
    create("dev") { applicationIdSuffix = ".dev"; buildConfigField("String", "BASE_URL", "\"https://api-dev.example.com\"") }
    create("staging") { applicationIdSuffix = ".staging"; buildConfigField("String", "BASE_URL", "\"https://api-staging.example.com\"") }
    create("prod") { buildConfigField("String", "BASE_URL", "\"https://api.example.com\"") }
}
```

- [ ] **KSP over KAPT** — Kotlin Symbol Processing is faster, supports K2 compiler, incremental by default. Use for Hilt, Room, Moshi. KAPT is deprecated — migrate if still using it.

## 2. Architecture & Module Structure

- [ ] **Multi-module architecture** — Modularize by feature for build speed, enforced encapsulation, and team scalability. Recommended structure:

```
:app              ← Application module (entry point, DI graph, navigation host)
:core:network     ← Retrofit/OkHttp setup, interceptors
:core:database    ← Room database, DAOs
:core:ui          ← Shared composables, theme, design system
:core:common      ← Utilities, extensions, constants
:core:domain      ← Use case interfaces, domain models (pure Kotlin, no Android deps)
:feature:auth     ← Login, registration, token management
:feature:home     ← Home screen, dashboard
:feature:settings ← User preferences, profile
```

- [ ] **MVVM + Clean Architecture** — ViewModel → UseCase → Repository → DataSource. Each layer has clear responsibility. Domain layer is pure Kotlin (no Android imports).
- [ ] **Unidirectional Data Flow (UDF)** — UI emits events → ViewModel processes → UI observes state. No two-way binding. State flows one direction. UI is a function of state.
- [ ] **Repository pattern** — Interface in domain module, implementation in data module. ViewModel never knows about Retrofit/Room directly:

```kotlin
// :core:domain
interface UserRepository {
    fun getUsers(): Flow<List<User>>
    suspend fun refreshUsers()
}

// :feature:home (data layer)
class UserRepositoryImpl @Inject constructor(
    private val api: UserApi,
    private val dao: UserDao,
) : UserRepository {
    override fun getUsers(): Flow<List<User>> = dao.observeAll().map { it.toDomain() }
    override suspend fun refreshUsers() { dao.insertAll(api.getUsers().toEntity()) }
}
```

- [ ] **Dependency injection: Hilt** — `@HiltViewModel`, `@Inject constructor`, `@Module`, `@Provides`, `@Binds`. Compile-time verified. No runtime reflection for DI graph resolution.

## 3. Jetpack Compose UI

- [ ] **Compose-first** — No XML layouts. All UI in `@Composable` functions. Single Activity architecture with Compose navigation. Fragments are legacy.
- [ ] **State hoisting** — Stateless composables receive state + event callbacks. Screen-level composables connect to ViewModel. Reusable composables know nothing about ViewModels:

```kotlin
@Composable
fun HomeScreen(viewModel: HomeViewModel = hiltViewModel()) {
    val uiState by viewModel.uiState.collectAsStateWithLifecycle()
    HomeContent(
        state = uiState,
        onItemClick = viewModel::onItemClick,
        onRefresh = viewModel::refresh,
    )
}

@Composable
fun HomeContent(
    state: HomeUiState,
    onItemClick: (String) -> Unit,
    onRefresh: () -> Unit,
    modifier: Modifier = Modifier,
) {
    // Pure UI — easy to preview, test, reuse
}
```

- [ ] **Material 3 (Material You)** — `MaterialTheme` with dynamic color support (Android 12+ wallpaper-based color). Custom color scheme fallback for older devices:

```kotlin
val colorScheme = if (Build.VERSION.SDK_INT >= 31) {
    dynamicLightColorScheme(context)
} else {
    lightColorScheme(primary = Purple40, secondary = PurpleGrey40)
}
MaterialTheme(colorScheme = colorScheme) { /* content */ }
```

- [ ] **Recomposition awareness** — Avoid unnecessary recompositions. Use `remember`, `derivedStateOf` for computed values, `key()` for list identity. Use Layout Inspector to count recompositions.
- [ ] **Stability annotations** — `@Stable`, `@Immutable` for classes passed to composables. Or use `data class` (all val properties = stable). Unstable params cause recomposition on every parent recomposition.
- [ ] **Modifier conventions** — Always accept `Modifier` as the first optional parameter after required params. Chain modifiers in the caller. Never hardcode size/padding inside reusable composables.
- [ ] **@Preview** — Multiple preview configs (dark mode, large font, different devices). Use `@PreviewLightDark`, `@PreviewFontScale`, `@PreviewScreenSizes` for comprehensive coverage.

## 4. State Management

- [ ] **ViewModel + StateFlow** — ViewModel holds `MutableStateFlow<UiState>`, exposes `StateFlow<UiState>`. UI collects with lifecycle awareness:

```kotlin
@HiltViewModel
class HomeViewModel @Inject constructor(
    private val getItemsUseCase: GetItemsUseCase,
) : ViewModel() {
    private val _uiState = MutableStateFlow<HomeUiState>(HomeUiState.Loading)
    val uiState: StateFlow<HomeUiState> = _uiState.asStateFlow()

    init { loadItems() }

    private fun loadItems() {
        viewModelScope.launch {
            getItemsUseCase()
                .onSuccess { _uiState.value = HomeUiState.Success(it) }
                .onFailure { _uiState.value = HomeUiState.Error(it.message ?: "Unknown error") }
        }
    }
}
```

- [ ] **UiState sealed interface** — Exhaustive `when` expressions. Compile-time safety for all states:

```kotlin
sealed interface HomeUiState {
    data object Loading : HomeUiState
    data class Success(val items: List<Item>) : HomeUiState
    data class Error(val message: String) : HomeUiState
}
```

- [ ] **One-time events** — `Channel<UiEvent>` or `SharedFlow` (replay=0) for navigation, snackbar, toast. Never model one-time events as state (double-trigger on config change).
- [ ] **collectAsStateWithLifecycle()** — Lifecycle-aware collection in Compose. Stops collection when UI is in background. Prevents wasted work and stale emissions.
- [ ] **SavedStateHandle** — Survives process death. Use for user input (search query, form data, scroll position). `SavedStateHandle` auto-injected by Hilt into ViewModel.
- [ ] **Never expose MutableStateFlow** — Public API is always `StateFlow` (read-only). Encapsulate mutation inside ViewModel.

## 5. Navigation

- [ ] **Navigation Compose (type-safe, 2.8+)** — Define routes as Kotlin serializable classes. Compile-time safe arguments. No string-based routes:

```kotlin
@Serializable data object Home
@Serializable data class Detail(val id: String)
@Serializable data object Settings

NavHost(navController, startDestination = Home) {
    composable<Home> {
        HomeScreen(onNavigateToDetail = { navController.navigate(Detail(it)) })
    }
    composable<Detail> { backStackEntry ->
        val route = backStackEntry.toRoute<Detail>()
        DetailScreen(id = route.id)
    }
    composable<Settings> { SettingsScreen() }
}
```

- [ ] **Auth guard** — Check auth state at NavHost level. Redirect unauthenticated users to login. Single enforcement point.
- [ ] **Deep linking** — Configure in `AndroidManifest.xml` intent filters + `deepLinks` in route definitions. Handle both custom scheme (`myapp://`) and verified App Links (`https://`).
- [ ] **Bottom navigation with multiple back stacks** — `saveState = true`, `restoreState = true` on `navigate()`. Each tab keeps its own navigation history:

```kotlin
NavigationBar {
    tabs.forEach { tab ->
        NavigationBarItem(
            selected = currentRoute == tab.route,
            onClick = {
                navController.navigate(tab.route) {
                    popUpTo(navController.graph.findStartDestination().id) { saveState = true }
                    launchSingleTop = true
                    restoreState = true
                }
            },
        )
    }
}
```

- [ ] **Nested navigation graphs** — Per feature module. Each feature owns its own navigation subgraph. App module composes them into the root NavHost.

## 6. Networking

- [ ] **Retrofit + OkHttp + Kotlin Serialization** — Type-safe HTTP client. `kotlinx.serialization` for JSON (faster than Gson, multiplatform ready). Alternative: Moshi (reflection-free with code gen).
- [ ] **OkHttp Interceptors** — Auth token injection, logging, retry logic. `Authenticator` for transparent token refresh:

```kotlin
class AuthInterceptor @Inject constructor(
    private val tokenManager: TokenManager,
) : Interceptor {
    override fun intercept(chain: Interceptor.Chain): Response {
        val token = tokenManager.accessToken
        val request = chain.request().newBuilder()
            .apply { token?.let { header("Authorization", "Bearer $it") } }
            .build()
        return chain.proceed(request)
    }
}

class TokenAuthenticator @Inject constructor(
    private val tokenManager: TokenManager,
    private val authApi: Lazy<AuthApi>,
) : Authenticator {
    override fun authenticate(route: Route?, response: Response): Request? {
        val newToken = runBlocking { tokenManager.refreshToken(authApi.get()) } ?: return null
        return response.request.newBuilder()
            .header("Authorization", "Bearer $newToken")
            .build()
    }
}
```

- [ ] **Coroutines everywhere** — All network calls are `suspend` functions. No callbacks, no RxJava. Structured concurrency via `viewModelScope` and `CoroutineScope`.
- [ ] **Error handling** — Sealed `Result<T>` class or `kotlin.Result`. Map HTTP errors to domain errors. Never expose Retrofit exceptions to UI layer.
- [ ] **Connectivity monitoring** — `ConnectivityManager` + `NetworkCallback`. Expose as `Flow<Boolean>`. Show offline banner in UI. Queue requests when offline.

## 7. Data & Storage

- [ ] **Room** — Type-safe SQLite with compile-time query verification. Reactive via `Flow<List<T>>`. Migration support with `AutoMigration` or manual `Migration`:

```kotlin
@Dao
interface ItemDao {
    @Query("SELECT * FROM items ORDER BY created_at DESC")
    fun observeAll(): Flow<List<ItemEntity>>

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertAll(items: List<ItemEntity>)

    @Query("DELETE FROM items")
    suspend fun deleteAll()
}
```

- [ ] **DataStore** — Preferences DataStore for key-value settings (replaces SharedPreferences). Proto DataStore for typed structured data. Both are coroutine-based and safe from UI thread.
- [ ] **EncryptedSharedPreferences** — AndroidX Security Crypto for sensitive key-value storage (tokens, credentials). Backed by Android Keystore system.
- [ ] **Offline-first repository** — Room is source of truth. Retrofit syncs with server. Show cached data immediately, refresh in background:

```kotlin
fun getItems(): Flow<List<Item>> = itemDao.observeAll()
    .map { entities -> entities.map { it.toDomain() } }
    .onStart { refreshFromNetwork() }

private suspend fun refreshFromNetwork() {
    runCatching { api.getItems() }
        .onSuccess { itemDao.insertAll(it.map { dto -> dto.toEntity() }) }
}
```

- [ ] **WorkManager** — Guaranteed background execution for sync, upload queues, periodic cleanup. Survives app kill. Respects battery/network constraints.

## 8. Testing

- [ ] **Unit tests** — JUnit 5 + MockK + Turbine. Test ViewModels, UseCases, Repositories in isolation:

```kotlin
@Test
fun `load items emits loading then success`() = runTest {
    val fakeRepository = FakeItemRepository(items = testItems)
    val viewModel = HomeViewModel(GetItemsUseCase(fakeRepository))

    viewModel.uiState.test {
        assertEquals(HomeUiState.Loading, awaitItem())
        assertEquals(HomeUiState.Success(testItems), awaitItem())
    }
}
```

- [ ] **Compose UI tests** — `ComposeTestRule` with semantics-based assertions. No flaky pixel matching:

```kotlin
@get:Rule val composeTestRule = createComposeRule()

@Test
fun `shows items when state is success`() {
    composeTestRule.setContent {
        HomeContent(state = HomeUiState.Success(testItems), onItemClick = {}, onRefresh = {})
    }
    composeTestRule.onNodeWithText("Test Item 1").assertIsDisplayed()
}
```

- [ ] **Screenshot tests** — Roborazzi (JVM-based, fast, no emulator) or Paparazzi. Catch visual regressions in CI without device.
- [ ] **Integration tests** — Hilt test modules (`@TestInstallIn`) + fake repositories. Real DI graph with swapped implementations.
- [ ] **E2E tests** — Maestro (YAML-based, cross-platform, easy to write) or Compose UI Test with `createAndroidComposeRule` on real device.
- [ ] **CI strategy** — Unit + Compose tests on every PR (fast). Maestro E2E nightly (slower, real device/emulator). Screenshot tests on merge to main.

## 9. Performance

- [ ] **Baseline Profiles** — Pre-compile hot paths (startup, navigation, scrolling) into AOT code. Generate with Macrobenchmark library. 30-50% faster cold start typical. Ship in AAB.
- [ ] **R8 full mode** — Shrinking + obfuscation + optimization. Enable `isMinifyEnabled = true` + `proguardFiles`. Add keep rules for Hilt, Retrofit, Kotlin Serialization (reflection-based).
- [ ] **Compose Compiler reports** — Generate stability reports (`-P plugin:androidx.compose.compiler.plugins.kotlin:reportsDestination`). Identify unstable classes causing unnecessary recompositions.
- [ ] **LazyColumn / LazyRow** — `key()` for stable item identity. `contentType` for heterogeneous lists (enables view recycling). Never use `Column` for dynamic lists.
- [ ] **Coil** — Compose-native image loading (coroutine-based). Disk + memory cache. Automatic resize to display size. `AsyncImage` composable. Crossfade transitions.
- [ ] **Startup optimization** — Minimize `Application.onCreate()` work. Use AndroidX App Startup library for deferred initialization. Lazy-inject non-critical dependencies.
- [ ] **StrictMode in debug** — Detect disk reads/writes and network calls on main thread. Detect leaked Closeables. Enable in debug builds only:

```kotlin
if (BuildConfig.DEBUG) {
    StrictMode.setThreadPolicy(StrictMode.ThreadPolicy.Builder().detectAll().penaltyLog().build())
    StrictMode.setVmPolicy(StrictMode.VmPolicy.Builder().detectAll().penaltyLog().build())
}
```

## 10. Security

- [ ] **EncryptedSharedPreferences** — Tokens, API keys, user secrets. Never plain SharedPreferences for sensitive data. Backed by Android Keystore:

```kotlin
val prefs = EncryptedSharedPreferences.create(
    context, "secure_prefs",
    MasterKey.Builder(context).setKeyScheme(MasterKey.KeyScheme.AES256_GCM).build(),
    EncryptedSharedPreferences.PrefKeyEncryptionScheme.AES256_SIV,
    EncryptedSharedPreferences.PrefValueEncryptionScheme.AES256_GCM,
)
```

- [ ] **Network security config** — Certificate pinning in `res/xml/network_security_config.xml`. Pin public key hash (survives cert rotation). Backup pins required:

```xml
<network-security-config>
    <domain-config cleartextTrafficPermitted="false">
        <domain includeSubdomains="true">api.example.com</domain>
        <pin-set expiration="2027-01-01">
            <pin digest="SHA-256">base64EncodedPublicKeyHash=</pin>
            <pin digest="SHA-256">backupPinHash=</pin>
        </pin-set>
    </domain-config>
</network-security-config>
```

- [ ] **R8 obfuscation** — Release builds have obfuscated class/method names. No plain names in APK. Upload `mapping.txt` to Crashlytics for symbolication.
- [ ] **No secrets in source** — Use `BuildConfig` fields injected from `local.properties` (gitignored) or CI environment secrets. Never commit API keys.
- [ ] **Play Integrity API** — Device attestation for high-security apps (banking, fintech). Verifies device is genuine, not rooted, app is legitimate.
- [ ] **FLAG_SECURE** — Prevent screenshots/screen recording on sensitive screens (login, payment, OTP):

```kotlin
DisposableEffect(Unit) {
    val window = (context as Activity).window
    window.setFlags(WindowManager.LayoutParams.FLAG_SECURE, WindowManager.LayoutParams.FLAG_SECURE)
    onDispose { window.clearFlags(WindowManager.LayoutParams.FLAG_SECURE) }
}
```

- [ ] **BiometricPrompt** — Fingerprint / face authentication for local unlock. Fallback to device credential. Use `androidx.biometric` library.

## 11. Build & Release

- [ ] **Signing** — Upload keystore stored in CI secrets (never in repo). Google Play App Signing (recommended): Google manages the signing key, you hold the upload key.
- [ ] **CI/CD** — GitHub Actions + Gradle. Build on every PR, deploy to internal track on merge to main. Cache Gradle dependencies and build outputs.
- [ ] **Fastlane + supply** — Automated Play Store deployment. Metadata management (screenshots, descriptions). `fastlane supply --aab app-release.aab --track internal`.
- [ ] **App Bundle (.aab)** — Always ship AAB, not APK. Dynamic delivery: users download only what their device needs (density, ABI, language). Smaller installs.
- [ ] **Versioning** — `versionCode`: monotonically increasing integer (CI build number). `versionName`: semver for display (`1.2.3`). Never reuse `versionCode`.
- [ ] **Beta distribution** — Firebase App Distribution for quick internal testing. Play Store internal/closed testing tracks for broader beta.
- [ ] **Staged rollout** — 5% → 25% → 50% → 100% on Play Store. Monitor crash rate, ANR rate, user feedback at each stage. Halt and rollback on regression.

## 12. Android-Specific Concerns

- [ ] **Lifecycle awareness** — Collect flows in `repeatOnLifecycle(Lifecycle.State.STARTED)`. In Compose: `collectAsStateWithLifecycle()` handles this automatically. No background leaks.
- [ ] **Process death** — `SavedStateHandle` in ViewModel for surviving process death. Test with "Don't keep activities" developer option enabled. Critical user data must survive.
- [ ] **Runtime permissions** — Request at point of use (when camera is needed, not on startup). Handle "don't ask again" gracefully — show rationale, link to settings:

```kotlin
val permissionLauncher = rememberLauncherForActivityResult(
    ActivityResultContracts.RequestPermission()
) { granted -> if (granted) onPermissionGranted() else onPermissionDenied() }
```

- [ ] **Adaptive UI** — Support foldables, tablets with Window Size Classes (Compact, Medium, Expanded). `calculateWindowSizeClass()` to determine layout:

```kotlin
val windowSizeClass = calculateWindowSizeClass(this)
when (windowSizeClass.widthSizeClass) {
    WindowWidthSizeClass.Compact -> PhoneLayout()
    WindowWidthSizeClass.Medium -> TabletLayout()
    WindowWidthSizeClass.Expanded -> DesktopLayout()
}
```

- [ ] **Predictive back gesture (Android 14+)** — Opt-in with `android:enableOnBackInvokedCallback="true"` in manifest. Compose Navigation handles automatically. Shows preview of previous screen during swipe.
- [ ] **Edge-to-edge (Android 15+)** — `enableEdgeToEdge()` in Activity. Handle system bar insets in Compose with `WindowInsets.systemBars`, `Modifier.windowInsetsPadding()`. No content behind nav bar.

---

## Quick Sanity Check Before Launch

- [ ] `allWarningsAsErrors = true` in Kotlin compiler options
- [ ] App runs on minimum supported API level without crash
- [ ] KSP used (not KAPT) for all annotation processors
- [ ] `EncryptedSharedPreferences` for tokens, not plain `SharedPreferences`
- [ ] R8 + obfuscation enabled for release builds. `mapping.txt` uploaded to Crashlytics.
- [ ] Baseline Profiles generated and included in AAB
- [ ] `LazyColumn` with `key()` for all dynamic lists
- [ ] `collectAsStateWithLifecycle()` used (not `collectAsState()`)
- [ ] No `MutableStateFlow` exposed from ViewModel — only `StateFlow`
- [ ] Deep links tested on cold start (app killed → link opens correct screen)
- [ ] Process death tested with "Don't keep activities" enabled
- [ ] Offline mode: cached data shown, network errors handled gracefully
- [ ] Auth interceptor handles 401 → refresh token → retry request
- [ ] CI runs lint (detekt) + unit tests + Compose tests on every PR
- [ ] `libs.versions.toml` has all dependencies. No hardcoded versions in `build.gradle.kts`.

---

## Library Quick Reference

| Category | Recommended | Alternative |
|----------|------------|-------------|
| **DI** | Hilt (`dagger-hilt`) | Koin (lightweight, no code gen) |
| **State** | ViewModel + StateFlow | Orbit MVI, Molecule |
| **Navigation** | Navigation Compose (type-safe 2.8+) | Voyager (multiplatform) |
| **Network** | Retrofit + OkHttp | Ktor Client (multiplatform) |
| **Serialization** | Kotlin Serialization (`kotlinx.serialization`) | Moshi (reflection-free) |
| **Storage (SQL)** | Room | SQLDelight (multiplatform) |
| **Storage (KV)** | DataStore (Preferences/Proto) | EncryptedSharedPreferences |
| **Image** | Coil 3 (Compose-native) | Glide (legacy, still maintained) |
| **Testing (Unit)** | JUnit 5 + MockK + Turbine | Kotest |
| **Testing (UI)** | Compose UI Test | Espresso (legacy views) |
| **Testing (Screenshot)** | Roborazzi | Paparazzi |
| **Testing (E2E)** | Maestro | UI Automator |
| **Lint** | detekt + compose-rules | ktlint (formatting only) |
| **Build** | Gradle Version Catalog + Convention Plugins | — |
| **CI/CD** | GitHub Actions + Fastlane | Bitrise, CircleCI |
| **Crash Reporting** | Firebase Crashlytics | Sentry |
| **Analytics** | Firebase Analytics | Mixpanel, Amplitude |
| **Background Work** | WorkManager | — |
| **Permissions** | Accompanist Permissions (Compose) | ActivityResult API directly |
