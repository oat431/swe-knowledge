# Node.js CLI Checklist

> Framework-specific checklist for building and shipping CLIs in Node.js/TypeScript (commander / yargs / oclif).
> Companion to the general [[cli]] checklist — that one is the contract, this one is the plumbing.
> Last updated: 2026-11-14 (created: CLI framework layer)

---

## 1. Project Setup & Layout

- [ ] **`bin` entry declared in package.json:** `"bin": { "tool": "dist/cli.js" }` maps command name to the entry file; the file starts with a `#!/usr/bin/env node` shebang (npm's shims key off it).

```json
"bin": { "tool": "dist/cli.js" },
"files": ["dist", "completions"]
```
```js
#!/usr/bin/env node   // line 1 of dist/cli.js — shims depend on it
```
- [ ] **TypeScript compiled or bundled before publish:** `prepublishOnly`/`prepare` runs the build; `files` whitelist ships `dist/` (and completions/man) only — never `src/` and `node_modules/` bloat → [[cli]] §8.
- [ ] **ESM/CJS decision for the bin entry is deliberate:** `type: "module"` (ESM) or CJS compiled output — pick one and make `import`/`require` consistent; dual-mode packages are where bin resolution breaks.
- [ ] **`tsconfig` strict for CLI code too:** `strict: true`, `moduleResolution` matching the ESM/CJS choice; `outDir` clean on every build (stale compiled files ship silently otherwise).
- [ ] **One-command vs multi-command layout chosen up front:** a single action script for one job; a `commands/` directory (one module per subcommand) when the tree will grow → [[cli]] §1.
- [ ] **Version sourced from one place:** read `package.json` at runtime (or inject at build) for `--version` — never a hard-coded string that drifts → [[cli]] §1.

## 2. Command Parsing Framework

- [ ] **commander (default):** `program.command('deploy <env>')`, `.option()`, `.argument()`, `.action()` handlers, `.parse()`. Ubiquitous, tiny surface, one maintainer — the right default for most CLIs.
- [ ] **yargs (alternative):** when you want parsing horsepower — `.middleware()` for shared setup, `.coerce()` for type conversion, `.strict()`, `.env()` for env-var config. Great for flag-heavy single commands.
- [ ] **oclif (when it earns its weight):** plugin architecture, lifecycle hooks (`init`, `command_not_found`), generator-scaffolded multi-command trees, enterprise update/install story. Heavier runtime — pick it for extensible platforms, not one-shot tools.
- [ ] **One framework only:** no mixing parsers; the command tree is the single source of truth for parsing, help, and completions → [[cli]] §1.
- [ ] **Help wired per command:** `.showHelpAfterError()`-style behavior so a parse failure prints usage on stderr, not just a complaint → [[cli]] §1, §3.
- [ ] **Completions generated from the same tree** (commander/yargs/oclif each ship a completion mechanism) so flags and subcommands can't drift from the parser → [[cli]] §1.
- [ ] **Required args validated at parse time** with actionable messages (`error: missing required argument 'file'`), not deep in the action handler as a runtime crash → [[cli]] §3.

## 3. Output & UX Libraries

- [ ] **Color via a TTY-aware library:** `picocolors` (tiny) or `chalk` (feature-rich) — both honor `NO_COLOR`/`FORCE_COLOR` and auto-disable when piped; never hand-roll ANSI codes → [[cli]] §2.
- [ ] **Spinners on stderr:** `ora`-class spinner, started only when `process.stderr.isTTY`; a piped CI run gets plain text, not escape garbage → [[cli]] §2.
- [ ] **Tables via `cli-table3`-class library** (column alignment and wide-char handling is solved already — don't pad strings by hand) → [[cli]] §2.
- [ ] **Ink for rich interactive UIs:** React-style terminal rendering (forms, multi-pane, live dashboards) — justified when plain prompts/tables aren't enough; it's a dependency and startup cost, not a default.
- [ ] **Prompts via one maintained library:** `@inquirer/prompts` (inquirer family), `prompts`, or `enquirer` — pick one; every prompt has a flag escape hatch (`--yes`, `--force`) so CI never hangs → [[cli]] §5.
- [ ] **Output helpers funnel through one module** (`printJson`, `printTable`, `log`) so the stdout/stderr split and quiet/verbose spectrum hold across every command → [[cli]] §2.

## 4. Errors & Exit Codes

- [ ] **`process.exitCode = 1`, not `process.exit()` mid-flow:** `exit()` truncates pending stdout/stderr writes; set `exitCode` and return, letting streams flush → [[cli]] §3.
- [ ] **One error boundary at the bin entry:** wrap the async `main()` in try/catch — unexpected errors print a short message to stderr (full stack only with `--debug`) and set the exit code; no crash produces a raw stack dump → [[cli]] §3.
- [ ] **Usage errors vs runtime errors exit differently:** parse/validation failures use the usage code with a hint on stderr; execution failures use their own documented code → [[cli]] §3.
- [ ] **Rejections are caught:** `main().catch(...)` at top level (or `process.on('unhandledRejection')` guard) — an unhandled promise rejection is a crash with a useless message and wrong exit code.
- [ ] **Ctrl-C defined:** `SIGINT` handler flushes output, cleans temp files, exits 130; on Windows handle the console-control reality (no SIGTERM delivery to children) → [[cli]] §3, §6.

## 5. Configuration & State

- [ ] **Config discovery via `cosmiconfig`-class loader:** searches rc files and `package.json` keys up the tree with sane `searchPlaces`; don't hand-roll file hunting → [[cli]] §4.
- [ ] **Precedence implemented as documented:** flags > env vars > config file > defaults — one resolution function, tested → [[cli]] §4.
- [ ] **Platform dirs respected:** `$XDG_CONFIG_HOME`/`$XDG_STATE_HOME`/`$XDG_CACHE_HOME` on Linux, `~/Library/Application Support` on macOS, `%APPDATA%`/`%LOCALAPPDATA%` on Windows (small helpers like the `env-paths`-style packages exist — or the XDG spec) → [[cli]] §4.
- [ ] **State and cache under their own dirs, never cwd:** cache purgeable (`tool cache clear`), state (auth, machine id) per-user and separate from config → [[cli]] §4.
- [ ] **Config location discoverable:** `--config` flag and/or `tool config path` prints the resolved files → [[cli]] §4.
- [ ] **Env vars use a unique tool prefix** (`TOOL_API_KEY`, `TOOL_CONFIG`) parsed in one place (yargs `.env()`-style or manual) — unprefixed names collide with other tools in the user's shell → [[cli]] §4.
- [ ] **Sensitive values out of plaintext config:** OS credential store or 600-permission files at minimum → [[02 Secrets Management]] → [[cli]] §4.

## 6. Testing

- [ ] **Black-box tests spawn the built binary** (`execa`-class child process over `node dist/cli.js` or the `bin` path): assert stdout, stderr, and exit code together — the shipped contract, not the source → [[cli]] §11.
- [ ] **Exit codes tested on all three paths:** success (0), usage error, runtime error — each asserts its documented code → [[cli]] §3, §11.
- [ ] **Pipe mode tested:** run with stdout piped (not a TTY) and assert zero ANSI bytes and no spinner frames → [[cli]] §2, §11.
- [ ] **Unit-test command modules:** export the action handler as a function taking parsed args + injected deps; parser wiring tested separately from logic.
- [ ] **Golden help snapshots:** `--help` output for the root and each subcommand snapshotted (vitest/jest snapshot or committed golden files) — help text is user-facing UI → [[cli]] §11.
- [ ] **Config/state tests are hermetic:** point `HOME`/`XDG_*`/`%APPDATA%` (and the tool's env prefix) at a per-test temp dir — config discovery walking the real home directory makes tests order-dependent and leak state.
- [ ] **Windows in the CI matrix:** the same spawn-based tests on Windows runners; path, exit-code, and shim bugs only surface there → [[cli]] §6, §11 → [[qa]].

## 7. Packaging & Distribution

- [ ] **npm (or pnpm/bun) publish is the primary channel:** `npx tool` gives a zero-install first run — document it as the quickstart path → [[cli]] §8.
- [ ] **Global install behavior verified:** `npm i -g` shim creation, `tool` on PATH after install, and a clean documented uninstall (`npm rm -g`) → [[cli]] §8.
- [ ] **Bundle with esbuild to cut install size and startup:** one compiled JS file with dependencies inlined (external: native/optional ones) — fewer files, faster cold start, fewer transitive-dependency surprises.
- [ ] **Single binary when a Node runtime isn't acceptable:** `bun build --compile` or a `pkg`-class compiler produces a standalone executable — for air-gapped machines and non-Node users; test the artifact on every target OS → [[cli]] §8.
- [ ] **`engines` declares the supported Node range** and CI runs the oldest supported runtime — install-time engine warnings are the only signal users get.
- [ ] **No surprise `postinstall` scripts** (or audit/justify them): install-time code execution in a dependency is the classic supply-chain vector for npm-distributed bins → [[02 Dependency & Supply Chain]] → [[cli]] §7.
- [ ] **Update story is npm's — keep it that way:** document `npm update -g tool`; if you add a self-update command, it must detect npm installs and defer to the manager (self-updater + npm fight over the same directory) → [[cli]] §9.
- [ ] **Breaking changes gated:** changelog published, deprecated flags warn before removal, major bumps carry a migration note → [[cli]] §9.
- [ ] **Published checksums/provenance for any non-npm artifact** → [[02 Dependency & Supply Chain]] → [[cli]] §7.

## 8. Performance & Startup

- [ ] **Cold start budget measured and held** (sub-100ms perceived for `--help` and top commands): every dep loaded at startup is on the clock → [[cli]] §10.
- [ ] **Lazy-import heavy dependencies:** `await import()` inside the action handler (or `require` on demand in CJS) so `tool --help` doesn't load the world.
- [ ] **Bundle before measuring** (esbuild-class bundling removes module-resolution overhead; compare unbundled vs bundled before blaming the framework).
- [ ] **Profile with `node --cpu-prof`** (or `--prof`) when startup or a command is slow — measure, don't guess which import hurts → [[cli]] §10.

## 9. Windows Behavior

- [ ] **npm's shim reality understood:** global installs create `.cmd`/`.ps1` shims next to the entry; the `#!/usr/bin/env node` shebang is ignored by Windows but read by the shims — ship it anyway.
- [ ] **PowerShell execution policy anticipated:** locked-down machines can block npm's `.ps1` shims; the `.cmd` path must work, and troubleshooting goes in the docs → [[cli]] §6.
- [ ] **Spawn with argument arrays, never command strings:** Windows command-line parsing (the `CommandLineToArgvW` rules) re-quotes differently than sh — `execa`/`child_process.spawn` with an args array sidesteps the whole class → [[cli]] §6, §7.
- [ ] **Paths via `node:path` (`path.join`/`path.resolve`),** case-insensitivity respected; no `/`-hardcoded paths or string concatenation → [[cli]] §6.
- [ ] **CRLF awareness:** scripts you generate are LF-stable or explicitly CRLF where the consumer needs it; watch for CRLF breaking shell completions and shebang-adjacent files.
- [ ] **Terminal glyph sanity:** emoji and box-drawing width differs in legacy consoles — table borders and spinners must not shred on the default Windows terminal → [[cli]] §2.

## 10. Security

- [ ] **No `exec(string)`, ever:** `child_process.exec`/`execSync` run through a shell — use `execa`/`spawn` with argument arrays so user input can never become shell syntax → [[02 Secure Coding Practices]] → [[cli]] §7.
- [ ] **Dependency hygiene for a supply-chain-sensitive bin:** lockfile committed, minimal deps (each one runs on the user's machine with the user's rights), audit in CI, pinned transitive versions for published bundles → [[02 Dependency & Supply Chain]] → [[cli]] §7.
- [ ] **Threat model the tool as code execution on user machines:** an npm-distributed CLI runs with the user's full rights at install and every run — review new deps like new setuid binaries → [[security]].
- [ ] **Secrets never in argv, logs, or help output:** argv shows up in `ps`/Task Manager; read tokens from stdin, prompt, or env → [[02 Secrets Management]] → [[cli]] §5, §7.
- [ ] **Subprocess env sanitized:** spawn children with an explicit env object, not the full inherited `process.env` (which may carry credentials) → [[cli]] §6.
- [ ] **File writes safe:** `0o600` on files holding tokens/state, no blind symlink-following on config writes → [[cli]] §7.

---

## Quick Sanity Check Before Release

- [ ] `npm pack` (or the bundle output) contains `dist/`, `bin` resolves, and `npx`/global install runs the tool on a clean machine
- [ ] `node dist/cli.js --help` and `--version` are fast (no heavy imports at startup) and match the documented grammar
- [ ] Piped run (`tool ... | cat`) shows zero ANSI codes and no spinner frames; `NO_COLOR=1` respected
- [ ] Failed parse exits with the usage code and a hint on stderr; a thrown error prints one clean message — never a raw stack
- [ ] Black-box tests spawn the *built* binary and pass on the Windows CI runner
- [ ] No self-update path conflicts with npm ownership of the install
- [ ] Secrets never appear in `ps`, logs, or error output → [[02 Secrets Management]]

---

## Sources

- commander docs: https://commander.js.org/
- yargs docs: https://yargs.js.org/
- oclif docs: https://oclif.io/
- Node.js docs (`child_process`, `process.exitCode`, `--cpu-prof`): https://nodejs.org/api/
- Master checklist: [[cli]].
- Cross-checklist: [[security]] (threat model, supply chain), [[qa]] (test strategy).
