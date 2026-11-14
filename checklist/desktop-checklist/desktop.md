# Desktop Application Checklist

> Framework-agnostic checklist for shipping desktop applications (Windows, macOS, Linux).
> Electron, Tauri, Qt, native — the OS contract is the same. The plumbing changes.
> Last updated: 2026-11-14 (created: general desktop surface; framework layers deferred)

---

## 1. Architecture & Process Model

- [ ] **Privileged and unprivileged code are separated:** the main/host process holds privileges; the UI layer is sandboxed (renderer, in Electron/Tauri terms).
- [ ] **IPC surface is minimal and typed:** every bridge message validated on both ends. Allowlists, never pass-through.
- [ ] **Background helpers are explicit:** any daemon/tray/helper process is documented, resource-bounded, and stoppable.

## 2. Security

- [ ] **UI layer cannot execute privileged code directly:** capability allowlists on, direct system access off → [[02 Secure Coding Practices]]
- [ ] **Remote content is hostile by default:** CSP wherever web content loads; navigation restricted to an allowlist; never load untrusted URLs directly.
- [ ] **Deep links / custom protocol handlers validated:** scheme handlers parse untrusted input — because they are one.
- [ ] **Local secrets in OS credential stores:** Keychain / Credential Manager / Secret Service — never plaintext config → [[02 Secrets Management]]
- [ ] **Update packages signed and verified before install:** mandatory, not advisory → [[02 Dependency & Supply Chain]]
- [ ] **Dependencies scanned; SBOM covers the bundled runtime** → [[02 Dependency & Supply Chain]]

## 3. Packaging & Signing

- [ ] **Platform-native installers:** MSI/MSIX or NSIS (Windows), signed + notarized DMG/pkg (macOS), AppImage/Flatpak/deb/rpm (Linux).
- [ ] **Code signing on every platform:** Authenticode (Windows), Developer ID + notarization (macOS — Gatekeeper blocks unsigned), GPG/Flathub verification (Linux).
- [ ] **Clean-machine install/upgrade/uninstall tested:** uninstall removes files, registry/launch entries, and offers user-data removal.
- [ ] **Per-user vs system install decided:** permissions, update paths, and enterprise policy differ.

## 4. Updates

- [ ] **Auto-update with staged rollout:** channels (stable/beta), gradual rollout, and a forced path for security fixes.
- [ ] **Updates are atomic and rollback-capable:** a failed update never bricks the install; the previous version stays recoverable.
- [ ] **Update UX is polite:** silent or notified; never interrupts work; release notes surfaced.
- [ ] **Update-check failures are invisible:** server down ≠ app broken.

## 5. OS Integration

- [ ] **Single-instance behavior:** second launch focuses the first window (or handles `--new-window` deliberately).
- [ ] **File associations and `Open with` work:** argument forwarding from OS launch survives spaces and unicode (Thai included).
- [ ] **Notifications use the OS API:** permission-aware; action buttons deep-link into the right app state.
- [ ] **Tray/menu-bar and autostart are opt-in:** "start at login" is a user setting, never a default.
- [ ] **Global shortcuts are remappable** and don't collide with OS/common-app shortcuts.

## 6. Window & Display Management

- [ ] **HiDPI and multi-monitor are correct:** crisp rendering at every scale factor; window state (position/size/maximized) restored per display.
- [ ] **Everything is keyboard-reachable:** sane focus order, no keyboard traps, discoverable shortcuts.
- [ ] **Shared local data across windows/instances is safe:** locking or merge strategy defined.

## 7. Data, Offline & Sync

- [ ] **Local schema versioned; migrations tested** from the oldest supported version — upgrades never lose data → [[03 Migration Backup & Scaling]]
- [ ] **Offline behavior defined:** what works offline, what queues, how conflicts resolve on reconnect.
- [ ] **Export/backup path exists:** user data exportable in an open format; local-DB corruption handling defined.

## 8. Accessibility (OS-level)

- [ ] **Screen readers work:** platform accessibility APIs expose names/roles/states (VoiceOver, Narrator, Orca) — tested with the screen reader itself, not just the API.
- [ ] **High contrast and reduced motion respected:** OS settings drive appearance; no color-only signals.
- [ ] **Text scales with OS settings;** controls meet minimum target sizes.
- [ ] **Known accessibility gaps documented** for regulated contexts.

## 9. Performance & Resources

- [ ] **Cold-start budget defined and measured;** warm launch is fast; splash screens hide real work only.
- [ ] **Memory footprint bounded:** idle target set; leak-checked across open/close window cycles.
- [ ] **Background behavior is polite:** near-zero CPU when hidden; battery impact considered.
- [ ] **Rendering verified without a GPU:** software fallback where hardware acceleration is unavailable.

## 10. Reliability & Observability

- [ ] **Crash reporting with user consent:** minidumps + symbols uploaded; PII scrubbed.
- [ ] **Local logs rotate and are findable:** documented path users can send to support.
- [ ] **Session restore / unsaved-work protection** where feasible.
- [ ] **About/doctor dialog:** version, OS, update state, data paths.

## 11. Testing & Release Engineering

- [ ] **UI automation runs on packaged builds,** not just dev — smoke E2E on the real installer artifact per OS.
- [ ] **Upgrade tests:** install version N-1 → upgrade to N → data intact; rollback path tested too.
- [ ] **Signing + notarization run in CI and fail loudly when broken** (they rot silently otherwise).
- [ ] **OS support matrix maintained:** minimum versions documented and tested; old OSes dropped deliberately, with notice.

## 12. Privacy & Compliance

- [ ] **Telemetry disclosed and controllable:** settings toggle; collection documented.
- [ ] **Permissions minimal and explained:** mic/camera/screen only when needed, with in-context rationale.
- [ ] **Deletion and retention honored:** account deletion removes local data; the privacy policy matches actual behavior.

---

## Quick Sanity Check Before Release

- [ ] Fresh install on a clean VM per OS: launches, signs in, completes the core task
- [ ] Upgrade from the previous version preserves data and settings
- [ ] Uninstall removes the app cleanly (and offers data removal)
- [ ] Screen reader reads the core flows; keyboard-only completes the core task
- [ ] The release path cannot produce an unsigned build (CI enforces signing)
- [ ] The core task works offline wherever offline support is claimed

---

## Sources

- Principles distilled from platform HIGs (Apple Human Interface, Windows Fluent/WinUI, GNOME/KDE), OWASP desktop guidance, secure-update practice.
- Cross-checklist: [[security]] (threat model, supply chain), [[qa]] (test strategy), [[release]] (versioning & rollout).
