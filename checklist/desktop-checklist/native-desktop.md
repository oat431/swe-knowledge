# Native Desktop Checklist

> Framework-specific checklist for building and shipping native desktop apps (Qt / GTK / SwiftUI).
> Companion to the general [[desktop]] checklist — that one is the contract, this one is the plumbing.
> Last updated: 2026-11-14 (created: desktop framework layer)

---

## 1. Toolkit Choice

- [ ] **SwiftUI/AppKit chosen when macOS is the product:** best platform integration — menu bar, Services, Quick Look, accessibility come free; AppKit for the corners SwiftUI hasn't reached.
- [ ] **Qt chosen for cross-platform native-feel from one codebase:** C++ or Qt for Python; strong on Windows/macOS/Linux with a consistent widget set that still respects platform look.
- [ ] **GTK chosen for Linux/GNOME-first apps:** C or Rust bindings; fits the GNOME platform (GSettings, portals, notifications) natively; viable on Windows/macOS at reduced polish.
- [ ] **Per-OS native (SwiftUI + WinUI + GTK) chosen when integration beats reuse:** three codebases, three platform-experience wins — decide this consciously, not by default → [[desktop]] §1
- [ ] **Core logic lives outside the toolkit layer:** business logic in toolkit-agnostic libraries; only views/widgets bind to the toolkit — so a toolkit swap or a headless mode stays possible.
- [ ] **Platform conventions honored per OS:** menu placement (macOS menu bar vs Windows window menus), button order, shortcut keys (⌘ vs Ctrl) — pick per platform, not one global compromise.
- [ ] **Qt's UI technology chosen deliberately:** Qt Quick (QML) for fluid/declarative UIs and touch, Widgets for dense traditional desktops — mixing them works but doubles the styling surface.

## 2. Licensing

- [ ] **Qt license path decided before code is written:** LGPL/GPL (community edition) vs commercial. Mixing commercial and community editions within one product is not allowed, and switching later is hard — the ship-blocker nobody budgets for.
- [ ] **LGPL compliance duties fulfilled when shipping Qt dynamically linked:** user can relink against a modified Qt, license notices included, source or written offer provided as the license requires. Static linking raises the bar — know it before you choose it.
  - Ship: copyright/license notices for every linked component
  - Keep: relink capability (dynamic linking keeps this simple)
  - Offer: corresponding source (or written offer) as the license text requires
- [ ] **Qt module licenses audited:** some modules and add-ons are commercial-only — verify every linked module's license before release, not after.
- [ ] **GTK LGPL obligations understood:** LGPL permits proprietary apps through shared-library linking; keep attribution and relink capability.
- [ ] **SwiftUI/AppKit are free with Xcode, but distribution is not:** Apple Developer Program membership is required for signing, notarization, and store submission.

## 3. Build & Packaging

- [ ] **Qt deployment tooling used, not hand-copied DLLs:** windeployqt / macdeployqt (and the Linux equivalents) gather runtime libs, plugins, and QML imports correctly → [[desktop]] §3
- [ ] **Qt for Python apps frozen/bundled for distribution:** standalone executable per OS; plugins and translations survive the freeze.
- [ ] **GTK apps ship Flatpak-first on Linux:** Flatpak as primary, deb/rpm where a store demands it; desktop entry, icon set, GSettings schemas, and typelib dependencies declared.
- [ ] **macOS app bundle is complete:** Info.plist (document types, URL schemes, usage strings), resources, frameworks, and universal or per-arch binaries built deliberately.
- [ ] **Per-OS installers built from the toolkit's native path:** MSI/MSIX or NSIS (Windows), DMG/pkg (macOS), Flatpak/deb/rpm (Linux) → [[desktop]] §3
- [ ] **Resources bundled through the toolkit's resource system:** Qt resource system / GResource — no loose files that break under sandbox or install layout changes.
- [ ] **Each package built on its native OS in CI:** macOS bundles on macOS hardware (signing/notarization require it), Windows installers on Windows, Flatpak on Linux — cross-building ships subtle ABI and signing bugs.

## 4. Signing & Notarization

- [ ] **macOS: Developer ID + hardened runtime + notarization is mandatory outside the store:** Gatekeeper blocks un-notarized downloads; hardened runtime is a prerequisite for notarization; entitlements kept minimal. Mac App Store distribution skips notarization because review includes equivalent checks.
- [ ] **Windows: Authenticode signing on every shipped binary and installer:** unsigned DLLs inside a signed installer still trip SmartScreen → [[desktop]] §3
- [ ] **Linux: Flatpak/Flathub verification and GPG-signed deb/rpm repos:** publishing unsigned packages teaches users to disable protections.
- [ ] **Entitlements documented and minimal:** sandbox, keychain-access groups, network — every entitlement justified; entitlements drift silently as features are added.
- [ ] **Signing and notarization run in CI and fail the build when broken** → [[desktop]] §11

## 5. Updates

- [ ] **Sparkle is the macOS auto-update standard:** signed update feeds (EdDSA signatures, verified against Apple code signing), delta updates, self-updating installs; works with Cocoa, SwiftUI, and Qt apps alike.
  - Feed (appcast) is HTTPS-hosted and signed per release; the public key ships inside the app
  - Delta updates optional but kind on users; full package always available as fallback
- [ ] **Windows updater matches that bar:** WinSparkle-class feed-based updater or equivalent; update packages signed and verified before install → [[security]]
- [ ] **Linux: Flatpak + OSTree updates:** atomic, rollback-capable; deb/rpm users get updates through the distro repo you control.
- [ ] **Update feeds are signed end to end and rolled out in stages:** stable/beta channels, gradual rollout, forced path for security fixes → [[desktop]] §4
- [ ] **A failed update never bricks the install;** previous version recoverable; update-check failures invisible to the user → [[desktop]] §4

## 6. OS Integration

- [ ] **Native menus done per platform:** macOS global menu bar with app menu and Services; Windows window menus with accelerators; don't force one model onto the other.
- [ ] **File associations registered per OS:** document types in Info.plist (macOS), registry associations (Windows), MIME types + desktop entry (Linux); `Open with` argument forwarding survives spaces and unicode → [[desktop]] §5
- [ ] **macOS platform strengths wired up where they fit:** Services, Quick Look previews, dock/termination behavior, proxy icons — cheap wins that web frameworks can't match.
- [ ] **URL scheme / deep links registered and validated per OS:** scheme handlers parse untrusted input → [[desktop]] §2
- [ ] **Tray/menu-bar icon per platform conventions:** macOS menu bar extra, Windows notification area, Linux status notifier; autostart opt-in only → [[desktop]] §5
- [ ] **Sandboxed Linux apps use xdg-desktop-portal** for file pickers, screenshots, and opening URLs instead of fighting the sandbox.
- [ ] **Native file dialogs and common panels used,** not toolkit-reimplemented ones — users get recent places, sidebar, and provider integration free.

## 7. Accessibility (The Native Advantage)

- [ ] **Platform accessibility APIs exposed properly:** NSAccessibility (macOS), UI Automation (Windows), AT-SPI (Linux) — the toolkit bridges these for standard widgets; verify the bridge actually fires → [[desktop]] §8
- [ ] **Tested with the real screen readers:** VoiceOver, Narrator, Orca — complete the core flow eyes-free, not just an API dump.
- [ ] **Custom-drawn widgets implement the accessibility contract:** name, role, value, state, actions — the classic native pitfall is a beautiful canvas widget that is invisible to assistive tech.
  - Expose at minimum: role, name, value/state, and the actions a pointer user gets
  - Announce dynamic changes (live regions / value changes), not just static structure
- [ ] **Keyboard navigation complete:** logical focus order, no traps, visible focus indication, all actions reachable without a pointer → [[desktop]] §6
- [ ] **High contrast, reduced motion, and OS text scaling respected;** no color-only signals → [[desktop]] §8

## 8. Localization & Input

- [ ] **IME correctness on every text input (CJK/Thai tested with a real IME):** composition/preedit rendering and commit behavior — native controls handle this best; custom text widgets must reimplement the contract or pick a toolkit text engine.
  - Preedit (candidate/composition) renders in place with the IME's underline conventions
  - Commit, cancel, and focus-change mid-composition leave consistent state
- [ ] **Dead keys, compose sequences, and non-QWERTY layouts tested:** keyboard handling follows physical/logical key semantics, not hardcoded keycodes.
- [ ] **RTL layout mirrors correctly:** bidi text, icon mirroring, layout direction flips with locale.
- [ ] **Per-locale formatting used:** dates, numbers, collation, plurals — from the platform/locale APIs, never hand-rolled.
- [ ] **String extraction pipeline exists:** Qt Linguist / gettext / String Catalogs — translators work from files, not source code; pseudo-localization catches hard-coded strings.
- [ ] **Font fallback chains verified per script:** no tofu boxes for Thai, CJK, or Arabic on a fresh OS install.

## 9. Performance & Footprint

- [ ] **Native footprint advantage is measured, not assumed:** idle memory and install size compared against the framework baseline you'd have used otherwise → [[desktop]] §9
- [ ] **Hardware acceleration used where available; software rendering fallback works** on VMs and locked-down machines → [[desktop]] §9
- [ ] **Cold-start budget defined and measured on packaged builds** (not dev runs); heavy init deferred past first frame → [[desktop]] §9
- [ ] **No main-thread blocking work:** long operations off the UI thread via the toolkit's worker/async facilities; UI stays responsive under load.
- [ ] **Custom-drawn views respect the frame budget:** no per-frame allocation or unbounded repaints; invalidation regions used so unchanged pixels aren't re-rendered.

## 10. Testing

- [ ] **Platform-native test frameworks used:** XCTest/XCUITest-class (macOS), Qt Test + its UI tooling (Qt), AT-SPI-based automation (Linux) → [[qa]]
- [ ] **Smoke E2E runs on packaged builds per OS:** the real installer artifact, not the dev tree → [[desktop]] §11
- [ ] **Accessibility regression coverage:** screen-reader labels/states asserted for core widgets; custom widgets carry a11y tests alongside functional ones.
- [ ] **Upgrade tests per OS:** version N-1 → N with data intact; uninstall clean → [[desktop]] §11
- [ ] **Theme/HiDPI/multi-monitor checks in CI or release checklist:** dark mode, scale factors, mixed-DPI monitors — native rendering bugs hide here.
- [ ] **Pseudo-localized build tested:** exaggerated translations expose hard-coded strings, truncated labels, and layouts that break under longer text before real translators ever see the app.

## 11. Per-OS Support Matrix

- [ ] **Three toolkits = three support matrices:** each toolkit/OS pairing declares its own minimum OS versions, tested deliberately — "it builds" is not "it's supported."
  - "Supported" = tested on that toolkit × OS pairing, not merely compilable
  - Minimum, tested, and oldest CI-covered OS versions documented per platform
- [ ] **Dropped platforms are dropped with notice:** docs and release notes say what was dropped and when → [[desktop]] §11
- [ ] **Platform-divergent behavior is documented:** where the three toolkits can't match (Services on macOS only, Flatpak on Linux only), the gap is a documented decision, not a surprise in the bug tracker.
- [ ] **CI covers every supported toolkit × OS pair;** an uncovered pair is an unsupported pair.
- [ ] **Toolkit runtime covered in the SBOM:** the bundled Qt/GTK/framework runtime and its plugins ship in the dependency inventory → [[security]]

---

## Quick Sanity Check Before Release

- [ ] Fresh install per OS from the real installer: launches, core task completes
- [ ] macOS build is notarized and opens without Gatekeeper intervention
- [ ] Update feed signed and tested: old build updates to new build, rollback path works
- [ ] VoiceOver / Narrator / Orca complete the core flow; keyboard-only works end to end
- [ ] IME composition (CJK or Thai) works in every text field, including custom widgets
- [ ] License obligations done: LGPL notices/relink info shipped, every Qt module license audited
- [ ] Uninstall per OS is clean; user data handled as promised

---

## Sources

- Qt licensing (commercial vs open source): https://www.qt.io/licensing
- Qt deployment tooling: https://doc.qt.io/qt/deployment.html
- GTK project & licensing: https://www.gtk.org/
- Notarizing macOS software before distribution: https://developer.apple.com/documentation/security/notarizing-macos-software-before-distribution
- Hardened Runtime: https://developer.apple.com/documentation/security/hardened-runtime
- Sparkle update framework: https://sparkle-project.org/
- Flatpak documentation: https://docs.flatpak.org/
- Master checklist: [[desktop]].
