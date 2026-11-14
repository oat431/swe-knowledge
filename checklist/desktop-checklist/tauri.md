# Tauri Desktop Checklist

> Framework-specific checklist for building and shipping desktop apps with Tauri.
> Companion to the general [[desktop]] checklist — that one is the contract, this one is the plumbing.
> Last updated: 2026-11-14 (created: desktop framework layer)

---

## 1. Architecture: Rust Core + System Webview

- [ ] **Privileged work lives in the Rust core; the webview only renders and requests:** the core process holds the privileges, the webview is the untrusted UI layer → [[desktop]] §1
- [ ] **No bundled Chromium — the OS webview is the renderer:** WebView2 on Windows, WKWebView on macOS, WebKitGTK on Linux. Engine differences (rendering, storage, clipboard, media codecs) are *your* test matrix, not an upstream footnote → [[desktop]] §1
- [ ] **System webview dependencies gate your OS support matrix:** WebKitGTK is a system package on Linux (distro availability decides your minimum); WKWebView tracks the OS's WebKit; WebView2 has its own runtime distribution story (§8).
- [ ] **Frontend framework is an implementation detail:** React, Vue, Svelte, or vanilla all work — Tauri serves built static assets to the webview; keep the boundary framework-agnostic.
- [ ] **Know what rides underneath:** TAO (windows, menus, tray) and WRY (webview abstraction) surface platform quirks that are not your code's fault — but are your bug tracker's problem.
- [ ] **Major-version posture checked before committing a roadmap:** the current stable major is established and a next major is in alpha — read the migration guides before betting long-lived projects on either.

## 2. Security Model: Capabilities & Permissions

- [ ] **Capability files are the allowlist:** permissions are granted per window/webview in capability files, with allow/deny sets — default deny, grant only what each window needs → [[desktop]] §2
- [ ] **Plugin permissions carry scopes:** fs path scopes, shell command scopes, HTTP URL scopes — the scope is part of the grant; a broad scope turns a narrow permission into a wide one.
- [ ] **Every Tauri command validates its arguments Rust-side:** `invoke` arguments arrive as JSON — parse into typed structs and check semantics (paths, URLs, lengths) before acting; the frontend is not a validator → [[security]]
- [ ] **`withGlobalTauri` stays off unless a hard requirement forces it:** it injects the API object into every page; the module import is the tighter default.
- [ ] **CSP is set for all bundled HTML, and the asset protocol is off unless needed** (its `scope` restricted to exact paths when enabled). Never weaken the injected CSP via the `dangerousDisableAssetCspModification` escape hatch → [[desktop]] §2
- [ ] **Remote or untrusted content never runs in the privileged webview:** arbitrary URLs get zero capability grants; the isolation pattern exists for a reason → [[security]]
- [ ] **Secrets go to the OS credential store or the Stronghold plugin, never plaintext config or webview storage** → [[02 Secrets Management]]

```json
{
  "identifier": "main-capability",
  "windows": ["main"],
  "permissions": [
    "core:default",
    "dialog:allow-open",
    "fs:allow-appdata-read"
  ]
}
```

## 3. IPC Hygiene

- [ ] **The command surface is the trust boundary:** keep the list of Tauri commands small and reviewable; every new command is new attack surface → [[desktop]] §1
- [ ] **Commands are typed request/response:** serializable args in, serializable results out — no ambient authority; the capability grant is what authorizes the call, the command still validates its input.
- [ ] **Events are one-way and permission-gated:** fire-and-forget messages usable from both sides, controlled by event permissions — don't grow a shadow RPC out of event channels.
- [ ] **Custom protocols and embedded-asset serving validate their inputs:** scheme handlers parse untrusted input — because they are one → [[desktop]] §2
- [ ] **Multi-window apps separate capabilities per window:** a settings window does not need file-write; never ship one fat capability for all windows.

```rust
// src-tauri/src/lib.rs
#[tauri::command]
fn open_project(path: String) -> Result<Project, AppError> {
    validate_path(&path)?; // the webview's arguments are untrusted input
    load_project(path)
}
```

## 4. Plugins

- [ ] **Inventory every official plugin you compile in:** shell, fs, dialog, opener, notification, deep-link, single-instance, updater, autostart, global-shortcut, clipboard, sql, store, http, os, process, window-state — each is justified in writing or dropped.
- [ ] **Each plugin adds permission surface and scope:** minimize the set, grant only the scopes in use, and re-audit when adding one → [[02 Dependency & Supply Chain]]
- [ ] **The shell plugin is the loaded one:** command execution and sidecar spawning are the highest-privilege operations in the system — scope to exact binaries, never a general shell → [[security]]
- [ ] **Community plugins are third-party code:** vet them like any dependency — maintenance, transitive crates, and which permissions they demand → [[02 Dependency & Supply Chain]]
- [ ] **Persisted scopes survive restarts (persisted-scope plugin):** a widened fs scope is remembered — review what got saved, not just what you granted today.

## 5. Packaging & Signing

- [ ] **Bundler output per OS:** deb/rpm/AppImage on Linux, app bundle + DMG on macOS, NSIS (`-setup.exe`) and MSI on Windows → [[desktop]] §3
- [ ] **macOS: hardened runtime + notarization + entitlements before distribution:** Gatekeeper blocks unsigned or un-notarized apps; entitlements match actual features, no blanket grants → [[desktop]] §3
- [ ] **Windows: Authenticode-sign the installer and the app binary:** SmartScreen reputation follows signatures → [[desktop]] §3
- [ ] **Linux: signatures/checksums on self-distributed artifacts** (GPG; Flathub/repos verify for you, direct AppImage downloads do not) → [[security]]
- [ ] **Clean-machine install/upgrade/uninstall tested per format:** uninstall removes app files and offers user-data removal → [[desktop]] §3

## 6. Updates

- [ ] **Updater signature verification is mandatory — it cannot be disabled:** sign every update bundle, pin the public key in the app config, and ship the `.sig` files alongside the artifacts → [[02 Dependency & Supply Chain]]
- [ ] **Updater artifacts produced deliberately (`createUpdaterArtifacts`):** the updater consumes specific archive forms of the bundles — serve and test exactly what it consumes.
- [ ] **Update endpoints template target, arch, and current version:** the server must return the right artifact per platform; a failed update check is invisible to the user (server down ≠ app broken) → [[desktop]] §4
- [ ] **Staged rollout and channels:** gradual rollout, beta channel, and a forced path for security fixes → [[desktop]] §4
- [ ] **Rollback tested:** a bad update never bricks the install; the previous version stays recoverable → [[desktop]] §4

```json
{
  "bundle": { "createUpdaterArtifacts": true },
  "plugins": {
    "updater": {
      "pubkey": "CONTENT OF PUBLICKEY.PEM",
      "endpoints": ["https://releases.example.com/{{target}}/{{arch}}/{{current_version}}"]
    }
  }
}
```

## 7. OS Integration

- [ ] **Deep links via the deep-link plugin, payloads validated like any untrusted input:** the URL is attacker-controllable → [[desktop]] §5
- [ ] **Single-instance plugin owns second-launch behavior:** a second launch focuses the running instance instead of spawning a twin → [[desktop]] §5
- [ ] **Tray, notifications, autostart, file associations, global shortcuts — scoped official plugins:** autostart is a user setting, never a default; notifications are permission-aware; shortcuts are remappable → [[desktop]] §5
- [ ] **Window state (position/size/maximized) persisted and restored per display** via the window-state plugin — verify multi-monitor restores → [[desktop]] §6

## 8. Windows Realities

- [ ] **WebView2 runtime distribution is a shipping decision:** download bootstrapper (default, needs network), embed the bootstrapper, embed the offline installer, or bundle a fixed runtime (installer grows by hundreds of MB); `skip` only when you control the machine fleet → [[desktop]] §3
- [ ] **`minimumWebview2Version` set deliberately:** pin the runtime floor to the features you rely on — the runtime auto-updates, but managed enterprise machines lag.
- [ ] **Paths and arguments survive real-world input:** unicode (Thai included), spaces, long paths — test file associations and launch args end to end → [[desktop]] §5
- [ ] **NSIS vs MSI is an enterprise question:** MSI suits managed deployment (Group Policy, software inventory); NSIS is the leaner default for direct downloads.

## 9. Performance & Resources

- [ ] **The small-binary advantage is earned, then easily lost:** the Rust core is tiny — don't ship a heavy frontend bundle that eats the win; audit the JS payload → [[desktop]] §9
- [ ] **Heavy work runs in Rust, off the webview thread:** async commands and background threads for compute and IO; never block the webview event loop → [[desktop]] §9
- [ ] **Startup budget defined and measured on all three webviews:** first paint includes webview init cost, which differs per OS engine.
- [ ] **Idle footprint bounded and leak-checked:** near-zero CPU when hidden; memory measured per webview engine, they differ → [[desktop]] §9

## 10. Reliability & Observability

- [ ] **Rust panics contained and reported:** a panic in a command must not kill the app silently; crash reporting on both sides (Rust core and webview JS) with user consent → [[desktop]] §10
- [ ] **Webview console and JS errors captured into the app log:** frontend stderr is invisible otherwise — forward it to the rotating local log → [[desktop]] §10
- [ ] **Logs rotate with a documented path** users can send to support, plus an About/doctor dialog showing version, OS, update state, and data paths → [[desktop]] §10

## 11. Testing & Release Engineering

- [ ] **WebDriver E2E via tauri-driver runs on packaged builds:** smoke the real installer artifact per OS, not just the dev server → [[desktop]] §11
- [ ] **Rust tests cover command logic; frontend tests cover UI logic:** command validation deserves plain `cargo test` units; mock the Tauri API layer in frontend tests.
- [ ] **Upgrade smoke tests:** install the previous release → upgrade → data intact; also test that a tampered update bundle is rejected by signature verification → [[qa]]
- [ ] **Signing and notarization run in CI and fail loudly when broken** — they rot silently otherwise → [[desktop]] §11

## 12. Sidecar Binaries

- [ ] **Shipped native helpers are part of the attack surface:** sidecars (embedded external binaries) run with the app's privileges — every invocation goes through scoped shell permissions → [[security]]
- [ ] **Sidecars are signed with the app:** unsigned helpers break notarization/Gatekeeper and are a trust gap → [[desktop]] §3
- [ ] **Sidecar packaging matches the target architecture:** the right binary variant per OS/arch is picked by the bundler — test each target's packaged output.

---

## Quick Sanity Check Before Release

- [ ] Capabilities audited: every window holds minimum permissions, `withGlobalTauri` off, no unscoped plugin grants
- [ ] UI verified on WebView2, WKWebView, and WebKitGTK — the webview matrix is tested, not assumed
- [ ] Update flow end to end: signed bundle served, signature verified, tampered bundle rejected, rollback available
- [ ] WebView2 install strategy chosen and tested on a machine without the runtime
- [ ] Shell/sidecar invocation scoped to exact binaries, fs scoped to exact paths, HTTP scoped to exact URLs
- [ ] Packaged-build E2E smoke passes per OS through the WebDriver setup
- [ ] Signed, notarized build installs on a clean machine and completes the core task

---

## Sources

- Tauri documentation: https://tauri.app (capabilities & permissions, IPC, plugins, bundler & signing, updater, WebDriver).
- Master checklist: [[desktop]].
