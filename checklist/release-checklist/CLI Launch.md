# CLI Launch Checklist

> Tick every box before a CLI tool ships to users. Framework-agnostic. Deep knowledge: follow the `→` links.

---

## Interface & Output

- [ ] `--help` works on every command and subcommand; `--version` prints instantly
- [ ] stdout is pure data (pipeable to `jq`/grep); all diagnostics on stderr
- [ ] Machine-readable mode (`--json`) has a stable schema where scripting is expected
- [ ] Color and progress disabled when not a TTY; `NO_COLOR` respected
- [ ] Quiet (`-q`) and verbose (`-v/-vv`) modes cover automation and debugging
- [ ] Output readable in narrow terminals

## Exit Codes & Errors

- [ ] Exit code contract documented: 0 success, distinct codes for usage vs runtime vs partial failure
- [ ] Usage errors exit non-zero with a hint on stderr — never a stack trace
- [ ] Partial failures say exactly what failed and what succeeded
- [ ] Ctrl-C exits cleanly: temp files cleaned, locks released, output flushed

## Config & State

- [ ] Precedence documented: flags > env > config > defaults
- [ ] Config/cache/state paths follow platform conventions (XDG / Application Support / %APPDATA%)
- [ ] Secrets never in argv (visible in `ps`) — prompt/stdin or credential store → [[02 Secrets Management]]
- [ ] Every prompt has a non-interactive flag (`--yes`/`--force`) → [[02 Secure Coding Practices]]
- [ ] `--dry-run` available for destructive operations

## Security

- [ ] User input never shell-interpolated (exec with arg arrays) → [[02 Secure Coding Practices]]
- [ ] TLS verification on by default; insecure modes warn loudly → [[03 Network & TLS]]
- [ ] Releases signed, checksums published → [[02 Dependency & Supply Chain]]
- [ ] Dependencies scanned; SBOM for distributed binaries → [[02 Dependency & Supply Chain]]
- [ ] Telemetry disclosed and disableable

## Testing

- [ ] Golden tests assert stdout/stderr/exit codes on the built binary → [[02 Functional Testing]]
- [ ] Piped (non-TTY) mode tested: no ANSI codes, no spinner noise
- [ ] CI runs on every OS you claim to support → [[03 CI-CD & Headless Testing]]
- [ ] Shell completions generate and load without errors

## Distribution & Updates

- [ ] Install documented for each supported path (package manager, binary, source)
- [ ] Exactly one update owner (self-updater or package manager) — they don't fight
- [ ] Uninstall is clean and documented
- [ ] Breaking changes gated behind major versions; changelog published

## Docs & Support

- [ ] README: install → first success in 30 seconds
- [ ] Real examples for the top use cases in `--help` and docs
- [ ] A `doctor`/`info` command prints version + config paths + effective settings
- [ ] Bug-report path documented (what to include)

---

## Sources

- Distilled from CLI design guidance (clig.dev, 12-factor CLI) and your vaults: Security, QA.
- Original deep reference: `../cli-checklist/cli.md`
