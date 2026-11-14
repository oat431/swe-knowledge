# .NET Desktop Checklist

> Framework-specific checklist for building and shipping desktop apps on .NET (WinUI / WPF / MAUI / Avalonia).
> Companion to the general [[desktop]] checklist — that one is the contract, this one is the plumbing.
> Last updated: 2026-11-14 (created: desktop framework layer)

---

## 1. Framework Choice & Project Shape

- [ ] **WinUI (Windows App SDK) when the app is Windows-only and modern:** Fluent styling, MSIX-first packaging, current Microsoft investment; the newest Windows UI stack.
- [ ] **WPF when Windows-only and mature enterprise:** huge ecosystem, battle-tested data tooling, existing skill base; still actively developed, not legacy.
- [ ] **.NET MAUI when one codebase must span Windows + macOS + iOS + Android:** shared XAML + C# with per-platform handlers; expect per-platform work, not zero.
- [ ] **Avalonia when true cross-platform including Linux matters:** XAML dialect, own rendering stack, active development; the pragmatic choice beyond the Apple/Microsoft OS matrix.
- [ ] **Decision driven by OS matrix + team skills:** XAML experience transfers across all four; platform integration knowledge does not — pick the framework your team can debug at 2am.
- [ ] **MVVM hygiene everywhere:** ViewModels are plain classes raising `INotifyPropertyChanged`; no UI types (Window, Control) referenced from ViewModels; views stay thin → [[desktop]] §1
- [ ] **Composition root in app startup:** one DI container (generic-host style) wires services and ViewModels; no service-locator lookups scattered through code → [[desktop]] §1
- [ ] **Multi-window/process policy explicit:** these frameworks run one process — document what shares state across windows and how → [[desktop]] §1

## 2. XAML & UI Hygiene

- [ ] **Resource dictionaries merged deliberately:** shared styles/templates in library-level dictionaries; per-theme dictionaries swapped for light/dark, not copy-pasted pages.
- [ ] **Bindings implement `INotifyPropertyChanged` faithfully:** bind to properties, never fields; use compiled bindings where the framework offers them (x:Bind / XamlC-class) to catch name errors at build time.

```csharp
public sealed class MainViewModel : INotifyPropertyChanged
{
    private string _title = "";
    public string Title { get => _title; set { _title = value; PropertyChanged?.Invoke(this, new(nameof(Title))); } }
    public event PropertyChangedEventHandler? PropertyChanged;
}
```
- [ ] **Event-handler unsubscription is a discipline:** static events and long-lived services holding view references are the classic leak — unsubscribe on close or use weak-event patterns → [[desktop]] §9
- [ ] **UI thread affinity respected:** all UI updates marshaled through the dispatcher; never block the UI thread with `.Result`/`.Wait()` (deadlocks the message pump).
- [ ] **Styles and templates keep naming contracts:** template parts named consistently; control defaults come from one style layer, not per-instance property piles.
- [ ] **Theming follows the OS:** light/dark switch with system settings; custom brushes get theme variants so nothing renders invisible in the wrong theme → [[desktop]] §8

## 3. Packaging & Signing

- [ ] **MSIX for WinUI/MAUI on Windows:** store distribution or sideload with App Installer; declarative manifest owns identity, capabilities, file associations → [[desktop]] §3
- [ ] **Classic installers (MSI/NSIS/WiX-class) when enterprise deployment demands it:** GPO/SCCM paths, per-user vs system install decided up front → [[desktop]] §3
- [ ] **macOS bundle (MAUI) is signed and notarized before distribution:** Gatekeeper blocks unsigned/un-notarized builds — signing runs in CI and fails loudly → [[desktop]] §3
- [ ] **Authenticode signing on every Windows artifact:** timestamped signatures so binaries stay valid after cert expiry; cert storage and rotation documented → [[security]]
- [ ] **Trimming and single-file publish treated as risky, not free:** WPF/XAML is reflection-heavy (XAML loading, serializers, binding) and trimming analysis cannot preserve everything — test the trimmed artifact end-to-end or ship untrimmed.

```xml
<PublishTrimmed>true</PublishTrimmed> <!-- only with full smoke on the trimmed artifact; WPF reflection paths are not fully analyzable -->
```
- [ ] **Self-contained vs framework-dependent decided explicitly:** self-contained ships the runtime (larger, guaranteed); framework-dependent requires the runtime installed — support policy matches the choice.
- [ ] **Per-RID publish matrix built and exercised:** win-x64/win-arm64, osx-arm64/osx-x64, linux-x64 as applicable — the arm64 build is tested, not merely produced → [[desktop]] §11
- [ ] **Dependencies scanned; SBOM covers the bundled runtime and native libraries** → [[02 Dependency & Supply Chain]]

## 4. Updates & Distribution

- [ ] **One update owner, chosen deliberately:** MSIX package updates (store or App Installer feed) or an installer-based update framework (Squirrel-class) — never two half-integrated mechanisms → [[desktop]] §4
- [ ] **Store distribution vs sideload decided:** the store owns updates and trust for you; sideloaded installs need their own signed update feed and trust story.
- [ ] **Staged rollout with channels:** stable/beta feeds, gradual rollout, and a forced path for security fixes → [[desktop]] §4
- [ ] **Updates atomic and rollback-capable:** a failed update never bricks the install; the previous version stays recoverable → [[desktop]] §4
- [ ] **Update UX polite and failure-invisible:** silent or notified; release notes surfaced; server down ≠ app broken → [[desktop]] §4
- [ ] **Update packages signed and verified before install:** mandatory, not advisory → [[02 Dependency & Supply Chain]]

## 5. OS Integration (Windows-first)

- [ ] **Toast notifications through the OS notification API:** permission-aware; action buttons deep-link into the right app state → [[desktop]] §5
- [ ] **Jump lists, taskbar progress, and tray icons opt-in and pinned:** tray support is not uniform across frameworks (WinUI-class apps need a helper library) — vet and pin the helper like any dependency.
- [ ] **File associations declared in the package manifest:** argument forwarding from OS launch survives spaces and unicode (Thai included) → [[desktop]] §5
- [ ] **Autostart is a user setting, never a default:** packaged startup task or registry Run entry — both opt-in → [[desktop]] §5
- [ ] **URI activation / deep links parse untrusted input:** protocol handlers receive attacker-controlled strings — validate, never pass through → [[desktop]] §2
- [ ] **Clipboard and drag-drop validated:** dropped file paths and pasted content are untrusted input; never execute or auto-open blindly → [[desktop]] §2
- [ ] **Single-instance behavior implemented explicitly:** second launch activates the first window (activation/mutex pattern); `--new-window` handled deliberately → [[desktop]] §5

## 6. Accessibility

- [ ] **Automation peers exposed for every interactive element:** standard controls ship working peers (WPF/WinUI AutomationPeer model); custom controls need custom peers with name, role, and state → [[desktop]] §8
- [ ] **Tested with Narrator, not just the API:** read-through of the core flows; Avalonia maps to per-OS accessibility APIs — verify against Orca/VoiceOver on non-Windows before claiming support.
- [ ] **Keyboard-only completes the core task:** sane tab order, access keys/mnemonics on actions, visible focus, no keyboard traps → [[desktop]] §6
- [ ] **High contrast themes honored:** system colors drive appearance in high-contrast mode; no color-only signals → [[desktop]] §8
- [ ] **Text scales with OS/DPI settings:** device-independent units throughout; controls hold up at large scale factors → [[desktop]] §6
- [ ] **Known accessibility gaps documented** for regulated contexts → [[desktop]] §8

## 7. Performance & Startup

- [ ] **Cold-start budget defined and measured:** splash screen while the shell composes; lazy-create views on first navigation, not all at startup → [[desktop]] §9
- [ ] **Startup XAML cost bounded:** defer non-critical resource dictionaries and service init until after the first frame renders.
- [ ] **Memory footprint measured and bounded:** desktop .NET apps can be lean — set an idle working-set target; leak-check open/close window cycles (event handlers are the usual suspect) → [[desktop]] §9
- [ ] **List virtualization on for large collections:** virtualizing panels by default; batch `ObservableCollection` updates instead of per-item raises.
- [ ] **Rendering verified on low-end GPUs and software fallback:** avoid gratuitous blur/effect stacks; confirm the app renders where hardware acceleration is missing → [[desktop]] §9
- [ ] **Background behavior polite:** timers, polling, and animations pause when minimized; near-zero CPU when hidden → [[desktop]] §9

## 8. Reliability & Crash Reporting

- [ ] **Visual Studio App Center is retired — do not adopt it:** use a maintained crash-reporting SDK (Sentry-class) or Windows Error Reporting as the backend → [[desktop]] §10
- [ ] **Every exception path lands in a handler:** UI-thread unhandled exceptions, background thread exceptions, and unobserved task exceptions — all logged and reported, never silently swallowed.

```csharp
AppDomain.CurrentDomain.UnhandledException += OnUnhandled;  // last resort — log, report, then let the process die
TaskScheduler.UnobservedTaskException += OnUnobserved;      // fire-and-forget tasks die here silently otherwise
```
- [ ] **Crash reporting with user consent:** minidumps + symbols uploaded; PII scrubbed before upload → [[desktop]] §10
- [ ] **Local logs rotate and are findable:** documented path users can send to support → [[desktop]] §10
- [ ] **Session restore / unsaved-work protection** where feasible; state persisted on suspend/close → [[desktop]] §7
- [ ] **About/doctor dialog ships:** version, OS, update state, data paths — support's first question answered in-app → [[desktop]] §10

## 9. Testing

- [ ] **Unit tests target ViewModels and services:** plain classes with `INotifyPropertyChanged` need no UI framework — property-change notifications asserted explicitly (silent binding failures are invisible at runtime) → [[qa]]
- [ ] **UI automation runs on packaged builds:** Appium-class drivers or platform UI-Automation-based tooling drive the real installer artifact — tooling status shifts, so pin and verify your driver stack → [[desktop]] §11
- [ ] **Packaged smoke tests per OS in CI:** launch, sign in, complete the core task on the actual MSIX/installer output → [[desktop]] §11
- [ ] **Upgrade tests:** install N-1 → upgrade to N → data and settings intact; rollback path tested too → [[desktop]] §4
- [ ] **Build hygiene enforced in CI:** `dotnet build` warnings-as-errors, publish matrix green, signing step fails the pipeline when broken → [[desktop]] §11
- [ ] **OS support matrix maintained:** minimum versions documented and tested per framework choice; old OSes dropped deliberately → [[desktop]] §11

## 10. macOS & Linux Reality (MAUI / Avalonia)

- [ ] **Menus redesigned per platform:** macOS keeps the menu bar at the top of the screen (window menus don't exist there) — menu structure is a per-OS deliverable, not a shared one → [[desktop]] §5
- [ ] **Signing per OS, each with its own gate:** Authenticode (Windows), Developer ID + notarization (macOS), package signing/verification (Linux) → [[desktop]] §3
- [ ] **Platform handlers/backends tested, not just compiled:** windowing semantics (close vs quit on macOS), dialogs, and file pickers differ by platform handler — exercise each target OS → [[desktop]] §6
- [ ] **Paths and config follow platform conventions:** per-OS config/data directories, path separators, and case-sensitive filesystems (Linux) verified.
- [ ] **Linux packaging chosen for the audience:** AppImage/Flatpak/deb per distribution story; rendering verified on both display-server stacks in common use → [[desktop]] §3
- [ ] **Per-OS accessibility and input reality checked:** keyboard layouts, IME input, and screen readers differ per OS — the Windows test pass does not certify macOS/Linux → [[desktop]] §8

---

## Quick Sanity Check Before Release

- [ ] Clean-machine install from the real MSIX/installer per OS: launches, signs in, completes the core task
- [ ] Trimmed/single-file build (if shipped) passes the full smoke suite — or trimming is explicitly off with a comment why
- [ ] Narrator reads the core flows; keyboard-only completes the core task; high contrast renders correctly
- [ ] A deliberate unhandled exception reaches the crash backend (and the report shows up, with symbols)
- [ ] Upgrade from the previous version preserves data and settings; rollback path proven
- [ ] Startup time and idle memory measured on a low-end machine, both within budget
- [ ] The release pipeline cannot produce an unsigned artifact on any OS

---

## Sources

- WinUI / Windows App SDK: https://learn.microsoft.com/en-us/windows/apps/winui/
- WPF docs: https://learn.microsoft.com/en-us/dotnet/desktop/wpf/
- .NET MAUI docs: https://learn.microsoft.com/en-us/dotnet/maui/
- Trimming incompatibilities (WPF reflection caveats): https://learn.microsoft.com/en-us/dotnet/core/deploying/trimming/incompatibilities
- App Center retirement (why not to adopt it): https://learn.microsoft.com/en-us/appcenter/retirement
- Avalonia accessibility (automation peers): https://docs.avaloniaui.net/docs/app-development/accessibility
- Avalonia docs: https://docs.avaloniaui.net/
- Master checklist: [[desktop]].
