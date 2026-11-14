# Flutter Desktop Checklist

> Framework-specific checklist for building and shipping desktop apps with Flutter.
> Companion to the general [[desktop]] checklist — that one is the contract, this one is the plumbing.
> Last updated: 2026-11-14 (created: desktop framework layer)

---

## 1. Shared Codebase, Desktop Deltas

- [ ] **One Dart codebase, three desktop targets:** the widget layer is shared with mobile → [[flutter]], but desktop is a windowed environment — don't ship a stretched phone UI.
- [ ] **Window shell, not pushed screens:** wide layouts favor master-detail panes, sidebars, and split views over a navigation stack of full-screen routes.
- [ ] **No system back button:** navigation must be self-evident (persistent panes, breadcrumbs, explicit close) — users never get a hardware back gesture.
- [ ] **Resize is the default user action:** every screen survives arbitrary window sizes; enforce minimum window size instead of letting layouts collapse → [[desktop]] §6.
- [ ] **Process model is single-app, multi-window:** background work shares the UI process budget — design against [[desktop]] §1, not the mobile lifecycle.

## 2. Window Management

- [ ] **window_manager-class package for window control:** size, position, minimum size, title-bar behavior, always-on-top, maximize/restore events — the framework alone doesn't expose these.
- [ ] **Window state persisted and restored:** size/position/maximized/fullscreen saved on exit, restored per monitor (remember which display) → [[desktop]] §6.

```dart
// Restore before showing the window; save on close
final saved = settings.windowBounds; // your persisted rect
if (saved != null) await windowManager.setBounds(saved);
await windowManager.show(); // only once the first frame is ready
```
- [ ] **Multi-window = separate engine and Dart isolate state:** no shared globals between windows by default; communicate via explicit channels/message ports or a shared persistence layer.
- [ ] **Per-window state is per-window:** route stacks, selections, and unsaved drafts belong to the window that owns them — decide what "New Window" clones.
- [ ] **Close semantics defined:** quit-on-last-window vs keep-running-in-tray; a tray-resident app must not die when its last window closes → [[desktop]] §5.
- [ ] **Custom frameless windows are opt-in:** if you draw your own title bar (window_manager-class), you own drag regions, resize edges, min/max/close buttons, and double-click-to-maximize.

## 3. Mouse & Keyboard-First UX

- [ ] **Right-click context menus everywhere they help:** `GestureDetector.onSecondaryTapDown` (and `onTertiaryTapDown` for middle click) — touch-only apps forget this.
- [ ] **Hover affordances are visible:** InkWell hover states, tooltips on icon-only buttons, and cursor changes (`MouseRegion` / `SystemMouseCursors.click`) so clickability is discoverable.
- [ ] **Shortcuts built on Shortcuts/Actions/CallbackShortcuts:** one action wired to menu, toolbar, and key binding — never three divergent code paths.

```dart
CallbackShortcuts(
  bindings: {
    LogicalKeySet(LogicalKeyboardKey.control, LogicalKeyboardKey.keyN): _newDoc,
    LogicalKeySet(LogicalKeyboardKey.control, LogicalKeyboardKey.keyS): _save,
  },
  child: const EditorPane(),
)
```

- [ ] **Modifier keys match the OS:** Ctrl on Windows/Linux, Cmd on macOS (a single `Platform.isMacOS` switch) — and shortcuts don't collide with OS/common-app conventions → [[desktop]] §5.
- [ ] **Focus order is deliberate:** `FocusTraversalGroup` groups panes; every action is keyboard-reachable; no keyboard traps → [[desktop]] §6.
- [ ] **Resizable panes behave:** draggable split dividers with sane min/max pane widths; drag-and-drop between panes where meaningful.

## 4. Platform Integration

- [ ] **MethodChannel or FFI (dart:ffi) only when no package exists:** Pigeon for typed channel contracts; every message validated on both sides → [[flutter]] → [[desktop]] §1.
- [ ] **Native menu bar on macOS via `PlatformMenuBar`:** in-window `MenuBar` elsewhere; menu items invoke the same actions as shortcuts and show accelerators.

```dart
PlatformMenuBar(menus: [
  PlatformMenu(label: 'File', menus: [
    PlatformMenuItem(
      label: 'New',
      shortcut: const SingleActivator(LogicalKeyboardKey.keyN, control: true),
      onSelected: actions.newDoc,
    ),
  ]),
], child: const AppHome())
```
- [ ] **System tray via tray_manager-class:** tray icon states, context menu, and an explicit interplay with single-instance behavior and close semantics → [[desktop]] §5.
- [ ] **Notifications through the OS API, per-OS support verified first:** desktop plugin coverage is uneven across Windows/macOS/Linux — never promise notifications before confirming each target.
- [ ] **File associations, `Open with`, and launch arguments:** forwarded from OS launch to the right window (or spawned one); survives spaces and unicode (Thai included) → [[desktop]] §5.
- [ ] **Deep links / custom protocol handlers are untrusted input:** registered per-OS, parsed defensively → [[desktop]] §2 → [[security]].
- [ ] **Drag-and-drop of files (desktop-only interaction):** desktop_drop-class or equivalent; visible drop-target hover state; multiple files and directories handled.
- [ ] **Autostart and global shortcuts are opt-in:** launch_at_startup-class for "start at login"; hotkey_manager-class for system-wide hotkeys — both remappable, both user settings → [[desktop]] §5.

## 5. Packaging & Signing per OS

- [ ] **`flutter build windows/macos/linux` gives raw artifacts, not installers:** the last mile (installer, signing, notarization) is your work — budget it → [[desktop]] §3.
- [ ] **Windows: MSIX (msix package) or Inno Setup/NSIS-class installer:** Authenticode-signed; per-user vs machine install decided → [[desktop]] §3.
- [ ] **macOS: signed .app bundle in a DMG/pkg with hardened runtime on:** entitlements declared (network client, user-selected files, …) and notarization via `notarytool` — Gatekeeper blocks un-notarized apps, full stop.
- [ ] **Linux: deb/rpm/AppImage/Flatpak chosen deliberately:** flutter_distributor-class tooling helps; GPG signing where the format supports it; Flatpak adds sandbox rules for tray/notifications → [[desktop]] §3.
- [ ] **Signing + notarization run in CI and fail loudly** — they rot silently otherwise → [[desktop]] §11.
- [ ] **Clean-machine install/upgrade/uninstall tested per OS:** uninstall removes files, registry/autostart entries, and offers user-data removal → [[desktop]] §3 → [[qa]].
- [ ] **SBOM covers the bundled Flutter engine and native artifacts,** not just pub dependencies → [[desktop]] §2 → [[security]].

## 6. Updates

- [ ] **No built-in auto-update — choose a strategy:** auto_updater-class package with a signed feed, or store/side-load distribution. Decide before release, not after → [[desktop]] §4.
- [ ] **Update packages signed and verified before install:** feed over HTTPS, signature check mandatory → [[desktop]] §2 → [[security]].
- [ ] **Updates are atomic and rollback-capable:** a failed update never bricks the install; previous version recoverable → [[desktop]] §4.
- [ ] **Channels and staged rollout:** stable/beta feeds, gradual rollout, forced path for security fixes; update UX never interrupts work → [[desktop]] §4.
- [ ] **Update-check failures are invisible:** server down ≠ app broken → [[desktop]] §4.

## 7. Rendering & Performance

- [ ] **Impeller is the renderer — profile with it:** Skia-era shader warm-up tricks don't apply; measure on real desktop GPUs and on software rendering (VMs, remote desktop) → [[desktop]] §9.
- [ ] **Always measure in profile mode:** `flutter run --profile` — debug-mode numbers are fiction → [[flutter]].
- [ ] **First-frame budget defined; no white flash:** hold the window hidden (window_manager-class visibility) until the first frame renders, then show it.
- [ ] **Memory per window is engine-sized:** each additional window carries its own engine; leak-check open/close window cycles and set an idle-memory target → [[desktop]] §9.
- [ ] **Polite when minimized or hidden:** pause timers, animations, and polling on window-minimize events — near-zero CPU in the background → [[desktop]] §9.

## 8. Accessibility

- [ ] **Semantics tree verified with real screen readers:** VoiceOver (macOS), Narrator (Windows), Orca (Linux) — the bridge to platform accessibility APIs works, but only real readers prove it → [[desktop]] §8.
- [ ] **Keyboard navigation is an accessibility feature on desktop:** focus order sane, focus visible, no traps; keyboard-only completes the core task → [[desktop]] §6.
- [ ] **Custom-drawn and icon UI carries semantics labels:** a tooltip is not a name; buttons, inputs, and status changes expose names/roles/states.
- [ ] **Text scaling and OS display scale both honored;** controls meet minimum target sizes at every scale → [[desktop]] §8.
- [ ] **High contrast and reduced motion respected:** OS settings drive appearance; no color-only signals; known gaps documented for regulated contexts → [[desktop]] §8 → [[qa]].

## 9. Storage & Data Paths

- [ ] **path_provider for all filesystem locations:** desktop paths are per-user OS directories (and differ per OS) — never assume a working-directory-relative path.
- [ ] **Local DBs (drift-class) live at platform-correct paths:** schema versioned, migrations tested from the oldest supported version → [[desktop]] §7.
- [ ] **Multi-window data access has a single-writer discipline:** SQLite locking or one owning connection; concurrent windows never race on the same files → [[desktop]] §6.
- [ ] **Secrets in OS credential stores:** flutter_secure_storage maps to Keychain (macOS) / DPAPI-class protection (Windows) / Secret Service (Linux) — never plaintext config → [[security]].
- [ ] **Export/backup path exists:** open-format user-data export and corruption recovery defined → [[desktop]] §7.

## 10. Testing & Release Engineering

- [ ] **integration_test runs on desktop targets:** `flutter test integration_test -d windows|macos|linux` in CI per OS — desktop is not covered by mobile pipelines → [[qa]].
- [ ] **Golden tests at desktop sizes:** multiple window widths (pane collapsed/expanded), scale factors, and light/dark — not one phone-shaped golden → [[qa]].
- [ ] **Window-interaction tests:** open/close windows, state restore on relaunch, multi-window flows, close-vs-tray behavior.
- [ ] **Packaged-build smoke tests:** automation runs against the real installer artifact per OS, not just the dev build → [[desktop]] §11.
- [ ] **Upgrade test N-1 → N:** local DB and window state intact; rollback path tested too → [[desktop]] §11.

## 11. Distribution Reality Check

- [ ] **The desktop packaging/integration ecosystem is thinner than Electron/Tauri's:** tray, notifications, autostart, protocol handlers all lean on community packages — audit each before committing → [[security]].
- [ ] **Windows and macOS are the polished targets; Linux is viable but rougher:** packaging, tray, and notifications vary by distro/desktop environment — test the DEs you claim.
- [ ] **Per-OS support is a deliberate decision:** minimum OS versions documented and tested; unsupported OSes stated up front → [[desktop]] §11.
- [ ] **If deep OS integration dominates the product** (global menus, heavy tray workflows, complex protocol handling), re-evaluate the framework choice early — not after the shell is built.

---

## Quick Sanity Check Before Release

- [ ] Window size/position/maximized state restores on a second monitor at a different scale factor
- [ ] Keyboard-only run of the core task: every action reachable, shortcuts follow OS conventions, focus order sane
- [ ] Right-click shows context menus; hover states and cursors correct on every clickable control
- [ ] macOS build is notarized — opens on a clean Mac with no right-click workaround
- [ ] Windows installer is Authenticode-signed; clean install → launch → uninstall leaves nothing behind
- [ ] Second window opens without crashing or corrupting state; last-window close matches the quit-vs-tray spec
- [ ] First frame renders with no white flash; CPU near zero while minimized
- [ ] VoiceOver/Narrator reads the core flow through the Semantics tree

---

## Sources

- Flutter desktop documentation: docs.flutter.dev (Building for Windows/macOS/Linux, Desktop platform integration, Desktop deployment).
- `Master checklist: [[desktop]].`
- `Mobile sibling: [[flutter]].`
- Testing strategy: [[qa]] · Threat model and supply chain: [[security]].
