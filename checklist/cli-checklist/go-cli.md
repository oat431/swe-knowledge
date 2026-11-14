# Go CLI Checklist

> Framework-specific checklist for building and shipping CLIs in Go (Cobra).
> Companion to the general [[cli]] checklist — that one is the contract, this one is the plumbing.
> Last updated: 2026-11-14 (created: CLI framework layer)

---

## 1. Project Layout & Command Tree

- [ ] **One `cmd/<tool>/main.go` per binary:** thin main that wires dependencies, calls `Execute`, maps errors to exit codes — no business logic in `main` → [[cli]] §1
- [ ] **Root command owns the tree:** subcommands register via `rootCmd.AddCommand(...)` from explicit constructor functions (`newFooCmd()`); visible wiring beats `init()` magic.
- [ ] **`internal/` holds the logic:** command files only parse flags, call a service, format results; the tested logic lives in plain internal packages.
- [ ] **`cobra-cli init` / `cobra-cli add` for scaffolding:** generator keeps layout consistent across contributors (a single-command tool may not need the full tree).
- [ ] **`os.Exit` appears exactly once — in `main`:** every command returns errors up to `Execute`, and `main` owns the exit-code mapping → [[cli]] §3
- [ ] **Context comes from `ExecuteContext`:** pass `signal.NotifyContext(...)` down so commands read cancellation from `cmd.Context()` → [[cli]] §5

## 2. Commands & Flags

- [ ] **pflag POSIX parsing is the contract:** `--flag`, `--flag=value`, shorthand `-f`; document any interspersed-args behavior users might trip on → [[cli]] §1
- [ ] **`PersistentFlags` for inherited options:** `--verbose`, `--config`, `--output` live on root; command-specific flags use that command's local `Flags()` — never scatter duplicates.
- [ ] **Arg-count validators set explicitly:** `cobra.ExactArgs`, `MinimumNArgs`, `MaximumNArgs`, `RangeArgs`, `NoArgs`, `ArbitraryArgs`; `MatchAll` composes them — never leave arity implicit.
- [ ] **Required flags marked, not just documented:** `cmd.MarkFlagRequired("name")` fails at parse time; `MarkFlagsRequiredTogether`, `MarkFlagsOneRequired`, `MarkFlagsMutuallyExclusive` encode flag relationships.
- [ ] **Shorthand discipline:** one meaning per letter within a command; `-h` stays help, `--version` documented per → [[cli]] §1
- [ ] **Static completions via `ValidArgs`, dynamic via `ValidArgsFunction`:** remote lookups at completion time need a short timeout (tab must feel instant) → [[cli]] §1
- [ ] **Every command fills `Short`/`Long`/`Example`:** Cobra renders them into help; an empty-help command ships as an undocumented one → [[cli]] §13

## 3. Configuration

- [ ] **Precedence flags > env > config > defaults:** Viper encodes this when flags are bound and `AutomaticEnv` is on; publish the mapping table → [[cli]] §4
- [ ] **Env names derived deterministically:** `SetEnvPrefix` + `SetEnvKeyReplacer` so `--api-url` maps to `TOOL_API_URL` — no hand-typed drift between flag and env names.
- [ ] **Config discovery follows platform dirs:** `os.UserConfigDir()` (or XDG helpers) with `AddConfigPath`; never assume `~/.tool` is acceptable everywhere → [[cli]] §6
- [ ] **`--config` overrides discovery, and the winner is discoverable:** Viper's `ConfigFileUsed` feeds a `tool config path` or `tool doctor` output → [[cli]] §12
- [ ] **Missing config is fine, invalid config is fatal:** tolerate "file not found" for optional config; syntax/type errors abort naming the offending file and key.
- [ ] **Secrets come from env or the OS credential store, never the config file** → [[02 Secrets Management]]

## 4. Output & UX

- [ ] **`SilenceUsage: true` + `SilenceErrors: true` on root, then print errors yourself:** Cobra's defaults dump the full usage text on every runtime failure — great for arg mistakes, noise for `RunE` failures → [[cli]] §2
- [ ] **One error printer owns stderr:** consistent `error: ...` shape; usage text shown only for flag/args mistakes; `--debug` adds wrapped-chain detail → [[cli]] §3
- [ ] **Color via a styling library (lipgloss/termenv class):** they handle TTY detection, color depth, `NO_COLOR`; hand-rolled ANSI does not survive piping or Windows consoles → [[cli]] §2
- [ ] **Tables aligned with `text/tabwriter` or a table renderer:** header-keyed columns; plain mode stays greppable → [[cli]] §2
- [ ] **Rich TUI (Bubble Tea) only when interactive:** the same operation keeps a flags-only path (`--yes`, `--json`) so CI never enters a TUI → [[cli]] §5
- [ ] **Templates customized sparingly:** `SetUsageTemplate`/`SetHelpTemplate`/`SetVersionTemplate` for branding; examples belong in each command's `Example` field, not in templates → [[cli]] §13

## 5. Errors & Exit Codes

- [ ] **Commands are `RunE` and return errors:** no `log.Fatal`/`os.Exit` inside command code (skips defers, untestable, breaks the exit-code contract) → [[cli]] §3
- [ ] **One error-class → exit-code map, applied in `main`:** `Execute`'s return value is the single funnel; each error class gets its stable code → [[cli]] §3

```go
if err := rootCmd.ExecuteContext(ctx); err != nil {
    os.Exit(exitCodeFor(err)) // usage errors vs runtime vs partial failure
}
```

- [ ] **Wrap with `%w`, classify with `errors.Is`/`errors.As`:** sentinel errors (`ErrUsage`, `ErrNotFound`, `ErrPartial`) survive `fmt.Errorf("...: %w", err)` chains to the mapper.
- [ ] **Usage errors ≠ runtime errors:** `SetFlagErrorFunc` catches flag-parse failures for the usage-exit path; `Args` validators return usage-class errors → [[cli]] §3
- [ ] **Shared validation in `PersistentPreRunE`/`PreRunE`:** fail fast on bad config or missing credentials before the command body runs.

## 6. Testing

- [ ] **Execute commands in-process with injected IO:** `cmd.SetArgs([]string{...})`, `SetOut`/`SetErr` to buffers, assert the returned error — subprocesses only for the smoke test → [[cli]] §11

```go
buf := new(bytes.Buffer)
root := newRootCmd()          // fresh tree per test
root.SetOut(buf); root.SetErr(buf)
root.SetArgs([]string{"list", "--json"})
err := root.Execute()
```

- [ ] **Fresh command tree per test:** constructors, not package-level shared `*cobra.Command` — flag state accumulates and parallel tests collide.
- [ ] **Table-driven per command:** args → want stdout substring/JSON, want error class, want exit code.
- [ ] **Golden files for help and formatted output:** `--update` regenerates; help text is output like any other and breaks like any other → [[qa]]
- [ ] **`httptest.Server` for API clients:** inject base URL via flag/config; unit tests never touch the network.
- [ ] **One smoke test on the built binary:** CI builds the binary and runs `--help`, `--version`, and a bad flag — catches wire-up issues in-process tests miss → [[cli]] §11

## 7. Distribution & Releases

- [ ] **Static binary: `CGO_ENABLED=0 go build`:** single file, no libc/dynamic-linker surprises → [[cli]] §8
- [ ] **Cross-compile matrix in CI:** `GOOS`/`GOARCH` for linux/darwin/windows × amd64/arm64 at minimum; the Windows build is tested, not merely produced → [[cli]] §6
- [ ] **GoReleaser owns releases:** build matrix, archives, checksums, changelog from one config; `git tag` triggers the release.
- [ ] **Version injected via `-ldflags "-X ..."`:** set the root command's `Version` (rendered via `SetVersionTemplate`) from build metadata; same for commit and build date → [[cli]] §9
- [ ] **Completions generated from the command tree:** ship the built-in `completion` subcommand (bash/zsh/fish/PowerShell), or generate explicitly via `GenBashCompletionV2`/`GenZshCompletion`/`GenFishCompletion`/`GenPowerShellCompletion` writers → [[cli]] §1
- [ ] **Package-manager installs published (Homebrew tap, scoop/winget):** GoReleaser can feed the tap; managers beat curl-pipe scripts → [[cli]] §8
- [ ] **Checksums (and signatures) on every release artifact** → [[security]]

## 8. Windows & Cross-Platform

- [ ] **`filepath` for all path math:** `filepath.Join`/`Clean`/`Base`; the `path` package is for slash-strings and URLs only → [[cli]] §6
- [ ] **ANSI behavior verified on legacy consoles and pipes:** styling libraries degrade gracefully; assert no escape codes when output is redirected on Windows too → [[cli]] §6
- [ ] **`.exe` naming accounted for:** build config, install docs, and any shell shims reference `tool.exe` where relevant → [[cli]] §8
- [ ] **Signals portably handled:** `signal.NotifyContext(ctx, os.Interrupt)` plus documented per-platform kill semantics; children get the cancellation → [[cli]] §6
- [ ] **Line endings explicit:** written output uses `\n` unless CRLF is intended; round-trip tests catch the drift.

## 9. Performance & Startup

- [ ] **Startup stays in budget (sub-100ms perceived):** Go makes this easy — don't ruin it with `init()` chains, heavy package-level template parsing, or eagerly loading large config at import time → [[cli]] §10
- [ ] **Network clients built lazily in `RunE`:** global HTTP/SDK clients at import time slow every `--help` and `completion` call.
- [ ] **Binary stays small:** audit `go.mod` occasionally — one convenience dependency is not worth ten transitive ones → [[cli]] §10
- [ ] **Completion functions are latency-critical:** `ValidArgsFunction` runs on tab; bound remote lookups with timeouts and cache where sensible.

## 10. Security

- [ ] **Secrets never in argv, env dumps, or `--debug` output:** anything printing flags or `os.Environ()` redacts sensitive values → [[02 Secrets Management]]
- [ ] **`os/exec` with arg slices:** `exec.CommandContext(ctx, "tool", args...)` — never string-built `sh -c` for user input → [[security]]
- [ ] **Signed releases with published checksums** → [[security]]
- [ ] **TLS verification on by default:** stdlib `http.Client` gives this; any `--insecure` escape hatch warns loudly → [[cli]] §7
- [ ] **Restrictive permissions on files you create:** `0600` files / `0700` dirs for state and credentials → [[cli]] §4

## 11. Choosing the Framework

- [ ] **urfave/cli:** when you want a smaller action-func API and lighter flag surface than Cobra's command structs.
- [ ] **Kong:** when struct-tag, declarative command definitions fit the codebase style better than tree-wiring.
- [ ] **stdlib `flag`:** tiny single-command tools with a handful of flags — zero dependencies, no subcommand tree needed.
- [ ] **Stay on Cobra for the ecosystem:** completions, man pages, `cobra-cli` scaffolding, and conventions shared across teams.

---

## Quick Sanity Check Before Release

- [ ] `go test -race ./...` clean, and every command has an in-process `SetArgs` test covering success, usage-error, and runtime-error paths
- [ ] Built binary: `--help` and `--version` instant; `tool <bad-flag>` exits non-zero with usage on stderr; runtime failures print no usage dump
- [ ] Completion generation works for bash, zsh, fish, and PowerShell from the built binary
- [ ] `CGO_ENABLED=0` build and Windows cross-compile both green in CI, and the Windows binary is exercised somewhere
- [ ] Startup measured after the last dependency change (`time tool --help`) and still in budget
- [ ] Exit codes asserted per error class — automation sees stable, documented codes
- [ ] No secrets in `--debug` output, error messages, or help text

---

## Sources

- Cobra documentation: https://cobra.dev (commands, flags, completions, testing patterns).
- Master checklist: [[cli]].
