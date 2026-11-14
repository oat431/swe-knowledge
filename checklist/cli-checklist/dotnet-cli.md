# .NET CLI Checklist

> Framework-specific checklist for building and shipping CLIs on .NET (System.CommandLine / Spectre.Console).
> Companion to the general [[cli]] checklist — that one is the contract, this one is the plumbing.
> Last updated: 2026-11-14 (created: CLI framework layer)

---

## 1. Project Setup & Tool Choice

- [ ] **System.CommandLine is the default:** the official parser (NuGet package `System.CommandLine`), GA after years of prerelease; it powers the .NET CLI itself and is documented trim-friendly.
- [ ] **Spectre.Console.Cli when you want batteries included:** attribute-driven settings classes (`CommandSettings`, `[CommandOption]`) plus a `CommandApp` runner with DI support — richer out-of-the-box DX at the cost of a third-party dependency.
- [ ] **CommandLineParser-class libraries only for legacy codebases:** fine to keep where they already work, but new tools should start from the two above — pick one parser and hold it.
- [ ] **Top-level statements keep `Program.cs` thin:** the entry point wires the command tree (or `CommandApp`) and returns the run result; real logic lives in testable classes.
- [ ] **Pin the SDK with `global.json`:** dev machines and CI build with the same SDK, so behavior doesn't drift with whatever is installed.
- [ ] **Parser-agnostic core:** handlers delegate to application services so behavior is testable without exercising parsing.
- [ ] **Build hygiene from day one:** treat warnings as errors in the csproj (warnings become build failures before they become runtime surprises).

## 2. Commands, Options & Binding

- [ ] **Model the grammar with `Command`, `Option<T>`, `Argument<T>`:** `RootCommand` at the entry point; `Subcommands`/`Options`/`Arguments` collections assemble the tree (generic types parse and bind to typed values for free).
- [ ] **Actions read typed values from the parse result:** `SetAction` handlers pull typed values via `GetValue(...)` (typed values in, exit code out); the handler stays small and the edge does the parsing.
- [ ] **One subcommand per verb:** each distinct operation is its own `Command`; share options only where they truly apply → [[cli]] §1
- [ ] **Global options via `Recursive = true`:** `--verbose`, `--output`, `--config` defined once, available on every subcommand.
- [ ] **Validate at parse time:** `Required = true`, `DefaultValueFactory`, and `Validators` (reporting with `AddError`) reject bad input before any work runs; `AcceptOnlyFromAmong` pins enum-like values (usage error, not runtime error) → [[cli]] §1
- [ ] **Help customized through help builders:** every command and subcommand carries real examples; `--help` and `--version` come from `RootCommand` and answer instantly → [[cli]] §1
- [ ] **Tab completion and response files come from the parser:** completion generated from the same command tree that parses input, plus built-in `@response.rsp` support — no hand-rolled completion scripts → [[cli]] §1

```csharp
var root = new RootCommand("Ship artifacts");
var target = new Argument<string>("target") { Description = "What to build" };
var force = new Option<bool>("--force", "-f") { Description = "Overwrite output" };
root.Arguments.Add(target);
root.Options.Add(force);
root.SetAction(parseResult => Run(parseResult.GetValue(target), parseResult.GetValue(force)));
root.Parse(args).Invoke();
```

## 3. Rich Output & Interactivity

- [ ] **Use Spectre.Console for human output regardless of parser choice:** `AnsiConsole` tables, trees, prompts, progress bars, and status spinners size and wrap themselves to the terminal → [[cli]] §2
- [ ] **Inject `IAnsiConsole`, never the static `AnsiConsole`, in command classes:** tests capture output by substituting a test console instead of redirecting the process.
- [ ] **Spectre.Console.Cli's attribute model is the batteries-included alternative:** `[CommandArgument]`/`[CommandOption]` on a `CommandSettings` class; `Command<T>` returns its exit code from `Execute`, and `CommandApp.Run(args)` runs the tree.
- [ ] **Live rendering etiquette:** progress/status renderers go quiet when output is redirected — never fight this; piped output must carry no ANSI codes → [[cli]] §2
- [ ] **Prompts only where a human is present:** every prompt has a flag equivalent (`--yes`, `--force`) so CI never hangs on a question → [[cli]] §5
- [ ] **Machine output bypasses markup:** `--json` goes through a serializer straight to stdout; Spectre styling is for the human channel only → [[cli]] §2

## 4. Errors & Exit Codes

- [ ] **One exit-code mapping (usage vs runtime):** parse/validation failures and execution failures map to distinct, documented non-zero codes; handlers return the code or set `Environment.ExitCode` → [[cli]] §3
- [ ] **Root exception handler at the entry point:** catch everything; print an actionable message by default and the full exception plus stack only under `--debug` → [[cli]] §3
- [ ] **No stack traces by default:** errors state what failed, why, and the next step — and they go to stderr → [[cli]] §3
- [ ] **Usage errors end with a hint:** name the offending token and point at `--help`; never a bare "invalid input".
- [ ] **Decide who prints parse errors:** the default parse-and-invoke path reports them; if you handle `ParseResult.Errors` (`ParseError.Message`) yourself, keep one consistent error format → [[cli]] §3
- [ ] **Cancellation honored:** long-running handlers take cancellation tokens (Ctrl-C flushes output, cleans temp files, exits cleanly) → [[cli]] §3

## 5. Configuration & Settings

- [ ] **Layer config with Microsoft.Extensions.Configuration:** a `ConfigurationBuilder` combining `appsettings.json`, environment variables, and command-line overrides → [[cli]] §4
- [ ] **Options pattern for settings classes:** bind config sections to typed settings and validate at startup (bad config fails fast with a clear message).
- [ ] **Precedence defined and documented:** flags > env vars > config file > defaults → [[cli]] §4
- [ ] **User config lives in platform directories:** `%APPDATA%` on Windows, XDG config on Linux, `~/Library/Application Support` on macOS → [[cli]] §6
- [ ] **State and cache separated from config,** per-user and never in the working directory → [[cli]] §4

## 6. Testing

- [ ] **xUnit/NUnit + in-process invocation:** drive the command tree with an args array and capture console output; System.CommandLine is built so app code tests independently of parsing → [[cli]] §11, [[qa]]
- [ ] **`CommandAppTester` for Spectre.Console.Cli:** the testing package runs commands in-memory and returns `ExitCode`/`Output` for assertions; `TestConsole` queues prompt input for interactive paths.

```csharp
var app = new CommandAppTester();
var result = app.Run("build", "--verbose");
Assert.Equal(0, result.ExitCode);
Assert.Contains("done", result.Output);
```
- [ ] **Golden help snapshots:** assert `--help` output per command so help drift fails the build → [[cli]] §11
- [ ] **Exit codes tested as a contract:** success, usage-error, and runtime-error paths each assert their code → [[cli]] §11
- [ ] **Integration tests on the built binary:** run the published executable as a subprocess — the only honest test of real argv parsing and console behavior → [[cli]] §11
- [ ] **Pipe-mode test:** redirected output contains no ANSI codes and no live-renderer noise → [[cli]] §11

## 7. Packaging & Distribution

- [ ] **Self-contained vs framework-dependent is an audience decision:** self-contained single-file removes the runtime prerequisite at the cost of size; framework-dependent stays small but requires a shared runtime — document which one you ship → [[cli]] §8
- [ ] **Trimming and AOT need testing, not just flags:** `<PublishTrimmed>` and `<PublishAot>` builds break on reflection-based serialization and DI; System.CommandLine is trim-friendly and AOT-capable, your code may not be.
- [ ] **Pack as a .NET tool:** `dotnet pack` with tool packaging metadata; users install via `dotnet tool install` (global, or local behind a committed tool manifest) → [[cli]] §8
- [ ] **Publish per runtime identifier:** `dotnet publish -r <rid>` per target platform (win-x64, linux-x64, osx-arm64, ...); the CI matrix produces them side by side → [[cli]] §8

```bash
dotnet publish -c Release -r win-x64 -o publish/win-x64
dotnet pack -c Release   # then: dotnet tool install --global --add-source ./nupkg MyTool
```
- [ ] **Reach beyond NuGet:** winget manifest for Windows, Homebrew tap/formula for macOS and Linux → [[cli]] §8
- [ ] **MSIX for Windows Store-class distribution:** signed installers, managed updates, clean uninstall.
- [ ] **Uninstall path documented:** `dotnet tool uninstall` and the package-manager equivalents → [[cli]] §8

## 8. Windows-First Realities

- [ ] **What you type is not what the process receives:** cmd and PowerShell each re-quote or flatten arguments before your parser sees them — test argument passing from every shell you claim → [[cli]] §6
- [ ] **Spawn children with argument lists, not command strings:** `ProcessStartInfo.ArgumentList` with `UseShellExecute = false` avoids quoting re-parsing → [[02 Secure Coding Practices]]
- [ ] **Paths composed with path APIs (`Path.Combine`, `Path`):** no string concatenation; separators and case sensitivity differ across filesystems → [[cli]] §6
- [ ] **PowerShell is a first-class caller:** ship completion for it; example scripts pass argument arrays, never one interpolated string.
- [ ] **Handle console control events, not just POSIX signals:** Ctrl-C must reach cancellation tokens on Windows too → [[cli]] §6
- [ ] **The apphost executable is the Windows contract:** single-file publish emits `MyTool.exe` next to (or wrapping) the managed code — verify shim naming, PATH install location, and that the exe launches from cmd, PowerShell, and double-click.

## 9. Performance & Startup

- [ ] **Startup is a feature:** sub-100ms perceived for common commands; lazy-load heavy services until a command actually needs them → [[cli]] §10
- [ ] **Trimmed self-contained (or AOT) builds kill JIT warmup:** the biggest single win for CLI feel — measure cold start, not steady state.
- [ ] **Watch hidden first-run costs:** DI container build and serializer discovery are classic warmup traps on the hot path.
- [ ] **ReadyToRun as the middle ground:** precompiled code trades binary size for startup when full AOT is too restrictive (`<PublishReadyToRun>`) — measure both.
- [ ] **Stream large outputs; bound parallelism** → [[cli]] §10

## 10. Security

- [ ] **Secrets never in argv, logs, or help output** → [[02 Secrets Management]]
- [ ] **No shell interpolation of user input:** exec with `ProcessStartInfo` argument lists, never `cmd /c` string building → [[02 Secure Coding Practices]]
- [ ] **Dependency scanning and SBOM for shipped binaries** (NuGet dependencies included) → [[02 Dependency & Supply Chain]]
- [ ] **Signed releases with published checksums** so users can verify downloads → [[security]]

---

## Quick Sanity Check Before Release

- [ ] `--help`, `--version`, and a bad flag all behave from the *published* binary (bad flag: non-zero exit + usage hint on stderr)
- [ ] The trimmed/self-contained build (if shipped) executes every command without trimming-time surprises
- [ ] In-process tests and subprocess integration tests both pass on every OS you claim
- [ ] Piped output is ANSI-free; Spectre progress/status go quiet when redirected
- [ ] `dotnet tool install` works on a clean machine (or the runtime prerequisite is documented)
- [ ] Prompt paths are covered with queued test input — CI never hangs on a prompt
- [ ] Exit codes match the documented contract on success, usage-error, and failure paths

---

## Sources

- System.CommandLine docs: https://learn.microsoft.com/en-us/dotnet/standard/commandline/
- Spectre.Console / Spectre.Console.Cli docs: https://spectreconsole.net/
- Master checklist: [[cli]].
