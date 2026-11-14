# Rust CLI Checklist

> Framework-specific checklist for building and shipping CLIs in Rust (clap).
> Companion to the general [[cli]] checklist — that one is the contract, this one is the plumbing.
> Last updated: 2026-11-14 (created: CLI framework layer)

---

## 1. Project Setup

- [ ] **Binary crate layout:** `main.rs` stays thin (parse → dispatch → map error to exit code); CLI types live in a `cli` module, or `src/bin/<cmd>.rs` per command once commands diverge → [[cli]] §1
- [ ] **clap derive is the default:** `#[derive(Parser)]` on the top struct, `#[derive(Subcommand)]` on the command enum, `#[derive(Args)]` for shared flag groups (merged with `#[command(flatten)]`)
- [ ] **Builder API when derive is too rigid:** dynamic command trees, runtime-generated subcommands, or conditional args (`Command::new(...).arg(...)`) — same parsing, help, and completion machinery underneath
- [ ] **Parse structs stay dumb:** `Args` structs are data only; logic lives in plain functions taking those structs, so behavior is unit-testable without argv

```
src/
├── main.rs        ← thin: parse → dispatch → map error to exit code
├── cli.rs         ← clap derive types (Parser / Subcommand / Args)
└── commands/
    ├── mod.rs
    ├── add.rs     ← pure logic behind the parse struct
    └── list.rs
```
- [ ] **argh (minimal derive):** choose when the tool is tiny, subcommand-free, and clap's surface is more than you need (smaller dep tree, faster compiles)
- [ ] **`std::env::args`:** acceptable for throwaway one-file tools where no dependency is justified; graduate to clap the moment flags or `--help` matter

## 2. Commands & Flags

- [ ] **Derive attributes are the documented contract:** `long`, `short`, `default_value`, `value_name`, `help`/`long_help` set deliberately, not left to defaults → [[cli]] §1
- [ ] **`value_parser` validates at parse time:** typed parsing (`PathBuf`, `NonZeroUsize`), ranges (`value_parser!(u16).range(1..)`), custom parsers returning `Result` — bad input fails before your code runs
- [ ] **`ArgAction` chosen deliberately:** `Set` (single value), `Append` (repeatable), `Count` (`-v -v`), `SetTrue` (boolean); never leave the action ambiguous
- [ ] **Required vs optional is visible in types:** `required = true` or `Option<T>`; a missing required arg yields clap's usage error, not a runtime failure → [[cli]] §3
- [ ] **Relationships declared in clap:** `conflicts_with`, `requires`, `requires_if`, `ArgGroup` — invalid combinations fail at parse time with a real message
- [ ] **Help text written for humans:** `about`/`long_about`, `after_help` with real examples, `verbatim_doc_comment` where formatting matters → [[cli]] §13

```rust
#[derive(Parser)]
struct Cli {
    #[arg(short, long, global = true, action = ArgAction::Count)]
    verbose: u8,
    #[command(subcommand)]
    command: Command,
}
```

## 3. Errors & Exit Codes

- [ ] **Typed errors in library code (thiserror):** one enum per failure class with `#[from]` sources; the binary layer wraps with context
- [ ] **anyhow for application glue:** `.context("what you were doing")` on fallible calls; the top level prints the full error chain, not just the top message
- [ ] **Rich diagnostics (miette-class):** labeled spans, stable error codes, `help:` suggestions for user-facing failures; graphical rendering only when stderr is a terminal
- [ ] **One `main` maps error classes to exit codes:** `fn main() -> ExitCode` (or `Result` + custom `Termination`); usage error, runtime failure, and partial failure each get their own code → [[cli]] §3
- [ ] **Never `std::process::exit` mid-flow:** it skips destructors and flushes (temp files, locks); return errors upward and exit at the boundary → [[cli]] §3

```rust
fn main() -> ExitCode {
    match run() {
        Ok(()) => ExitCode::SUCCESS,
        Err(e) => { eprintln!("error: {e:#}"); exit_code_for(&e) }
    }
}
```
- [ ] **Ctrl-C behavior defined:** termination handling leaves output flushed, temp files cleaned, locks released; documented exit code per platform convention → [[cli]] §3

## 4. Configuration & State

- [ ] **serde + a config crate (toml/figment-class):** layered sources with documented precedence — flags > env > config file > defaults → [[cli]] §4
- [ ] **Env binding is explicit per key:** opt in (env attribute or a manual merge step); never blanket-inherit the whole environment
- [ ] **Platform dirs via a dirs/directories-class crate:** XDG on Linux, Application Support on macOS, `%APPDATA%`/`%LOCALAPPDATA%` on Windows — no hand-rolled dotfile paths → [[cli]] §4
- [ ] **Config location discoverable:** a `--config` flag and/or a `config path`/`doctor` readout; the path is output, not folklore → [[cli]] §4, §12
- [ ] **State and cache split:** cache dir is purgeable, state dir holds auth/machine identity; neither ever lands in the current working directory → [[cli]] §4
- [ ] **Sensitive values never plaintext in config:** prefer the OS credential store; if a file must hold them, restrictive permissions (600) and a warning → [[02 Secrets Management]]

## 5. Output & UX

- [ ] **stdout/stderr discipline held in code:** data via `println!`/`stdout()`, diagnostics via `eprintln!`; `--json` mode puts only the payload on stdout → [[cli]] §2
- [ ] **ANSI styling via anstyle-class crates:** color as a styling layer, not string literals; honor `NO_COLOR`/`FORCE_COLOR`, strip when not a TTY → [[cli]] §2
- [ ] **Terminal detection (console-class):** TTY checks gate color, progress bars, and spinners → [[cli]] §2
- [ ] **Progress via indicatif-class bars:** drawn on stderr, auto-disabled when not a TTY; steady ticks for indeterminate work → [[cli]] §2
- [ ] **Prompts via a prompt library (dialoguer/inquire-class):** every prompt has a flag equivalent (`--yes`, `--force`); never prompt when stdin is not a TTY → [[cli]] §5
- [ ] **Width-aware output:** wrap to terminal width and degrade to plain text when width is unknown; no hard-wrapped lines that shred narrow terminals → [[cli]] §2

## 6. Testing

- [ ] **assert_cmd + predicates for black-box tests:** spawn the real binary from `tests/`, assert stdout, stderr, and exit code → [[cli]] §11, → [[qa]]
- [ ] **trycmd/golden snapshots for help and output:** `--help`, error text, and formatted output captured as reviewable fixtures; regeneration is deliberate, never automatic in CI
- [ ] **Exit-code paths each asserted:** success, usage-error, and runtime-error paths assert their own code → [[cli]] §11
- [ ] **Unit tests on pure logic:** parsing structs and command functions tested without argv; I/O stays at the edges → [[qa]]
- [ ] **Pipe-mode test:** run with stdout captured (not a TTY) — no ANSI codes, no spinner noise → [[cli]] §11
- [ ] **CI matrix covers every claimed OS:** including a Windows leg before Windows is claimed → [[cli]] §6, §11

## 7. Distribution & Release

- [ ] **cargo-dist-class release automation:** tag → build matrix → checksummed artifacts → release page; one command produces every shipped target
- [ ] **Static musl builds where they fit:** fully static Linux binaries remove glibc coupling; cross-builds via cross-class tooling or an explicit target matrix
- [ ] **Package managers over raw binaries:** Homebrew tap, cargo-binstall, scoop/winget, distro packages — document every supported install path → [[cli]] §8
- [ ] **Completions generated from the command tree:** clap_complete for bash/zsh/fish/PowerShell/elvish, exposed as `tool completion <shell>`; generation smoke-tested in CI → [[cli]] §1
- [ ] **Man pages via clap_mangen-class generation:** same source of truth as `--help`; shipped with the package or generated at install → [[cli]] §13
- [ ] **Uninstall is clean and documented:** which files the tool creates (config/state/cache paths) and how to remove them → [[cli]] §8

## 8. Performance & Resources

- [ ] **Startup is a feature:** no eager statics or heavy imports on the parse path; `std::sync::LazyLock` (or once-cell-class) for anything expensive → [[cli]] §10
- [ ] **Release profile tuned:** `lto = true`, `codegen-units = 1` for shipped builds — trade CI compile time for runtime speed

```toml
[profile.release]
lto = true
codegen-units = 1
strip = true
```
- [ ] **Binary size measured, not guessed:** `opt-level = "z"`, `strip = true`, and `panic = "abort"` tradeoffs applied where size matters (static downloads)
- [ ] **Stream large I/O:** buffered readers/writers and iterator pipelines; flat memory on large inputs → [[cli]] §10
- [ ] **Bounded concurrency:** explicit limits on parallel work (rayon thread pools, bounded channels); no unbounded thread spawning → [[cli]] §10

## 9. Windows Behavior

- [ ] **MSVC vs GNU toolchain decided once:** MSVC is the mainstream default; GNU only for specific linking needs; CI builds exactly the target you ship
- [ ] **Paths via `PathBuf` end to end:** no string concatenation, no hardcoded separators; respect case-sensitivity differences across filesystems → [[cli]] §6
- [ ] **ANSI on modern consoles:** enable virtual terminal handling where required; still honor `NO_COLOR` and piped (non-TTY) output → [[cli]] §6
- [ ] **Windows in the release/test matrix:** quoting, path separators, and console behavior verified before claiming support → [[cli]] §6

## 10. Security & Supply Chain

- [ ] **No shell string exec:** `std::process::Command` with `.arg()`/`.args()` slices; never build `sh -c`/`cmd /C` strings from user input → [[cli]] §7, → [[security]]
- [ ] **Secrets out of argv and logs:** read via stdin or a prompt; zeroize-class wiping for secrets held in memory where practical → [[02 Secrets Management]]
- [ ] **Dependencies audited:** cargo-audit/audit-class advisory scanning in CI; `Cargo.lock` committed for the shipped binary → [[02 Dependency & Supply Chain]]
- [ ] **File writes are safe:** restrictive permissions on state/secret files, no symlink-following surprises on config writes → [[cli]] §7
- [ ] **Unsafe code justified or forbidden:** `#![forbid(unsafe_code)]` where possible; every remaining `unsafe` block documented with its invariant → [[security]]

---

## Quick Sanity Check Before Release

- [ ] `cargo clippy -- -D warnings` clean and `cargo fmt --check` passes
- [ ] `--help` renders correctly for every command and subcommand; completions generate for all target shells
- [ ] Golden/help snapshots match current output — regenerating produces no diff
- [ ] Release build (LTO on) starts fast on a warm machine; startup cost measured, not assumed
- [ ] Bad flag, runtime failure, and success each return their documented exit code
- [ ] Piped run (`tool ... | cat`) shows no ANSI codes and no progress noise
- [ ] Dependency advisory scan is clean; release artifacts ship with checksums
- [ ] Fresh install from the documented path works on a clean machine for each claimed OS

---

## Sources

- clap docs: https://clap.rs/
- Master checklist: [[cli]].
