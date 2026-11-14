# Electron Desktop Checklist

> Framework-specific checklist for building and shipping desktop apps with Electron.
> Companion to the general [[desktop]] checklist — that one is the contract, this one is the plumbing.
> Last updated: 2026-11-14 (created: desktop framework layer)

---

## 1. Process Model: Main, Renderer & Preload

- [ ] **Main process holds every privilege; renderers hold none:** Node, filesystem, and OS access live in main; renderers are Chromium-only view layers → [[desktop]] §1
- [ ] **One preload script exposes a minimal typed API surface:** `contextBridge.exposeInMainWorld('api', {...})` — every method named and typed; no passthrough of arbitrary channels or modules → [[desktop]] §1
- [ ] **`webPreferences` on every window: `nodeIntegration: false`, `contextIsolation: true`, `sandbox: true`:** the secure defaults must survive custom windows, popups, and `webview` embeds.

```js
new BrowserWindow({
  webPreferences: {
    preload: PRELOAD, // typed contextBridge surface only — nothing else
    nodeIntegration: false,
    contextIsolation: true,
    sandbox: true,
  },
});
```
- [ ] **The `remote` module era is over:** never reintroduce it via compatibility shims — its main↔renderer object sharing was the root insecurity it carried.
- [ ] **IPC is typed and channel-fixed:** `ipcMain.handle` / `ipcRenderer.invoke` for request/response, explicit `send`/`on` pairs for events; both ends validate payloads against the channel's shape → [[desktop]] §1
- [ ] **Background helpers are explicit:** `utilityProcess`-class workers and tray helpers documented, resource-bounded, stoppable → [[desktop]] §1

## 2. Security Hardening (Electron Threat Surface)

- [ ] **`webPreferences` locked down per window:** a child window created with defaults silently undoes the main window's lockdown — every `BrowserWindow` construction site passes the hardened options explicitly → [[security]]
- [ ] **Navigation guarded:** `will-navigate` cancelled outside an allowlist; `setWindowOpenHandler` denies or routes allowed origins with a hardened window — never `{ action: 'allow' }` blind.
- [ ] **CSP on every loaded content path:** local and remote; via `onHeadersReceived` for `http(s)` responses, not just `<meta>` tags → [[desktop]] §2
- [ ] **IPC senders validated:** handlers check `event.senderFrame`/sender identity against expected windows; no wildcard listeners on dynamically constructed channel names.
- [ ] **`executeJavaScript` never runs on untrusted input:** string-built scripts from user or network data are the classic injection path into a privileged renderer.
- [ ] **Untrusted remote content never gets preload privileges:** it loads in its own window/session without a preload — a preload bridged to remote content is a full compromise by design → [[desktop]] §2
- [ ] **Secrets in the OS credential store** (`safeStorage`/Keychain/Credential Manager), never plaintext config or localStorage → [[02 Secrets Management]]

## 3. Packaging & Signing

- [ ] **electron-builder/forge-class tooling, config committed:** one reproducible pipeline builds all platforms; installer config is code, reviewed like code → [[desktop]] §3
- [ ] **Platform-native installers:** NSIS on Windows, signed + notarized DMG/pkg on macOS, AppImage/Flatpak/deb/rpm on Linux → [[desktop]] §3
- [ ] **macOS signing + notarization entitlements correct:** hardened runtime on; JIT-adjacent entitlements (appended JIT class) only where the app actually needs them; no `get-task-allow` in release builds — Gatekeeper blocks otherwise → [[desktop]] §3
- [ ] **Windows Authenticode signed and timestamped:** unsigned or untimestamped binaries age into untrusted after cert expiry.
- [ ] **Per-user vs system install decided deliberately:** NSIS `perMachine` changes permissions, update paths, and enterprise policy → [[desktop]] §3
- [ ] **Clean-machine install/upgrade/uninstall tested per installer flavor:** the uninstaller removes install dir, registry, and start-menu entries and offers user-data removal → [[desktop]] §3
- [ ] **Bundled runtime in the SBOM; dependencies scanned:** the packaged app ships Chromium + Node — both count as supply chain → [[02 Dependency & Supply Chain]]

## 4. Updates

- [ ] **Auto-update via electron-updater-class tooling or built-in `autoUpdater`:** one chosen path, feed over HTTPS, consistent across installers → [[desktop]] §4
- [ ] **Artifacts signed and verified before install:** the updater must never apply an unsigned package — mandatory, not advisory → [[02 Dependency & Supply Chain]]
- [ ] **Differential downloads enabled** (blockmap-class deltas): full-CDN-size updates on every release are the top bandwidth cost of Electron apps → [[desktop]] §4
- [ ] **Channels and staged rollout:** stable/beta feeds, gradual rollout, forced path for security fixes; update UX polite and rollback-capable → [[desktop]] §4
- [ ] **Update-check failures invisible:** feed down ≠ app broken → [[desktop]] §4
- [ ] **Feed integrity enforced:** HTTPS feed with signed artifacts and published checksums — a compromised feed must not become code execution → [[02 Dependency & Supply Chain]]

## 5. OS Integration

- [ ] **Single-instance lock** (`app.requestSingleInstanceLock`-style): second launch forwards its argv to the running instance, which focuses or handles `--new-window` deliberately → [[desktop]] §5
- [ ] **Deep links via protocol registration** (`app.setAsDefaultProtocolClient`-class): the `open-url`/argv handler parses untrusted input — validate scheme, host, and path like any other external request → [[desktop]] §2
- [ ] **Tray and application menu are real:** `Tray`/`Menu` rebuilt on locale and window-state changes; tray icon has a documented quit path → [[desktop]] §5
- [ ] **Notifications use the OS-native API** (`Notification`): permission-aware; action buttons deep-link into the right window state → [[desktop]] §5
- [ ] **Jump lists, taskbar progress, file associations wired:** `app.setUserTasks`-class lists, `setProgressBar`-class feedback, installer file associations + argv forwarding surviving spaces and unicode → [[desktop]] §5
- [ ] **Autostart is opt-in** (`app.setLoginItemSettings`-class): "start at login" is a user setting, never a default → [[desktop]] §5

## 6. Windows & Display Management

- [ ] **Window state persisted per display:** bounds + maximized/normal stored and validated on restore — the remembered monitor may be gone; clamp to visible work area → [[desktop]] §6
- [ ] **HiDPI verified at real scale factors:** crisp at 125–200% on Windows, Retina on macOS; zoom/focus behavior checked across mixed-DPI monitors → [[desktop]] §6
- [ ] **`BrowserWindow` lifecycle etiquette:** `close` intercepted for unsaved-work prompts, then destroyed deliberately; `ready-to-show` (or `show: false` + reveal) avoids the white flash; multi-window instances sync shared state.
- [ ] **Keyboard reachability preserved in the webview UI:** focus order, no traps, shortcuts registered via `globalShortcut`-class APIs remappable and collision-checked → [[desktop]] §6
- [ ] **Shared local data across windows is single-writer:** mutations funnel through the main process or an explicit lock/merge; two renderers editing the same store corrupt it → [[desktop]] §6

## 7. Native Modules & Sidecars

- [ ] **N-API prebuilds over source builds:** prebuilt binaries avoid the node-gyp rebuild matrix across Electron's ABI; source builds pin the rebuild step (`@electron/rebuild`-class) in the build pipeline.
- [ ] **Preload scripts stay dependency-free:** a native module in preload fights the sandbox and multiplies the crash surface.
- [ ] **Heavy or crash-prone work in sidecar processes** (`utilityProcess`/child process class): a segfaulting native worker must not take the app down → [[desktop]] §1
- [ ] **Native deps reviewed like web deps:** advisories, licenses, and SBOM entries cover compiled code too → [[02 Dependency & Supply Chain]]
- [ ] **ABI drift caught in CI:** the packaged build's smoke test actually loads each native module — a rebuild mismatch surfaces at runtime, not at build time.

## 8. Performance & Resources

- [ ] **Chromium memory footprint budgeted per window:** each renderer is its own Chromium instance — the single biggest Electron cost; measure with `app.getAppMetrics`/process views and cap the window count → [[desktop]] §9
- [ ] **`backgroundThrottling` left on** (or a documented reason for turning it off): hidden windows near zero CPU; battery impact considered → [[desktop]] §9
- [ ] **Startup budget measured on the packaged build:** lazy window creation, no sync I/O on the main process at launch, splash screens hiding real work only → [[desktop]] §9
- [ ] **GPU-free rendering verified:** software fallback works (`app.disableHardwareAcceleration`-class path) when hardware acceleration is unavailable or blacklisted → [[desktop]] §9
- [ ] **Window/renderer reuse policy defined:** long-lived hidden windows vs recreate-on-demand — decided, not accumulated.

## 9. Reliability & Observability

- [ ] **`crashReporter.start` with user consent:** minidumps + symbols uploaded, PII scrubbed → [[desktop]] §10
- [ ] **Sentry-class SDK wired in both processes** (main and renderer), with PII scrubbing before upload; renderer errors can't silently vanish behind the bridge.
- [ ] **Uncaught-exception boundaries in both processes:** `uncaughtException`/`unhandledRejection` handling in main (a main crash is an app crash) and a renderer error boundary; both report and degrade gracefully → [[desktop]] §10
- [ ] **Local logs rotate and are findable** at a documented path users can send to support → [[desktop]] §10
- [ ] **About/doctor dialog reports the plumbing:** app + runtime versions, OS, update state, data paths, GPU status → [[desktop]] §10

## 10. Testing & Release Engineering

- [ ] **Playwright's Electron driver for E2E:** run against the packaged build, not just the dev server → [[desktop]] §11. **Not Spectron** — it is retired/dead; do not adopt or recommend it.
- [ ] **Unit tests per process:** main-process logic tested as plain Node modules (no window needed); renderer components tested in a DOM harness; the bridge contract tested from both sides → [[qa]]
- [ ] **Upgrade smoke tests:** install version N-1 → upgrade to N → data intact; rollback path exercised → [[desktop]] §11
- [ ] **Signing + notarization run in CI and fail loudly:** they rot silently otherwise; an unsigned artifact must be unreachable → [[desktop]] §11
- [ ] **Security checklist run per release:** grep gates for `nodeIntegration: true`, `sandbox: false`, `contextIsolation: false` in the shipped tree → [[security]]
- [ ] **OS support matrix maintained:** minimum OS versions documented and tested against the packaged build; old OSes dropped deliberately, with notice → [[desktop]] §11

## 11. Accessibility (Chromium Layer)

- [ ] **Chromium accessibility tree exposes names/roles/states:** real ARIA semantics on custom controls — test with VoiceOver/Narrator/Orca themselves, not just the tree dump → [[desktop]] §8
- [ ] **Native-feel caveats documented:** a webview UI won't match every platform convention (focus rings, standard dialogs, font rendering); known gaps listed for regulated contexts → [[desktop]] §8
- [ ] **OS settings respected:** high contrast / forced-colors, reduced motion, and text scaling driven by OS preferences → [[desktop]] §8

## 12. Privacy & Telemetry

- [ ] **Permission handler deny-by-default** (`session.setPermissionRequestHandler`): media, geolocation, notifications, clipboard allowed per-origin and per-use with in-context rationale → [[desktop]] §12
- [ ] **Remote/webview content gets its own session and telemetry policy:** third-party pages in `webview` are third parties — separate partition, no shared cookies or identifiers.
- [ ] **Renderer telemetry scrubbed and disclosed:** what leaves the machine documented, toggled in settings → [[desktop]] §12
- [ ] **Local user data deletable and exportable:** uninstall/deletion removes app data per policy; privacy policy matches actual behavior → [[desktop]] §12

---

## Quick Sanity Check Before Release

- [ ] Every `BrowserWindow` construction passes explicit hardened `webPreferences`; no `nodeIntegration: true`, `sandbox: false`, or `contextIsolation: false` in the shipped tree
- [ ] `contextBridge` surface is the typed allowlist only — no raw `ipcRenderer` reachable from page code
- [ ] `window.open` and `will-navigate` are blocked outside the origin allowlist
- [ ] Fresh install on a clean VM per OS: launches, signs in, completes the core task
- [ ] Auto-update from the previous version installs cleanly; the update package is verified as signed
- [ ] Crash reporting and error boundaries active in main *and* renderer (a thrown test error in each reaches the sink)
- [ ] Screen reader reads the core flows; keyboard-only completes the core task
- [ ] Per-window memory within budget after 30 minutes of normal use, including open/close cycles

---

## Sources

- Electron docs: https://www.electronjs.org/docs/latest
- Master checklist: [[desktop]].
- Cross-checklist: [[security]] (threat modeling), [[qa]] (test strategy), [[02 Secrets Management]] (credential storage), [[02 Dependency & Supply Chain]] (native deps, signed updates).
