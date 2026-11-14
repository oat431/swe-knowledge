# JVM CLI Checklist

> Framework-specific checklist for building and shipping CLIs on the JVM (picocli / Spring Shell).
> Companion to the general [[cli]] checklist — that one is the contract, this one is the plumbing.
> Last updated: 2026-11-14 (created: CLI framework layer)

---

> **Naming note:** "Spring CLI" is ambiguous in conversation. **Spring Shell** is Spring's interactive-shell framework — `@ShellMethod` commands behind a REPL or a scripted one-shot runner. **Spring Boot CLI** is a different dev tool for running Groovy scripts. Everything below about "Spring Shell" means the shell framework; where a note says "spring cli", check which one the speaker meant.

## 1. Project Setup & Tool Choice

- [ ] **picocli is the JVM default:** annotation-driven (`@Command` / `@Option` / `@Parameters`), single dependency, no framework required — the fastest path from zero to a real CLI → [[cli]] §1.
- [ ] **Spring Shell when the product is an interactive admin shell for a Spring shop:** `@ShellComponent` / `@ShellMethod` beans share the Boot application context (config, security, data access) instead of standing alone.
- [ ] **Lightweight arg parsers (args4j-class) only for tiny internal tools:** less ceremony, but you hand-roll help, subcommands, and completions — the cost reappears at the second subcommand → [[cli]] §13.
- [ ] **One entrypoint owns the command tree:** the root `@Command` declares subcommands (or registers them on the parser object) — never two sources of truth for the same tree.
- [ ] **Annotation processor on the build path from day one:** it derives help metadata, man pages, and native-image reflection config from the same annotations; retrofitting it later misses commands.
- [ ] **A launchable artifact exists before UX polish:** one fat/uber jar (or native image) runnable end-to-end; wiring polish is cheap, packaging discovered late is not → [[cli]] §8.

## 2. Commands & Flags Ergonomics (per tool)

- [ ] **picocli: `@Command` per verb, `@Option` for flags, `@Parameters` for positionals** — this mirrors the grammar contract (short + long names on every user-facing flag) → [[cli]] §1.
- [ ] **Commands are `Callable<Integer>` or `Runnable`:** `Callable` returns the exit code; keep business logic in plain services so it is unit-testable without the parser in the way.

```java
@Command(name = "fetch", description = "Fetch one record")
class Fetch implements Callable<Integer> {
    @Option(names = {"-v", "--verbose"}) boolean verbose;
    @Parameters(index = "0") String id;
    public Integer call() { return service.fetch(id, verbose) ? 0 : 1; }
}
```

- [ ] **Subcommands: declarative or programmatic, pick one style:** declarative (`subcommands = ...` on `@Command`) for static trees, programmatic registration for plugin-shaped trees — mixed styles break help generation assumptions.
- [ ] **Shared root context via parent-command injection:** global flags (`--verbose`, `--config`) live on the root command and flow down; don't redeclare them per subcommand → [[cli]] §1.
- [ ] **Mutually exclusive / required groups declared, not hand-checked:** argument groups make "exactly one of `--file` / `--stdin`" a parse-time error with correct usage text.
- [ ] **Type conversion declared at the parser:** enums and common types convert automatically; custom types via a type converter; Bean Validation or explicit checks run at parse time (bad input dies before any work).
- [ ] **Spring Shell: `@ShellMethod` names the command, `@ShellOption` binds arguments and flags** (defaults, arity, optional vs required) — the method signature is the CLI contract.

```java
@ShellComponent
class AdminCommands {
    @ShellMethod("Show one user")
    String show(@ShellOption("--id") long id) { return service.render(id); }
}
```

- [ ] **Spring Shell: support both execution modes** — interactive REPL (editing, history, custom prompt) and scripted one-shot (command as launch args, then exit); CI and automation use the one-shot path → [[cli]] §5.
- [ ] **Help is generated from the model, never hand-written:** usage templates, width-aware layout, examples in the command description; deprecated flags warn before removal → [[cli]] §13.

## 3. Errors & Exit Codes

- [ ] **Return value is the exit code:** `Callable` returns it; for failures thrown below, use the parser's exit-code generator / exit-code exception mapper so exceptions become documented non-zero codes, not stack dumps → [[cli]] §3.
- [ ] **Usage errors ≠ runtime errors:** parse failures (bad flag, missing arg) exit with the usage code and a hint on stderr; command failures use their own codes; the code table is documented.
- [ ] **One central exception mapping:** `IOException`/domain exceptions map to stable codes plus a one-line actionable message (what failed, why, next step); stack traces only under `--debug` → [[cli]] §3.
- [ ] **Spring Shell: failed commands still exit non-zero in one-shot mode** — a friendly REPL exception message is not an exit code; propagate the failure to the runner's exit status.
- [ ] **Interrupt behavior defined:** Ctrl-C exits cleanly with output flushed, temp files removed, locks released (Unix convention: 130) → [[cli]] §3.
- [ ] **Partial failure reported honestly:** batch subcommands that half-fail print per-item status and exit non-zero, never a silent partial success.

## 4. Configuration

- [ ] **Precedence defined and documented: flags > env vars > config file > defaults** — whichever library you use; show effective values in `doctor`/`--help` → [[cli]] §4.
- [ ] **One config stack per tool:** MicroProfile Config or Typesafe Config (HOCON) for standalone picocli tools; Spring `Environment` / `@ConfigurationProperties` when the CLI runs on Boot (→ [[spring-boot-api]] §14).
  Mixing two stacks in one CLI produces two precedence rules — and users will guess the wrong one.
- [ ] **Config paths follow platform conventions** (XDG / Application Support / `%APPDATA%`) with a `--config` override; state and cache live apart from config → [[cli]] §4.
- [ ] **Env var names documented alongside flags** with one naming convention (`TOOL_VERBOSE`, `TOOL_CONFIG_HOME`); they are a public interface too.
- [ ] **Secrets never in config files, argv, or help text** — prompt, stdin, env, or the OS credential store → [[02 Secrets Management]].

## 5. Output & UX

- [ ] **stdout data, stderr diagnostics** — the JVM default fights you: `System.out` and `System.err` both land in the console, so wire the slf4j console appender to stderr explicitly → [[cli]] §2.
- [ ] **ANSI via a terminal library (Jansi / JLine-class), not raw escape codes:** auto-detect TTY, honor `NO_COLOR`/`FORCE_COLOR`, degrade to plain text when piped.
- [ ] **JLine for interactive commands:** line editing, history, completion inside the Spring Shell REPL; never hand-roll console reads once editing is needed.
- [ ] **`--json` for scripting output** where automation is expected: the human renderer and the JSON renderer consume the same result object, so the schema is the contract → [[cli]] §2.
- [ ] **Progress/spinner on stderr for >2s operations,** auto-disabled when not a TTY; terminal-width-aware help text → [[cli]] §2.
- [ ] **A `doctor`/`info` subcommand:** version, JVM, config paths, effective settings — the first thing support asks for → [[cli]] §12.

## 6. Testing (JUnit)

- [ ] **Test through the parser:** invoke the command model with arg arrays (`new CommandLine(app)` / its execute entrypoint) and assert stdout, stderr, and exit code — parse errors are behavior → [[cli]] §11.
- [ ] **Parser-level edge cases covered:** defaults, arity violations, type-conversion failures, and argument-group conflicts each produce the usage code and a usable message.
- [ ] **Exit-code assertions on all three paths** — success, usage error, runtime failure — with print streams captured (or injected) so tests assert text, not console side effects → [[qa]].
- [ ] **Spring Shell: `@SpringBootTest` integration tests** boot the shell context and drive `@ShellMethod` commands end-to-end, including one-shot mode and a non-interactive run that must not hang → [[cli]] §5.
- [ ] **Golden-output tests for `--help` and generated completions** on the built artifact; a piped run asserts no ANSI codes → [[cli]] §11.
- [ ] **CI runs the built artifact on every OS you claim,** including a headless no-TTY leg where nothing may prompt or colorize → [[cli]] §6.

## 7. Packaging & Distribution

- [ ] **Fat/uber jar as the baseline** (`java -jar tool ...`): fine where a JVM is guaranteed, but it drags a runtime into every install → [[cli]] §8.
- [ ] **jlink/jpackage trimmed runtime** when you own the install story: a minimal runtime plus a native launcher removes the "install a JDK first" failure class.
- [ ] **GraalVM native image is the fast-startup path:** the picocli annotation processor generates the reflection/resource metadata the native build needs — treat native as a build target exercised in CI, not an afterthought.
- [ ] **Native-image quirks covered by a smoke test:** reflection of user classes, bundled resources, and no silent fallback to a JVM-mode build when the native build fails.
- [ ] **jbang for zero-build single-file scripts** and internal tools; move to a real build the moment packaging or publishing matters.
- [ ] **Completions and man pages ship with the tool, generated from the same command tree:** bash/zsh generation is built in; fish/PowerShell need maintained equivalents — `tool completion <shell>` installers, each smoke-tested → [[cli]] §8.
  Generated, never hand-maintained: a hand-copied completion script rots on the first subcommand rename.
- [ ] **Distribution through package managers** (Homebrew / scoop / winget / sdkman-class) with checksummed artifacts and a clean documented uninstall → [[cli]] §8.
- [ ] **One update owner** — self-updater or the package manager, never two (→ [[cli]] §9); `--version` reports build/commit metadata baked in at build time.

## 8. Startup Performance

- [ ] **JVM cold start is the CLI's biggest liability — measure it:** `time tool --help` on every release against the general budget (sub-100ms perceived for common commands) → [[cli]] §10.
- [ ] **Mitigations in payoff order:** native image > jlink/jpackage trimmed runtime > lazy class loading and deferring framework startup to the first command that needs it.
- [ ] **Spring Shell's startup is the heaviest option** (full application context before the first prompt): acceptable for a long-lived REPL, wrong for `tool --version` latency — lazy-init and one-shot deferral where the mode allows.
  Measure the two modes separately: REPL startup and one-shot startup are effectively different products.
- [ ] **No heavy work before argument parsing:** no connections, classpath scans, or remote config reads until the command line has validated; every dependency on the classpath is startup cost.
- [ ] **The startup budget is enforced in CI as a regression check** (timed `--help`/`--version` runs), not a one-off measurement someone did once.

## 9. Windows

- [ ] **Ship an `.exe`/launcher (jpackage or a shim), not just a jar:** `tool.bat` + jar works until quoting breaks; test cmd, PowerShell, and Git-Bash → [[cli]] §6.
- [ ] **Console code page and encoding handled:** non-ASCII output survives code-page differences; test CJK/emoji output and CRLF in piped output.
- [ ] **Paths via `java.nio` (`Path.of`), never string concatenation:** accept both separators, respect case-insensitive filesystems → [[cli]] §6.
- [ ] **Ctrl-C and console control events handled:** shutdown hooks release locks and clean temp files on every exit path, including crashes → [[cli]] §3.
- [ ] **Windows is tested or explicitly unsupported:** a CI leg (or a documented "Unix-only") — Windows behavior diverges quietly, never by announcement.

## 10. Security

- [ ] **Secrets never in argv, logs, help output, or crash reports** — argv is visible in process listings → [[02 Secrets Management]].
- [ ] **No shell string building for subprocesses:** `ProcessBuilder` with an argument list, never a concatenated `Runtime.exec` string → [[02 Secure Coding Practices]].
- [ ] **Scan the fat jar's dependency tree** (shading hides transitive CVEs) and publish an SBOM with releases → [[02 Dependency & Supply Chain]].
- [ ] **File writes safe:** restrictive permissions on config/state files, no symlink-following surprises → [[cli]] §7.
- [ ] **TLS verification on by default;** any `--insecure` escape hatch warns loudly, never silently → [[security]].

---

## Quick Sanity Check Before Release

- [ ] `tool --version` and `tool --help` return near-instantly on a cold JVM (or you ship native/jlink and measured it)
- [ ] Bad flag → usage code + hint on stderr; thrown exception → distinct runtime code + one-line message, stack only under `--debug`
- [ ] `tool ... | jq` is clean — logging goes to stderr through the console appender; piped output contains no ANSI codes
- [ ] One-shot / scripted mode never hangs in CI: no prompt lacks a flag equivalent (`--yes`/`--force`)
- [ ] Completion scripts for every shipped shell are generated at build time and parse-tested
- [ ] The jar/exe runs on a clean Windows machine: code page, both path separators, Ctrl-C cleanup
- [ ] No secrets in `ps`, help output, or logs; fat-jar dependencies scanned → [[02 Secrets Management]]

---

## Sources

- picocli reference documentation — https://picocli.info/
- Spring Shell reference documentation — https://docs.spring.io/spring-shell/reference/
- Master checklist: [[cli]].
