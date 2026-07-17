# Mobile App Checklist

> Framework-agnostic checklist for production mobile applications.
> Flutter, React Native, Kotlin (Jetpack Compose), Swift (SwiftUI), or KMP — the principles are the same. The tools change.
> Last updated: 2026-07-17

---

## 1. Platform & Project Setup

- [ ] **Target platforms decided** — iOS only, Android only, or both. Cross-platform (Flutter, React Native, KMP) or native per platform. Decision drives everything: team size, timeline, UX fidelity.
- [ ] **Minimum OS versions** — iOS 16+ (drops ~5% of users per version dropped), Android API 26+ (Android 8.0). Balance reach vs maintenance burden. Check analytics if existing app.
- [ ] **Package manager locked** — `pubspec.lock` (Flutter), `yarn.lock`/`package-lock.json` (RN), `gradle.lockfile` (Android), `Package.resolved` (Swift). Commit lockfiles. CI and dev must match.
- [ ] **Project structure** — Feature-based or layer-based. Consistent module boundaries. Shared code separated from platform-specific code. No god files.
- [ ] **Strict mode enabled** — TypeScript strict (RN), Dart `analysis_options.yaml` strict (Flutter), Kotlin compiler warnings as errors, Swift strict concurrency checking. Catch bugs at compile time.
- [ ] **Linting & formatting** — `dart analyze` + `dart format` (Flutter), ESLint + Prettier (RN), ktlint/detekt (Kotlin), SwiftLint (Swift). Pre-commit hooks. CI blocks on lint failures.
- [ ] **Environment configuration** — Dev, staging, production configs separated. Flavors (Android) / Schemes (iOS) / `--dart-define` (Flutter). No hard-coded URLs or keys per environment.
- [ ] **Dependency management strategy** — Pin exact versions for production deps. Renovate/Dependabot for automated updates. Audit native transitive dependencies (CocoaPods subspecs, Gradle transitive deps). Know what's in your binary.
- [ ] **Dev tooling** — Hot reload/restart configured. Flipper/DevTools for debugging. Network inspector. Layout inspector. Performance overlay available in debug builds. New developer productive in < 30 minutes.

## 2. Navigation & Routing

- [ ] **Navigation library chosen** — GoRouter/auto_route (Flutter), React Navigation (RN), Navigation Component (Jetpack Compose), NavigationStack (SwiftUI). Don't roll your own.
- [ ] **Stack navigation** — Push/pop semantics. Predictable back behavior. Avoid deeply nested stacks (> 5 levels deep = UX smell).
- [ ] **Tab navigation** — Bottom tabs for top-level sections. Each tab maintains its own navigation stack. Tab state preserved on switch.
- [ ] **Deep linking** — `myapp://path/to/screen` custom scheme + `https://myapp.com/path` universal links. Handle gracefully when target screen requires auth or data loading.
- [ ] **Universal links / App Links** — iOS: Associated Domains + `apple-app-site-association`. Android: `assetlinks.json` + intent filters. Verified domain ownership. Test with real devices.
- [ ] **Back button handling** — Android hardware back button + gesture. iOS swipe-to-go-back. Custom back behavior for modals, multi-step flows, unsaved changes confirmation.
- [ ] **Navigation state persistence** — Restore navigation stack after app kill (optional, depends on UX). At minimum: return to correct tab after backgrounding.
- [ ] **Route guards / auth gates** — Unauthenticated users redirected to login. Role-based screen access. One place to enforce, not scattered in every screen.
- [ ] **Modal & sheet navigation** — Bottom sheets, full-screen modals, dialogs as part of navigation stack. Proper dismiss handling. Pass data back to parent on dismiss.

## 3. UI & Styling

- [ ] **Responsive layouts** — Phone and tablet support. Adaptive layouts (not just stretched phone UI on iPad). Breakpoints or `LayoutBuilder`/`MediaQuery` equivalent.
- [ ] **Safe area handling** — Notch, status bar, home indicator, rounded corners. `SafeAreaView` (RN), `SafeArea` (Flutter), `safeAreaInset` (SwiftUI). Never assume screen geometry.
- [ ] **Platform-specific UI** — Material Design on Android, Human Interface Guidelines on iOS. Or consistent cross-platform design — but be intentional. Platform users expect platform conventions (back button position, scroll physics, date pickers).
- [ ] **Dark mode** — System preference detection + manual override. Persist user choice. Test every screen in both modes. Not just inverted colors — a designed dark palette.
- [ ] **Dynamic type / font scaling** — Respect system font size settings. Test at largest accessibility size. Layouts must not break. Text must not truncate critical info.
- [ ] **Animations at 60fps** — Use GPU-accelerated properties (transform, opacity). No layout recalculation during animation. `Animated` API (RN), implicit/explicit animations (Flutter), `withAnimation` (SwiftUI). Profile with DevTools.
- [ ] **Gesture handling** — Swipe, pinch, long press, drag. Conflict resolution between nested gestures. Platform-standard gestures (swipe-to-delete, pull-to-refresh, swipe-back).
- [ ] **Skeleton screens over spinners** — Show content shape while loading. Users perceive faster load times. Spinners = uncertainty; skeletons = progress.
- [ ] **Haptic feedback** — Subtle vibration on key interactions (success, error, selection). UIImpactFeedbackGenerator (iOS), VibrationEffect (Android). Don't overuse — meaningful, not annoying.
- [ ] **Orientation handling** — Lock to portrait if app isn't designed for landscape. If supporting both: test every screen in landscape. Tablet apps should support both orientations.
- [ ] **Keyboard handling** — Keyboard doesn't obscure input fields (scroll into view). Dismiss keyboard on tap outside. `keyboardDismissMode` for scroll views. Proper `inputType` / keyboard type per field (email, number, phone).

## 4. State Management

- [ ] **Local component state** — `useState`/`setState` for UI-only state (toggle open, text input value, animation state). Keep it where it's used.
- [ ] **Global app state** — Zustand/Jotai (RN), Riverpod/Bloc/Provider (Flutter), StateFlow/ViewModel (Kotlin), `@Observable` (SwiftUI). Single source of truth for cross-screen state.
- [ ] **Server state** — TanStack Query (RN), Riverpod AsyncValue (Flutter), repository pattern with Flow (Kotlin). Separate from UI state. Cache, refetch, invalidate.
- [ ] **Offline state** — Queue mutations when offline. Replay when connectivity returns. UI reflects pending state ("Saving..." indicator). Conflict resolution strategy defined.
- [ ] **State persistence across app kills** — Critical state survives process death. SharedPreferences/UserDefaults for simple data. SQLite/Hive for complex state. Don't lose user's draft.
- [ ] **Hydration** — Restore persisted state on app launch without flash of empty state. Show cached data immediately, refresh in background.
- [ ] **State scoping** — State lives as close to where it's used as possible. Screen-level state dies with the screen. App-level state survives navigation. Don't put everything in global state — most state is local.

## 5. Networking & Data

- [ ] **HTTP client** — Dio (Flutter), Axios/fetch (RN), Retrofit/Ktor (Kotlin), URLSession/Alamofire (Swift). Interceptors for auth headers, logging, retry.
- [ ] **Offline-first architecture** — Local DB is source of truth. Sync with server in background. App works without network. Not "online-only with error messages."
- [ ] **Caching strategy** — HTTP cache headers respected. Application-level cache (in-memory + disk). Stale-while-revalidate pattern. Cache invalidation on mutations.
- [ ] **Retry with connectivity detection** — Detect network state changes. Auto-retry queued requests when connectivity returns. Exponential backoff + jitter for server errors.
- [ ] **Background sync** — Sync data when app is backgrounded (WorkManager on Android, BGTaskScheduler on iOS). Respect battery and data constraints. Don't sync everything — sync what matters.
- [ ] **Pagination** — Cursor-based for feeds/infinite scroll. Load more on scroll threshold (not at absolute bottom). Show loading indicator. Handle "end of list" state.
- [ ] **Optimistic updates** — Update UI immediately on user action. Rollback on server error. Feels instant. Essential for likes, saves, toggles, simple edits.
- [ ] **Request cancellation** — Cancel in-flight requests when user navigates away. Prevent stale responses updating wrong screen. AbortController (RN), CancelToken (Dio), Job cancellation (coroutines).
- [ ] **API versioning awareness** — Mobile apps can't be force-updated instantly. Support multiple API versions simultaneously on the backend. Graceful degradation when server returns unknown fields or deprecation notices.

## 6. Offline & Storage

- [ ] **Local database** — SQLite/Drift (Flutter), WatermelonDB/Realm (RN), Room (Kotlin), SwiftData/CoreData (Swift). For structured relational data. Not SharedPreferences for lists.
- [ ] **Secure storage** — iOS Keychain, Android Keystore/EncryptedSharedPreferences. For tokens, credentials, sensitive user data. Never plain-text storage for secrets.
- [ ] **File system management** — App documents directory vs cache directory. Know the difference: documents are backed up, cache is purgeable by OS. Store user files in documents, generated data in cache.
- [ ] **Cache size limits** — Set max cache size. LRU eviction for images and network cache. Monitor storage usage. iOS can purge cache directory at any time — don't store important data there.
- [ ] **Data migration between app versions** — Schema versioning for local DB. Migration scripts run on app update. Test upgrade from oldest supported version. Never lose user data on update.
- [ ] **Sync conflict resolution** — Last-write-wins (simple), merge (complex), or user-decides (safest). Document the strategy. Test with concurrent edits from multiple devices.
- [ ] **Data encryption at rest** — Encrypt sensitive local database content. SQLCipher, Realm encryption, or platform-level file encryption. Tokens and credentials always in secure storage, never in plain SQLite.

## 7. Authentication

- [ ] **Biometrics** — Face ID, Touch ID, fingerprint. Use as convenience unlock, not sole auth factor. Fallback to PIN/password. LocalAuthentication (iOS), BiometricPrompt (Android).
- [ ] **OAuth2 + PKCE** — Authorization code flow with PKCE for mobile (not implicit flow). Use system browser for login (ASWebAuthenticationSession / Chrome Custom Tabs), not embedded WebView.
- [ ] **Token storage** — Access token in secure storage (Keychain/Keystore), not AsyncStorage/SharedPreferences. Refresh token with higher security. Never log tokens.
- [ ] **Session management** — Token refresh before expiry (proactive, not reactive). Handle 401 globally — refresh token, retry request, or redirect to login. Concurrent request queue during refresh.
- [ ] **Auto-logout on inactivity** — Configurable timeout for sensitive apps (banking, health). Background timer. Show warning before logout. Clear sensitive cached data on logout.
- [ ] **Deep link auth callbacks** — OAuth redirect back to app via custom scheme or universal link. Validate state parameter. Handle edge case: user killed app during auth flow.
- [ ] **Multi-device session awareness** — User logged in on multiple devices. Logout on one device can optionally revoke others. Show active sessions. Token revocation propagates via push or next API call.

## 8. Push Notifications

- [ ] **FCM / APNs setup** — Firebase Cloud Messaging (both platforms or Android-only), APNs (iOS). Server sends to device token. Token refresh handling (tokens change — update server).
- [ ] **Permission request timing** — Never on first launch. Show value proposition first ("Get notified when your order ships"). Pre-permission screen explaining benefits before OS prompt. Handle "denied" gracefully.
- [ ] **Notification channels / categories** — Android: channels (user can mute per channel). iOS: categories with actions. Group by type: messages, promotions, system alerts. Let users control granularity.
- [ ] **Background vs foreground handling** — Foreground: in-app banner or toast (not OS notification). Background: standard notification. App killed: standard notification + handle tap to navigate.
- [ ] **Rich notifications** — Images, action buttons, expandable content. iOS: Notification Service Extension for media. Android: BigPictureStyle, BigTextStyle. Keep payload under size limits.
- [ ] **Deep link from notification** — Tap notification → navigate to relevant screen. Handle: app was killed, app was backgrounded, app was in foreground. Pass data via notification payload.
- [ ] **Silent / data notifications** — Background data sync triggered by server. No user-visible notification. iOS: content-available. Android: data-only message. Rate-limited by OS.
- [ ] **Notification analytics** — Track: received, displayed, tapped, dismissed. Measure tap-through rate per notification type. A/B test notification copy and timing.

## 9. Performance

- [ ] **Startup time** — Cold start < 2 seconds to interactive content. Measure from process start to first meaningful frame. Defer non-critical initialization. Splash screen hides loading, doesn't excuse slow startup.
- [ ] **List virtualization** — RecyclerView (Android), FlatList (RN), ListView.builder (Flutter). Only render visible items + buffer. No `Column` with 1000 children. Constant item height = better performance.
- [ ] **Image optimization** — Resize to display size (don't decode 4000px image for 200px thumbnail). Cache aggressively (memory + disk). Progressive loading (blur placeholder → full image). WebP format. Libraries: cached_network_image (Flutter), FastImage (RN).
- [ ] **Memory management** — Monitor memory usage in profiler. Dispose controllers, cancel subscriptions, release large objects. Avoid memory leaks from undisposed listeners or retained references.
- [ ] **Battery usage** — No unnecessary background work. Batch network requests. Reduce GPS polling frequency. Dark mode saves battery on OLED. Profile with battery profiling tools.
- [ ] **App size optimization** — Remove unused assets and dependencies. Enable tree-shaking. ProGuard/R8 (Android) to strip unused code. App Thinning (iOS). Deferred components/split APKs for large apps. Target: < 50MB download.
- [ ] **Frame drops & jank** — Profile with DevTools/Instruments/Android Profiler. No heavy computation on UI thread. Offload to isolates (Flutter), native threads, or background workers. 16ms per frame budget.
- [ ] **Network efficiency** — Compress payloads (gzip). Use efficient serialization (protobuf over JSON for high-frequency data). Batch requests where possible. Minimize payload size — mobile networks are unreliable.
- [ ] **Lazy initialization** — Don't initialize all services at startup. Load feature modules on demand. Defer analytics, crash reporting SDK initialization to after first frame.

## 10. Security

- [ ] **Certificate pinning** — Pin server certificate or public key. Prevents MITM even if CA is compromised. Update pins before cert rotation. Have backup pins. Handle pinning failure gracefully.
- [ ] **No secrets in client code** — API keys, private keys, signing secrets — none in the app binary. Reverse engineering extracts everything. Use server-side proxy for sensitive API calls.
- [ ] **Code obfuscation** — ProGuard/R8 (Android), Bitcode (deprecated iOS — use symbol stripping), `--obfuscate` (Flutter). Makes reverse engineering harder. Not impossible — never rely solely on obfuscation.
- [ ] **Jailbreak / root detection** — Detect compromised devices. Decide policy: warn, degrade, or block. Not security — just defense in depth. Determined attackers bypass detection.
- [ ] **Secure data at rest** — Encrypt sensitive local data. Use platform encryption (iOS Data Protection, Android file-based encryption). SQLCipher for encrypted database if needed.
- [ ] **Prevent screenshot in sensitive screens** — `FLAG_SECURE` (Android), `UIScreen.captured` detection (iOS). For banking, health, auth screens. Not bulletproof but raises the bar.
- [ ] **OWASP Mobile Top 10** — Review against current OWASP Mobile checklist. Covers: improper credential usage, inadequate supply chain security, insecure auth, insufficient input validation, insecure communication, inadequate privacy controls, insufficient binary protections, security misconfiguration, insecure data storage, insufficient cryptography.
- [ ] **Runtime application self-protection** — Detect debugger attachment, hooking frameworks (Frida), tampered binaries. For high-security apps (fintech, health). React proportionally.
- [ ] **Clipboard protection** — Clear clipboard after paste timeout for sensitive data (OTP, passwords). Prevent copy on sensitive fields. `secureTextEntry` for password inputs.

## 11. Testing

- [ ] **Unit tests** — Business logic, state management, data transformations. Fast, no device needed. Same test runner as platform: `flutter test`, Jest (RN), JUnit (Kotlin), XCTest (Swift).
- [ ] **Widget / component tests** — Render components in isolation. Verify UI output for given state. Flutter widget tests, React Native Testing Library, Compose UI tests, SwiftUI previews + snapshot.
- [ ] **Integration tests** — Multiple components working together. Navigation flows, data layer + UI. Real (or mocked) API responses. Slower than unit tests, more confidence.
- [ ] **E2E tests** — Full app running on device/emulator. Detox (RN), Maestro (any platform), Patrol (Flutter), XCUITest (iOS), Espresso (Android). Critical user flows: onboarding, login, core feature, purchase.
- [ ] **Snapshot / golden tests** — Visual regression detection. Compare screenshots pixel-by-pixel. Catch unintended UI changes. Flutter golden tests, Percy, Chromatic (for Storybook-like component catalogs).
- [ ] **Device farm testing** — Test on real devices, not just emulators. BrowserStack, Firebase Test Lab, AWS Device Farm, Sauce Labs. Cover: different screen sizes, OS versions, manufacturers (Samsung, Xiaomi, Pixel — Android fragmentation is real).
- [ ] **Performance tests** — Measure startup time, frame rate, memory usage in CI. Catch regressions. Set budgets: cold start < 2s, no frame drops in critical flows, memory < threshold.
- [ ] **Network condition testing** — Test on slow 3G, high latency, packet loss. iOS Network Link Conditioner, Android emulator throttling. App must handle real-world network, not just WiFi in the office.

## 12. Accessibility

- [ ] **VoiceOver / TalkBack support** — Every interactive element has a semantic label. Images have descriptions. Decorative elements are hidden from screen reader. Navigate entire app with screen reader — it must make sense.
- [ ] **Semantic labels** — `Semantics` widget (Flutter), `accessibilityLabel` (RN), `contentDescription` (Android), `accessibilityLabel` (SwiftUI). Not just "button" — "Add item to cart" or "Navigate back."
- [ ] **Minimum touch targets** — 48x48dp minimum (Material guidelines), 44x44pt (Apple HIG). Small icons wrapped in larger touchable area. Fat finger-friendly. No 20px tap targets.
- [ ] **Color contrast** — WCAG AA: 4.5:1 normal text, 3:1 large text. Don't convey information by color alone (add icons, labels). Test with color blindness simulators.
- [ ] **Dynamic type support** — App respects system font size settings (including accessibility sizes). Layouts adapt — no truncated text, no overflowing containers. Test at maximum font size.
- [ ] **Screen reader testing on real device** — Emulator accessibility tools miss real-world issues. Test with VoiceOver on physical iPhone, TalkBack on physical Android. Complete a full user flow.
- [ ] **Focus management** — Logical focus order. Focus moves to new content (modal opens → focus moves inside). Return focus when dismissing (modal closes → focus returns to trigger).
- [ ] **Reduce motion support** — Respect `prefers-reduced-motion` / `accessibilityReduceMotion`. Provide simpler transitions for users with motion sensitivity. Disable parallax, auto-scroll carousels.
- [ ] **Switch control / external keyboard** — App works with iOS Switch Control, Android Switch Access. External Bluetooth keyboard navigates correctly. Critical for motor-impaired users.

## 13. Build & Release

- [ ] **CI/CD pipeline** — Lint → test → build → distribute. Fastlane (iOS + Android automation), Codemagic (Flutter-focused), Bitrise, GitHub Actions, or platform-specific. Automated, not manual builds.
- [ ] **Code signing** — iOS: certificates + provisioning profiles (match/fastlane managed). Android: keystore (secure, backed up, never lost — losing keystore = new app listing). Signing in CI, not on dev machines.
- [ ] **Versioning strategy** — Semver for marketing version (1.2.3). Auto-incrementing build number. Never reuse build numbers. `major.minor.patch` + build: `1.2.3 (456)`.
- [ ] **Beta distribution** — TestFlight (iOS, up to 10k testers), Firebase App Distribution (both platforms), App Center. Internal testing → closed beta → open beta → production.
- [ ] **Staged rollouts** — Google Play: percentage rollout (1% → 10% → 50% → 100%). iOS: phased release (7 days automatic). Monitor crash rate at each stage. Halt on regression.
- [ ] **Multiple build flavors** — Development (debug, verbose logging, mock server), Staging (release build, staging API), Production (release, production API, no debug tools). Separate app IDs so all can be installed simultaneously.
- [ ] **Reproducible builds** — Same commit → same output. Pin all dependency versions. Lock native dependency versions (Podfile.lock, gradle lockfile). No ambient state affecting builds.
- [ ] **Crashlytics / dSYM upload in CI** — Automatically upload debug symbols (dSYMs for iOS, mapping.txt for Android) on every release build. Without this, crash reports are unsymbolicated garbage.
- [ ] **Build time optimization** — Cache dependencies, use build cache (Gradle build cache, CocoaPods cache). Modularize to enable incremental builds. CI build < 15 minutes target.

## 14. App Store Submission

- [ ] **Screenshots per device size** — iPhone 6.7", 6.5", 5.5" (required). iPad 12.9", 11" (if supporting iPad). Android: phone + 7" tablet + 10" tablet. Localized screenshots if multi-language.
- [ ] **App description & keywords** — Clear value proposition in first line. Keywords researched (App Store only — Google Play uses description text). A/B test listing if possible.
- [ ] **Privacy policy** — Required by both stores. URL accessible, not just in-app. Covers: data collected, usage, sharing, retention, deletion. GDPR/CCPA compliance if applicable.
- [ ] **Data safety / App Privacy labels** — Google Play: Data Safety section. iOS: App Privacy "nutrition labels." Declare every data type collected, used, shared. Audit third-party SDKs — they collect data too.
- [ ] **Review guidelines compliance** — Apple is strict: no hidden features, no misleading metadata, no private API usage, proper permission request strings. Google: target API level requirements, Families policy if applicable. Read guidelines before submission.
- [ ] **Age rating** — Accurate content rating questionnaire. Under-rating = rejection. Over-rating = reduced visibility. If user-generated content: higher rating required.
- [ ] **In-app purchases (if applicable)** — StoreKit 2 (iOS), Google Play Billing Library v6+ (Android). Server-side receipt validation. Restore purchases flow. Subscription management (upgrade, downgrade, cancel). Handle edge cases: family sharing, promo codes, grace period.
- [ ] **Permission usage descriptions** — iOS: every permission needs a human-readable description string (camera, location, photos, etc.). Explain WHY, not WHAT. "To scan QR codes for quick login" not "This app needs camera access." Missing or vague = rejection.
- [ ] **Export compliance** — iOS asks about encryption usage. If using HTTPS only (standard), you can mark "No" for export compliance. Custom encryption = additional compliance steps. Get this wrong and your app sits in "waiting for compliance" limbo.

## 15. Post-Launch & Monitoring

- [ ] **Crash reporting** — Sentry, Firebase Crashlytics, Bugsnag. Symbolicated stack traces (upload dSYMs/mapping files in CI). Alert on crash rate spike. Target: < 0.5% crash-free session rate regression.
- [ ] **Analytics** — Event tracking (screen views, button taps, feature usage). Funnels (onboarding completion, purchase flow). Retention (D1, D7, D30). Tools: Firebase Analytics, Mixpanel, Amplitude, PostHog.
- [ ] **Performance monitoring** — Startup time trends, network request latency, slow frames percentage. Firebase Performance, Sentry Performance, or custom metrics. Alert on degradation.
- [ ] **Forced update mechanism** — Remote config check on app launch. Compare app version with minimum required version. Block usage if critical update needed. Graceful UX: explain why, link to store.
- [ ] **Remote config / feature flags** — Toggle features without app update. Firebase Remote Config, LaunchDarkly, Unleash. Gradual rollout of new features. Kill switch for broken features.
- [ ] **A/B testing** — Test onboarding flows, UI variations, feature rollouts. Firebase A/B Testing, Statsig, or custom. Measure impact on key metrics before full rollout.
- [ ] **User feedback channel** — In-app feedback (shake to report, feedback button). App store review prompts (timed right — after positive experience, not on first launch). Respond to reviews.
- [ ] **OTA updates (if applicable)** — CodePush (RN), Shorebird (Flutter) for JS/Dart bundle updates without store review. Not for native code changes. Use cautiously — rollback capability essential.
- [ ] **App size monitoring** — Track download and install size per release. Alert on unexpected growth. Users uninstall large apps when storage is low. Google Play and App Store show size prominently.
- [ ] **ANR / hang monitoring** — Android Application Not Responding (ANR) rate tracked. iOS hang detection (main thread blocked > 250ms). Alert on regression. Target: < 0.5% ANR rate (Google Play vitals threshold).
- [ ] **Store rating management** — In-app review prompt (StoreKit `requestReview`, Google In-App Review API) shown after positive moments. Never nag. Track average rating trend. Respond to negative reviews within 24h.

---

## Quick Sanity Check Before Launch

- [ ] App doesn't crash on first launch — tested on oldest supported OS version
- [ ] Works completely offline (or degrades gracefully with clear messaging)
- [ ] Deep links navigate to correct screen, even when app is cold-launched
- [ ] Push notification tap opens correct screen (backgrounded and killed states)
- [ ] Login → use app → kill app → reopen → still logged in (token persisted securely)
- [ ] No secrets in the app binary — verified with reverse engineering tool (jadx, Hopper)
- [ ] Tested on real low-end device (not just latest flagship or emulator)
- [ ] VoiceOver/TalkBack can navigate and complete core user flow
- [ ] Dark mode doesn't break any screen (no invisible text, no missing assets)
- [ ] Back button / swipe-back works correctly on every screen (no traps)
- [ ] App size is reasonable (< 50MB download, < 150MB installed)
- [ ] Battery usage is not excessive — verified with profiler
- [ ] Crash-free rate > 99% in beta testing
- [ ] Forced update mechanism works — tested with version gating
- [ ] App store metadata complete: screenshots, description, privacy policy, data safety

---

## Framework Quick Reference

### Flutter
- **Navigation:** GoRouter (declarative, deep linking), auto_route (code generation)
- **State:** Riverpod (recommended), Bloc (enterprise), Provider (simple apps)
- **Networking:** Dio + Retrofit (code gen) or http package
- **Local DB:** Drift (SQLite, type-safe), Hive (NoSQL, fast), Isar
- **Testing:** flutter_test (unit + widget), integration_test, Patrol (E2E), golden tests
- **CI/CD:** Codemagic (Flutter-optimized), Fastlane, GitHub Actions

### React Native
- **Navigation:** React Navigation (standard), Expo Router (file-based)
- **State:** TanStack Query + Zustand, or Redux Toolkit (complex apps)
- **Networking:** Axios, fetch + TanStack Query for caching
- **Local DB:** WatermelonDB (SQLite, sync), MMKV (fast key-value), Realm
- **Testing:** Jest + React Native Testing Library, Detox (E2E), Maestro
- **CI/CD:** EAS Build (Expo), Fastlane, App Center, GitHub Actions

### Kotlin (Jetpack Compose)
- **Navigation:** Navigation Compose (official), Voyager (multiplatform)
- **State:** ViewModel + StateFlow, Orbit MVI, or Molecule
- **Networking:** Retrofit + OkHttp, Ktor (multiplatform)
- **Local DB:** Room (SQLite, official), SQLDelight (multiplatform)
- **Testing:** JUnit + Compose UI Test, Espresso (legacy views), Maestro (E2E)
- **CI/CD:** Fastlane, GitHub Actions + Gradle, Bitrise

### Swift (SwiftUI)
- **Navigation:** NavigationStack (iOS 16+), custom Coordinator pattern
- **State:** `@Observable` (iOS 17+), `@StateObject` + Combine, TCA (The Composable Architecture)
- **Networking:** URLSession + async/await, Alamofire (complex needs)
- **Local DB:** SwiftData (iOS 17+), CoreData (legacy), GRDB (SQLite)
- **Testing:** XCTest + Swift Testing (new), XCUITest (E2E), Maestro
- **CI/CD:** Fastlane + Xcode Cloud, GitHub Actions + xcodebuild

### Universal Tools (any framework)
- **E2E Testing:** Maestro (YAML-based, any platform, easy to write)
- **Device Farm:** Firebase Test Lab, BrowserStack, AWS Device Farm
- **Crash Reporting:** Sentry (best DX), Firebase Crashlytics (free, good)
- **Analytics:** Firebase Analytics, Mixpanel, Amplitude, PostHog
- **Push:** Firebase Cloud Messaging (both platforms)
- **Feature Flags:** Firebase Remote Config (free), LaunchDarkly, Unleash
- **CI/CD:** Fastlane (universal mobile automation), Codemagic, Bitrise
- **OTA Updates:** Shorebird (Flutter), CodePush (RN) — skip store review for non-native changes
- **Design:** Figma → code workflows, design tokens for consistent cross-platform UI
