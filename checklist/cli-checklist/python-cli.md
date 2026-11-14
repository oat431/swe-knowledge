# Python CLI Checklist

> Framework-specific checklist for building and shipping CLIs in Python (Typer / Click).
> Companion to the general [[cli]] checklist — that one is the contract, this one is the plumbing.
> Last updated: 2026-11-14 (created: CLI framework layer)

---

## 1. Framework Choice

- [ ] **Typer for most new tools:** type hints become the parser (types, defaults, help, validation) with Rich-powered help out of the box; pick it when ergonomics and development speed win → [[cli]] §1
- [ ] **Click when you need lower-level control:** custom parameter types, exotic parsing, or an existing Click codebase — Typer runs on Click, so you can drop down mid-project without a rewrite.
- [ ] **argparse for zero-dependency tools:** stdlib only; right when installing your tool must not pull a dependency tree (bootstrap scripts, distro packages, locked-down environments).
- [ ] **Fire for throwaway scripts only:** it derives the whole CLI from function signatures — fine for a 20-line script, wrong for a stable public interface (help and ergonomics are not your contract).
- [ ] **One command grammar end-to-end:** the parser tree is generated from the same structure you document; never hand-roll a parallel dispatch layer → [[cli]] §1

## 2. App Structure & Commands

- [ ] **Typer app object:** `app = typer.Typer()`; commands via `@app.command()`; invoke `app()` from the entry-point function.
  ```python
  app = typer.Typer()

  @app.command()
  def greet(name: str) -> None:
      typer.echo(f"Hello {name}")

  def main() -> None:
      app()
  ```
- [ ] **`@app.callback()` for group-level concerns:** global flags (`--verbose`, `--config`) and shared setup live on the group callback, not copy-pasted into every command → [[cli]] §1
- [ ] **Subcommands via sub-Typer apps:** `sub = typer.Typer()` then `app.add_typer(sub, name="domain")` — one module per verb domain keeps the tree readable.
- [ ] **Entry function stays thin:** the console-script target just calls the app; logic lives in plain functions tests can call directly.
- [ ] **`--version` is eager:** an `is_eager=True` callback that prints and exits before any other parsing — never requires valid arguments → [[cli]] §1
- [ ] **Shell completions come from the same tree:** Typer's built-in `--install-completion`/`--show-completion`; Click's envvar-triggered completion mechanism; document per-shell install → [[cli]] §1

## 3. Arguments & Options

- [ ] **Annotated type hints drive parsing:** `def cmd(path: Annotated[Path, typer.Argument(...)], force: Annotated[bool, typer.Option("--force")] = False)` — type, help, and default in one declaration.
- [ ] **Enum classes become choices:** `class Format(str, Enum)` as the parameter type gives validated values and readable help instead of free-form strings.
- [ ] **Ranges/validators at the boundary:** numeric `min`/`max` or a custom parameter type/parse callback — reject bad input before any logic runs.
- [ ] **Required vs optional is explicit:** required positionals marked in the declaration; optional parameters carry a default in the signature, and that default *is* the documented behavior → [[cli]] §1
- [ ] **Prompt for missing required input only when interactive:** `prompt=True` with a flag equivalent for automation; never prompt when stdin is not a TTY → [[cli]] §5
- [ ] **Sensitive input via hidden-input prompts or stdin, never argv** → [[cli]] §5 → [[02 Secrets Management]]

## 4. Output & UX

- [ ] **Rich for human output:** tables, panels, progress bars (`rich.progress`) — or Typer's built-in Rich styling for help/errors. Rich rendering stays on the human path only → [[cli]] §2
- [ ] **`typer.echo`/`click.echo` over `print`:** handles Windows console encoding and strips ANSI when output is not a terminal — `print` does neither.
- [ ] **stdout is data, stderr is diagnostics:** logs, progress, and warnings go to stderr (`err=True` echoes) so piping works clean → [[cli]] §2
- [ ] **Quiet/verbose spectrum wired once:** `-q` errors-only, default summary, `-v/-vv` debug — configure logging from these in the group callback → [[cli]] §2
- [ ] **Honor `NO_COLOR` and non-TTY:** Rich and Click both respect the conventions; verify with `NO_COLOR=1 tool ... | cat` → [[cli]] §2
- [ ] **Terminal-width aware:** let Rich wrap to the detected width; never hard-code column widths or wrap by hand → [[cli]] §2

## 5. Errors & Exit Codes

- [ ] **`typer.Exit(code=...)` (or `click.exceptions.Exit`) for deliberate exits:** 0 success, documented non-zero classes per the contract → [[cli]] §3
- [ ] **Usage errors stay separate from runtime errors:** `UsageError`/`BadParameter` yield the usage message and the usage exit code; runtime failures raise domain exceptions handled centrally → [[cli]] §3
- [ ] **One top-level handler:** wrap `app()` so unexpected exceptions become a one-line actionable error on stderr with the right exit code — never a raw traceback as the default UX → [[cli]] §3
- [ ] **Tracebacks only under `--debug`:** re-raise with full stack when debug is set; the default path prints message-style errors → [[cli]] §3
- [ ] **Ctrl-C is clean:** Click's abort path (and your own handlers) flush output, clean temp files, release locks, exit non-zero → [[cli]] §3
- [ ] **Exit code table documented and tested:** help text or docs list every code; each is asserted in tests → [[cli]] §3

## 6. Configuration & State

- [ ] **Precedence: flags > env vars > config file > defaults:** one merge in a config loader under the CLI, not scattered `os.environ` reads → [[cli]] §4
- [ ] **Env binding is one documented mechanism:** Click's `envvar=` parameter support or reads in the config layer — pick one, don't mix → [[cli]] §4
- [ ] **Platform config dirs via a platformdirs-class package:** XDG on Linux, Application Support on macOS, `%APPDATA%` on Windows — no dotfile scatter → [[cli]] §4
- [ ] **Config location discoverable:** `--config` flag and/or a `config path` subcommand printing resolved locations → [[cli]] §4
- [ ] **dotenv-style `.env` loading is dev-only:** convenient locally; production reads real environment → [[cli]] §4
- [ ] **State and cache separated from config; sensitive values never plaintext** (OS credential store or 600-permission file) → [[cli]] §4 → [[02 Secrets Management]]

## 7. Testing

- [ ] **`typer.testing.CliRunner` (or `click.testing.CliRunner`) with pytest:** assert `result.exit_code` and `result.output` → [[cli]] §11 → [[qa]]
  ```python
  from typer.testing import CliRunner

  def test_greet():
      result = CliRunner().invoke(app, ["greet", "Ada"])
      assert result.exit_code == 0
      assert "Hello Ada" in result.output
  ```
- [ ] **Assert stderr separately when checking diagnostics:** usage hints and warnings must not hide behind data assertions → [[cli]] §11
- [ ] **Prompt paths tested:** feed `input=` to the runner for confirmations, and test the `--yes`/`--force` non-interactive equivalents → [[cli]] §5
- [ ] **Golden tests on help text:** snapshot `--help` per command; the help output is a documented contract → [[cli]] §1
- [ ] **Test the installed console script too:** invoke the real entry point in an integration test — catches packaging mistakes the in-process runner hides → [[cli]] §8
- [ ] **Exit codes asserted per path:** success, usage-error, and runtime-error each assert their code → [[cli]] §11

## 8. Packaging & Distribution

- [ ] **`pyproject.toml` `[project.scripts]` entry points:** console scripts generated at install (`mytool = "mytool.cli:app"`-style) → [[cli]] §8
- [ ] **Modern build backend:** a Hatchling- or uv-class backend in `[build-system]`; produce sdist + wheel with build-tooling, never ad-hoc uploads.
- [ ] **Publish from CI:** tag → build → test the built artifacts → upload; no laptop releases.
- [ ] **Users install via `pipx` or `uv tool`:** isolated environments, no interpreter pollution — these are the documented primary install paths → [[cli]] §8
- [ ] **Standalone binaries via a bundler when the runtime isn't acceptable:** PyInstaller-class freezing for users without Python; budget for size and antivirus false positives → [[cli]] §8
- [ ] **Completions install story shipped:** documented per shell alongside the install instructions → [[cli]] §8
- [ ] **Lockfile pins for reproducible installs of distributable tools** → [[02 Dependency & Supply Chain]]

## 9. Performance

- [ ] **Startup time is a feature:** interpreter + imports dominate; measure with `python -X importtime -c "import mytool"` → [[cli]] §10
- [ ] **Lazy imports inside command bodies,** not at module top: heavy deps (HTTP clients, dataframes, Rich extras) load only when their command runs → [[cli]] §10
- [ ] **Cheap commands stay cheap:** `--help` and `--version` must not import the world before answering → [[cli]] §10
- [ ] **Stream large output:** echo line-by-line; never build giant strings or tables for machine-readable output → [[cli]] §10

## 10. Windows & Cross-Platform

- [ ] **Console-script `.exe` shims:** installers generate wrapper executables in the scripts dir — test the installed command on Windows, not just `python -m` → [[cli]] §6
- [ ] **ANSI reality check:** modern terminals handle color, legacy consoles don't; rely on Rich/Click terminal detection, never force escape codes → [[cli]] §6
- [ ] **`pathlib` everywhere, no string concatenation:** respect case-insensitive filesystems when matching paths → [[cli]] §6
- [ ] **CRLF and encoding handled explicitly:** open text files with explicit `newline`/`encoding`; console output via `typer.echo` avoids codepage crashes → [[cli]] §6

## 11. Security

- [ ] **Subprocess with argument lists, never shell strings:** `subprocess.run([...])`, never `shell=True` with interpolated input → [[cli]] §7 → [[security]]
- [ ] **Secrets out of argv, logs, and help output:** hidden-input prompts or stdin; redact in debug/verbose output → [[02 Secrets Management]]
- [ ] **Pinned dependencies + lockfile for shipped tools; audit the tree; sign releases with published checksums** → [[02 Dependency & Supply Chain]]
- [ ] **Config/state file writes are safe:** restrictive permissions on sensitive files, no symlink-following surprises → [[cli]] §7

---

## Quick Sanity Check Before Release

- [ ] `python -m mytool --help` and the installed `mytool --help` produce identical help text
- [ ] `mytool ... | grep` sees clean data — no Rich styling, progress, or logs on stdout when piped
- [ ] `NO_COLOR=1` output contains no ANSI escapes; `--json` output parses as JSON
- [ ] `pytest` green through `CliRunner`, including prompt, usage-error, and runtime-error paths with their exit codes
- [ ] `python -X importtime` shows no heavy module imported before the first command runs
- [ ] Fresh `pipx install` (or `uv tool install`) from the built artifact works on a clean machine, Windows included
- [ ] A typo'd flag exits non-zero with a usage hint on stderr — never a traceback

---

## Sources

- Typer docs: https://typer.tiangolo.com/
- Click docs: https://click.palletsprojects.com/
- Master checklist: [[cli]].
