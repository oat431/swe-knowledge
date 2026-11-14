# CLI Tool Checklist

> Framework-agnostic checklist for command-line tools that ship to users.
> Go, Rust, Node, Python — the contract is the same. The plumbing changes.
> Last updated: 2026-11-14 (created: general CLI surface; framework layers deferred)

---

## 1. Command & Argument Design

- [ ] **One consistent grammar:** `tool <noun> <verb>` or `tool <verb> <noun>` — pick one and hold it across every subcommand.
- [ ] **Sensible defaults for the 90% path:** zero-flag invocation does the common thing or prints clear usage. Never silently does nothing.
- [ ] **Short + long flags:** `-v/--verbose`, `-o/--output`. Long flags are the documented contract; short flags are aliases.
- [ ] **`--help` is the manual:** every command and subcommand ships help with real examples. `--version` prints version instantly (build/commit info is a bonus).
- [ ] **Args vs flags is deliberate:** positional for the primary object (file, name), flags for options. Don't require both for the same thing.
- [ ] **Shell completions shipped:** bash/zsh/fish/PowerShell generated from the same command tree that parses input — single source of truth.
- [ ] **Names are stable:** renaming a flag is a breaking change. Deprecate with a warning before removing.

## 2. Output Discipline

- [ ] **stdout is data, stderr is diagnostics:** `tool list | jq` works with zero log noise on stdout.
- [ ] **Machine-readable mode (`--json`) where scripting is expected:** its schema is a contract; human output is free to evolve.
- [ ] **Quiet/verbose spectrum:** `-q` (errors only), default (summary + progress), `-v/-vv` (debug/trace).
- [ ] **No color or emoji when piped:** TTY detection; respect `NO_COLOR` and `FORCE_COLOR` conventions.
- [ ] **Progress for anything over ~2 seconds:** bar/spinner on stderr, auto-disabled when not a TTY.
- [ ] **Output adapts to terminal width:** no hard-wrapped text that shreds in narrow terminals.
- [ ] **Greppable key lines:** important output lines stay parseable as plain text.

## 3. Exit Codes & Errors

- [ ] **Exit code contract documented and stable:** 0 success; distinct non-zero classes (usage error vs runtime failure vs partial failure). Automation depends on this.
- [ ] **Usage errors ≠ runtime errors:** bad flags exit with the usage code and a hint on stderr; tool failures use their own code.
- [ ] **Partial failure reported honestly:** a batch operation that half-fails exits non-zero and says exactly what failed.
- [ ] **Errors are actionable:** what failed, why, and the next step. Never a bare stack trace — `--debug` shows the stack.
- [ ] **Ctrl-C behavior defined:** clean exit (130 on Unix convention), output flushed, temp files cleaned, locks released.

## 4. Configuration & State

- [ ] **Precedence defined and documented:** flags > env vars > config file > defaults.
- [ ] **Config paths follow platform conventions:** XDG on Linux (`$XDG_CONFIG_HOME`), `~/Library/Application Support` on macOS, `%APPDATA%` on Windows. Don't scatter dotfiles blindly.
- [ ] **Config location is discoverable:** `--config` flag and/or a `config path` subcommand.
- [ ] **State and cache separated from config:** cache is purgeable; state (auth, machine identity) is per-user — never in the current working directory.
- [ ] **Sensitive values never plaintext:** OS credential store, or at minimum file permissions 600 → [[02 Secrets Management]]

## 5. Interactivity & Safety

- [ ] **Non-interactive first:** every prompt has a flag equivalent (`--yes`, `--force`). CI must never hang on a question.
- [ ] **Destructive actions confirm or require `--force`:** print what will be affected before it happens.
- [ ] **Sensitive input via stdin or prompt, never argv:** argv is visible in `ps` and process listings → [[02 Secrets Management]]
- [ ] **`--dry-run` for destructive operations:** show planned actions without executing.
- [ ] **Idempotent where sensible:** re-running converges instead of duplicating.

## 6. Cross-Platform Behavior

- [ ] **Windows is first-class or explicitly unsupported:** cmd/PowerShell/Git-Bash quirks (quoting, `\` vs `/`, CRLF), executable naming and shims.
- [ ] **Paths handled by a path library:** no string concatenation; respect case-sensitivity differences across filesystems.
- [ ] **Signals handled everywhere:** SIGINT/SIGTERM on Unix, console control events on Windows; timeouts propagate to children.
- [ ] **Subprocess env sanitized:** spawned children don't inherit secrets they don't need.

## 7. Security

- [ ] **Secrets never in argv, logs, or help output** → [[02 Secrets Management]]
- [ ] **No shell interpolation of user input:** exec with argument arrays, never `sh -c` string building → [[02 Secure Coding Practices]]
- [ ] **File writes are safe:** no symlink-following surprises on config/state writes; restrictive permissions on sensitive files created.
- [ ] **TLS verification on by default:** an `--insecure` escape hatch must warn loudly, never be silent or default.
- [ ] **Signed releases with published checksums:** users can verify downloads → [[02 Dependency & Supply Chain]]
- [ ] **Dependency scanning + SBOM for distributed binaries** → [[02 Dependency & Supply Chain]]
- [ ] **Telemetry opt-in or clearly disclosed:** what is collected, how to disable. No silent phone-home.

## 8. Distribution & Installation

- [ ] **Install paths match the audience:** package managers (brew/scoop/winget/apt/cargo/npm/pipx) beat raw binaries for most users. Document every supported path.
- [ ] **Single-binary or minimal-dependency installs:** statically linked or runtime-free distribution removes a class of install failures.
- [ ] **Completions and man page ship with the tool:** `tool completion bash` style installers are the norm.
- [ ] **Uninstall is clean and documented.**

## 9. Update Strategy

- [ ] **One update owner:** self-updater OR package manager. If both exist they must cooperate (detect install source; defer to the manager).
- [ ] **Update checks never block the tool:** background or on-notice, skippable — never a startup stall.
- [ ] **Breaking changes gated:** major-version behavior changes warned on upgrade; changelog published.
- [ ] **State migrations are versioned:** backward-tolerant for at least one version so rollback doesn't corrupt state.

## 10. Performance & Resources

- [ ] **Startup time is a feature:** sub-100ms perceived for common commands; lazy-load heavy work.
- [ ] **Stream, don't buffer:** flat memory on large inputs; large outputs stream.
- [ ] **Temp files cleaned on every exit path,** including crashes.
- [ ] **Concurrency bounded:** limits and backpressure on parallel work — no FD/thread exhaustion.

## 11. Testing

- [ ] **Golden-output tests on the built binary:** stdout/stderr/exit codes asserted as a contract → [[02 Functional Testing]]
- [ ] **Exit codes tested:** success, usage-error, and runtime-error paths each assert their code.
- [ ] **Pipe mode tested:** run with stdout piped (not a TTY) — no ANSI codes, no spinner noise.
- [ ] **CI matrix covers every OS you claim** → [[03 CI-CD & Headless Testing]]
- [ ] **Completion scripts smoke-tested** (generation runs; target shells parse).

## 12. Observability & Support

- [ ] **Logs to stderr or file, never stdout:** `--log-level` or `-v` scale; timestamps in debug output.
- [ ] **A `doctor`/`info` diagnostic command:** version, config paths, effective settings, environment — the first thing support asks for.
- [ ] **Crash reports carry version + OS + what was attempted,** with user-controllable reporting.

## 13. Documentation & DX

- [ ] **README leads with a 30-second quickstart:** install → first success.
- [ ] **Examples over reference:** `--help` and docs show real invocations; recipes for the top 5 use cases.
- [ ] **Man page or equivalent** for offline help.
- [ ] **Feedback loop documented:** where to file bugs and what to include (the `doctor` output).

---

## Quick Sanity Check Before Release

- [ ] `tool --help`, `tool --version`, and `tool <bad-flag>` all behave sensibly (the last: non-zero exit + usage hint on stderr)
- [ ] `tool ... | jq` is clean; `tool ... 2>/dev/null` silences exactly the diagnostics
- [ ] `NO_COLOR=1` and piped output contain no ANSI codes
- [ ] A CI run without a TTY never hangs on a prompt
- [ ] Fresh install from the documented path works on a clean machine
- [ ] Secrets never appear in `ps`, logs, or crash reports

---

## Sources

- Principles distilled from clig.dev (Command Line Interface Guidelines), 12-factor CLI apps, GNU/POSIX conventions.
- Cross-checklist: [[security]] (threat model, supply chain), [[qa]] (test strategy), [[release]] (versioning & rollout).
