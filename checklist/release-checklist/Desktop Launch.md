# Desktop Launch Checklist

> Tick every box before a desktop app ships to users. Framework-agnostic. Deep knowledge: follow the `→` links.

---

## Security

- [ ] UI layer sandboxed; privileged operations only via minimal typed IPC → [[02 Secure Coding Practices]]
- [ ] Remote content: CSP + navigation allowlist; untrusted URLs never loaded directly
- [ ] Deep links/protocol handlers validated like untrusted input
- [ ] Secrets in OS credential stores, not plaintext config → [[02 Secrets Management]]
- [ ] Dependencies scanned; SBOM for the bundled runtime → [[02 Dependency & Supply Chain]]

## Packaging & Updates

- [ ] Platform-native installers for every OS you ship
- [ ] Code signing on all platforms (Authenticode / Developer ID + notarization / GPG) — CI fails if signing breaks
- [ ] Auto-update: signed packages verified before install; atomic + rollback-capable → [[02 Dependency & Supply Chain]]
- [ ] Update channels (stable/beta) with staged rollout; security fixes can force-update
- [ ] Fresh install, upgrade (N-1 → N), and uninstall tested per OS

## OS Integration

- [ ] Single-instance behavior: second launch focuses the existing window
- [ ] File associations / `Open with` work with unicode and spaced paths (Thai included)
- [ ] Notifications via OS APIs; tray and autostart opt-in only
- [ ] Global shortcuts remappable and non-colliding

## Window & Display

- [ ] HiDPI crisp; multi-monitor window state restored correctly
- [ ] Full keyboard reachability; no focus traps
- [ ] OS text scaling, high contrast, and reduced motion respected

## Data & Offline

- [ ] Local schema versioned; migration from the oldest supported version tested → [[03 Migration Backup & Scaling]]
- [ ] Offline behavior defined: what works, what queues, conflict resolution
- [ ] User data exportable in an open format

## Accessibility

- [ ] Screen reader reads the core flows (tested with VoiceOver/Narrator/Orca)
- [ ] No color-only signals; control target sizes meet platform minimums
- [ ] Accessibility gaps documented

## Reliability & Observability

- [ ] Crash reporting with consent; symbols uploaded; PII scrubbed
- [ ] Logs rotate and live in a documented location
- [ ] Session restore / unsaved-work protection on crash
- [ ] About/doctor dialog shows version, OS, and update state

## Testing & Release

- [ ] E2E smoke runs on the packaged artifact per OS, not just dev builds → [[03 CI-CD & Headless Testing]]
- [ ] Minimum OS versions documented and in the CI matrix
- [ ] Telemetry disclosed and toggleable; permissions minimal and explained
- [ ] Rollback plan for a bad release (previous installer retained; update can be reverted)

---

## Sources

- Distilled from platform HIGs (Apple, Microsoft, GNOME/KDE) and your vaults: Security, QA.
- Original deep reference: `../desktop-checklist/desktop.md`
