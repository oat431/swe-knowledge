# JVM Desktop Checklist

> Framework-specific checklist for building and shipping desktop apps on the JVM (Compose Multiplatform / JavaFX).
> Companion to the general [[desktop]] checklist — that one is the contract, this one is the plumbing.
> Last updated: 2026-11-14 (created: desktop framework layer)

---

## 1. Framework Choice

- [ ] **Compose Multiplatform when Kotlin-first and code sharing matter:** modern declarative UI, state-driven rendering, shares business logic (and often UI patterns) with Android; the desktop target renders through a Swing/AWT-backed canvas — native interop behaves accordingly → [[desktop]] §1
- [ ] **JavaFX/OpenJFX for mature JVM UI:** scene graph, FXML for declarative layouts, rich built-in controls (charts, tables, WebView), solid data-binding — fits existing Java codebases and teams.
- [ ] **Swing is maintenance-only:** still everywhere in legacy estates and fine to keep alive, but don't start new apps in it; if you inherit one, freeze the toolkit surface and wrap new features behind interfaces.
- [ ] **Embedded web content is a selection input:** JavaFX ships a WebView component (WebKit-based); Compose desktop has no first-party webview — check current options before committing if the app renders remote web content → [[desktop]] §2
- [ ] **The honest landscape:** both shipped toolkits have uneven edges — accessibility bridges, native look-and-feel, tray/notifications, and packaging all need per-OS verification (the sections below say where). Pick the toolkit your team can debug at the JVM *and* the OS level.
- [ ] **UI toolkit is a one-way door:** isolate business logic in plain JVM modules so a toolkit swap or headless reuse stays possible → [[desktop]] §1

## 2. Architecture & State Model

- [ ] **Compose state model:** single source of truth via `mutableStateOf`-class observable state; hoist state out of composables; recomposition is the render mechanism — state writes are the only way the UI updates.
- [ ] **JavaFX properties/MVVM:** observable properties (`ObservableValue`, property binding) drive view-model → view updates; FXML defines layout, controllers stay thin; bind, don't hand-sync.
- [ ] **UI thread discipline — never block it:** JavaFX Application Thread / Swing EDT (Compose desktop inherits the EDT for interop). `Platform.runLater`-class dispatch to hop onto the UI thread; never call it from the UI thread to "fix" reentrancy.

```java
executor.submit(() -> {
    var result = compute();                          // heavy work — never on the UI thread
    Platform.runLater(() -> view.show(result));      // hop back for the update
});
```

- [ ] **Background work stays off the UI thread:** I/O, parsing, and computation on executors or coroutine dispatchers (`Dispatchers.Default`/`Dispatchers.IO`), then post results back to the UI thread → [[desktop]] §9
- [ ] **Long operations report progress and cancel:** determinate progress where possible; a frozen UI thread is indistinguishable from a crash to the user → [[desktop]] §9
- [ ] **Window/scene lifecycle owned explicitly:** stage-close handlers dispose listeners, timers, and executors; the last-window-close behavior (exit vs hide-to-tray) is deliberate → [[desktop]] §5
- [ ] **ViewModel/presenter logic testable without the toolkit:** plain unit tests cover state transitions; the toolkit only renders them → [[qa]]

## 3. Packaging & Distribution

- [ ] **`jpackage` builds per-OS installers:** MSI/EXE (Windows), DMG/pkg (macOS), deb/rpm (Linux) — from a modular app or an app image; it runs `jlink` for you unless given a runtime image → [[desktop]] §3

```
jpackage --type msi --name MyApp --input build/libs --main-jar app.jar --icon app.ico
# per-OS type and flags: --type dmg / pkg (macOS), --type deb / rpm (Linux); run each on its target OS
```
- [ ] **`jlink` trimmed runtime bundled inside the app image:** users never install a JVM, and you never depend on the one they happen to have; cut unused modules to shrink size and startup.
- [ ] **Launchers beyond installers:** launch4j-class wrappers produce standalone Windows executables (icon, version metadata, single-instance flags) from the packaged app; Linux/macOS lean on the installer's launcher + desktop entry/Info.plist.
- [ ] **Fat-jar is a dev artifact, not a distribution format:** no bundled runtime, no per-OS natives, unreliable double-click behavior — ship installers to users; ship the jar to developers → [[desktop]] §3
- [ ] **GraalVM native-image feasibility varies by toolkit:** JavaFX apps can reach native images with metadata/configuration work; Compose desktop sits on AWT/Swing, which limits native-image compatibility — treat it as a per-app experiment, not a default promise.
- [ ] **Build on the target OS:** packaging and signing generally run on their target platform (macOS signing/notarization requires macOS); the CI matrix produces all three installers, not one cross-built one → [[desktop]] §11
- [ ] **Icon, app metadata, and file associations declared in the package config,** not patched in post-install → [[desktop]] §5
- [ ] **Signing (and macOS notarization) wired into the packaging step** so an unsigned installer cannot be produced → [[security]]

## 4. Startup Performance

- [ ] **Cold start is the JVM desktop liability:** JVM boot + toolkit init + class loading before the first frame; define a per-OS cold-start budget and measure it → [[desktop]] §9
- [ ] **Trimmed runtime (jlink/jpackage) is the biggest cheap win:** fewer modules to load and a smaller image to page in.
- [ ] **Splash screen hides init honestly:** jpackage/java can show a splash while the toolkit boots — measure time-to-interactive *without* it, so the splash covers work instead of masking it → [[desktop]] §9
- [ ] **Lazy module init:** defer heavy controllers, plugins, and data loads until after the first frame; wire "open document" arguments after the window is up.
- [ ] **Static initializers stay trivial:** anything expensive at class-load time taxes every launch (and every `--help`-style early exit in tool-style apps).
- [ ] **Warm relaunch paths defined:** single-instance focus (second launch signals the first) is often faster than a second full boot → [[desktop]] §5

## 5. OS Integration

- [ ] **AWT hooks for the basics:** `java.awt.Desktop` (Desktop.browse-class) opens URLs and mail, `java.awt.SystemTray` covers tray icons — support is uneven per OS (macOS menu bar especially), verify on each target → [[desktop]] §5
- [ ] **File associations and argument forwarding work:** the OS launches the app with a file path — survives spaces and unicode (Thai included); second-instance handling passes the path to the running window → [[desktop]] §5
- [ ] **Notifications go through platform APIs:** no unified JVM notification API — typically JNI/JNA bridges or a library wrapping each OS's mechanism; assume nothing, test each OS → [[desktop]] §5
- [ ] **Global shortcuts need native help:** the JDK has no global hotkey API — JNI/JNA or a library; make bindings remappable and collision-aware → [[desktop]] §5
- [ ] **Autostart is opt-in and OS-specific:** registry (Windows), LaunchAgents (macOS), autostart desktop entry (Linux); user setting, never a default → [[desktop]] §5
- [ ] **Single-instance enforced with a real mechanism:** socket, lock file, or OS-specific mutex; the second launch forwards its arguments and exits cleanly → [[desktop]] §5

## 6. Window & Display Management

- [ ] **Multi-window management explicit:** each window is a stage/scene with its own lifecycle; parent/child ownership and close-all behavior defined → [[desktop]] §6
- [ ] **HiDPI correct on every target:** JavaFX and Compose handle scale factors internally, but mixed-DPI multi-monitor setups still catch layout and blur bugs — verify per OS and per monitor → [[desktop]] §6
- [ ] **Window state persisted and restored per display:** position, size, maximized, monitor — clamped to visible bounds on restore (a saved second-monitor position must not restore off-screen) → [[desktop]] §6
- [ ] **Fullscreen/undecorated/always-on-top quirks budgeted:** these modes behave differently across window managers; test on each target OS → [[desktop]] §6
- [ ] **Software rendering fallback verified:** machines without GPU acceleration must still render acceptably (JavaFX software pipeline; Compose/AWT rendering paths) → [[desktop]] §9

## 7. Accessibility

> **Honest baseline:** the Java accessibility bridge is the least reliable layer on the JVM desktop — it has improved but remains uneven across OSes and screen readers. Treat accessibility sign-off as a per-OS manual test, not a build result.

- [ ] **Java Accessibility API + screen readers — historically inconsistent:** AWT/JavaFX accessibility bridges vary by OS and reader; do not trust "the API exists" — test with the real reader per OS (VoiceOver, Narrator, Orca) → [[desktop]] §8
- [ ] **Compose accessibility semantics exposed:** content descriptions, roles, and state on interactive elements; verify current desktop semantics support against the JetBrains docs — the surface moves → [[desktop]] §8
- [ ] **Keyboard-only flows complete:** focus order sane, no keyboard traps, every action reachable — scene-graph focus traversal (JavaFX) and focus targets (Compose) configured deliberately → [[desktop]] §8
- [ ] **Text scales with OS settings;** controls keep minimum target sizes at high scale factors → [[desktop]] §8
- [ ] **Known gaps documented** for regulated contexts instead of discovered in procurement → [[desktop]] §12

## 8. Testing

> **CI reality:** UI tests can run headless on Linux behind a virtual framebuffer (xvfb-class), but visual fidelity, HiDPI, IME, and screen-reader checks need real-OS passes — budget both.

- [ ] **Compose UI testing has its own test artifacts:** JUnit-based desktop UI tests (`runComposeUiTest`-class API) find nodes, assert state, and drive interaction — mirror the official testing docs for the current API shape → [[qa]]

```kotlin
runComposeUiTest {              // Compose desktop test artifact; TestFX-class equivalents for JavaFX
    setContent { App() }
    onNodeWithText("Save").performClick()
}
```

- [ ] **TestFX-class UI tests for JavaFX:** drive the scene graph, assert on nodes and properties; run them on the real toolkit, not a mock.
- [ ] **Headless CI is a real constraint:** xvfb-class virtual framebuffer on Linux for UI tests; anything visual, HiDPI, or IME-related needs real-OS runners (or periodic manual passes) → [[desktop]] §11
- [ ] **Packaged-build smoke test per OS:** the *installer artifact* boots, shows the main window, and completes the core task — dev-mode tests never catch packaging breakage → [[desktop]] §11
- [ ] **Logic tested toolkit-free:** view-model/state tests run as plain JVM tests in the fast suite; UI tests cover wiring, not business rules → [[qa]]
- [ ] **OS matrix in CI:** at least build + smoke on each supported OS; support matrix maintained and old OSes dropped deliberately → [[desktop]] §11

## 9. Memory & Resources

- [ ] **JVM heap footprint for desktop apps is a baseline cost:** runtime + toolkit + app before any user data; set an idle RSS target and measure it (heap size ≠ RSS — measure the process) → [[desktop]] §9
- [ ] **GC pauses can hitch the UI:** size the heap for pause-friendly behavior on the desktop cadence; watch for jank spikes that correlate with collection → [[desktop]] §9
- [ ] **Idle CPU near zero when hidden:** no busy-wait loops, no polling timers without backoff; tray-hidden apps especially → [[desktop]] §9
- [ ] **Leaks hunted across open/close cycles:** stages disposed, listeners removed, executors shut down — repeated open/close of windows is the leak test that matters → [[desktop]] §9
- [ ] **Thread hygiene:** bounded executors, named threads, no unbounded fan-out from UI event handlers → [[desktop]] §9

## 10. Security

- [ ] **Dependency scan covers the packaged runtime image,** not just the jar — jlink pulls module dependencies into the artifact → [[02 Dependency & Supply Chain]]
- [ ] **Per-OS code signing (and macOS notarization) enforced:** Authenticode on Windows, Developer ID + notarization on macOS, GPG/repo verification on Linux; CI fails on unsigned output → [[security]]
- [ ] **JNI/JNA native libraries audited:** every native dependency tracked, version-pinned, and from a trusted source — native code bypasses the JVM's sandbox of guarantees → [[02 Dependency & Supply Chain]]
- [ ] **FXML/XML parsing hardened if loading external layouts:** external entity resolution off; never load layout or config XML from untrusted locations → [[desktop]] §2
- [ ] **Secrets in OS credential stores** (Keychain / Credential Manager / Secret Service via JNI/JNA bridges), never in plaintext config → [[02 Secrets Management]]
- [ ] **Update packages signed and verified before install**; the updater trusts only artifacts that verify against the pinned key → [[desktop]] §4

---

## Quick Sanity Check Before Release

- [ ] Installer built and smoke-tested per OS from the packaged artifact (not the dev run)
- [ ] Cold start measured per OS against the budget — with and without the splash screen
- [ ] jlink/jpackage runtime ships inside the app: a machine with no JVM installed runs the app
- [ ] UI thread never blocked: long operations move work off-thread and show progress
- [ ] Screen reader reads the core flow on at least one real reader per OS; keyboard-only completes the core task
- [ ] Single instance, file-open forwarding, and tray/close behavior verified per OS
- [ ] Installer signed (and notarized on macOS); CI cannot produce an unsigned build
- [ ] Idle memory and CPU measured on a real desktop session, not just in tests

---

## Sources

- JetBrains Compose Multiplatform (getting started, desktop testing): https://www.jetbrains.com/help/kotlin-multiplatform-dev/compose-multiplatform-getting-started.html · https://kotlinlang.org/docs/multiplatform/compose-desktop-ui-testing.html
- OpenJFX documentation (JavaFX, jlink/jpackage packaging guidance): https://openjfx.io/openjfx-docs/
- Master checklist: [[desktop]].
