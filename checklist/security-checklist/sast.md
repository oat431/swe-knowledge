# SAST Checklist

> **Static Application Security Testing (SAST)** analyzes source code *without running it* to find security defects: injection sinks, crypto misuse, hard-coded secrets, dangerous APIs.
> The master checklist ([[security]]) §9 says "SAST in CI." This file is *how* to choose, configure, tune, and triage a SAST tool for your stack.
> Deep references: → [[02 Secure Coding Practices]], [[04 Vulnerability Management]].
> Last updated: 2026-08-07

---

## 1. What & Why

- [ ] **Core concept** — SAST reads your source code and matches it against security rules. It doesn't run the code — so it's fast, runs on every commit, and catches issues before the app is deployed. Trade-off: it can't see runtime behavior, so it may flag code that's actually safe in context (false positives).
- [ ] **What SAST is good at** — Injection sinks (SQLi, command injection, path traversal), crypto misuse (weak algorithms, hardcoded keys), dangerous APIs (`eval`, `pickle.load`, `Runtime.exec`), hardcoded secrets, outdated dependencies (overlaps with SCA).
- [ ] **What SAST is NOT good at** — Business logic flaws, authZ logic (IDOR), race conditions, runtime configuration issues, anything that requires understanding the running system. These need DAST, pentest, or code review.
- [ ] **SAST vs SCA vs DAST** — SAST reads code (static). SCA checks dependency versions against CVE databases (static, library-level). DAST attacks a running app (dynamic). All three are needed; they don't replace each other.

---

## 2. Tool Selection by Stack

| Language | Recommended SAST | Notes |
|---|---|---|
| Multi-language | **Semgrep** | Rule-based, fast, custom rules, generous free tier |
| Multi-language | **GitHub CodeQL** | Deep data-flow analysis, free for OSS, rule packs per language |
| JavaScript/TypeScript | **ESLint + eslint-plugin-security** | Linter with security rules, already in most JS stacks |
| Python | **bandit** | Python-native, simple, CI-friendly |
| Go | **gosec** | Go-native, simple |
| Java | **SpotBugs + Find Security Bugs** | Bytecode analysis |
| .NET | **Security Code Scan** | .NET-native |
| Enterprise / multi-lang | **SonarQube / Snyk Code / Checkmarx** | Commercial, dashboarded, compliance reporting |

- [ ] **Tool matches the stack** — Don't force bandit on a Go project. Pick the native tool first, Semgrep/CodeQL as a cross-cutting layer.
- [ ] **Free tier meets needs** — Semgrep CE and CodeQL are free and strong. Don't pay for commercial SAST until you've exhausted the free options.
- [ ] **Community rules exist for your framework** — Check Semgrep Registry / CodeQL query packs for your framework (Spring, Django, Express, Fiber, etc.).

---

## 3. Semgrep Setup

### Install & Scan

- [ ] **Install** —
  ```bash
  pip install semgrep
  # or
  brew install semgrep
  ```
- [ ] **First scan** —
  ```bash
  # Scan with default rules (auto-rules)
  semgrep scan --config auto
  ```
- [ ] **Specific rulesets** —
  ```bash
  semgrep scan --config p/javascript      # JS security
  semgrep scan --config p/python          # Python security
  semgrep scan --config p/golang          # Go security
  semgrep scan --config p/owasp-top-ten   # OWASP Top 10
  semgrep scan --config p/secrets         # Hardcoded secrets
  ```
- [ ] **Custom rules file** — `.semgrep.yml` in repo root for project-specific rules (see §8).

### CI Integration (GitHub Actions)

- [ ] **Semgrep CI workflow** —
  ```yaml
  # .github/workflows/semgrep.yml
  name: Semgrep
  on:
    push:
      branches: [ main ]
    pull_request:
      branches: [ main ]
  jobs:
    semgrep:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v4
        - uses: returntocorp/semgrep-action@v1
          with:
            config: auto
            generateSarif: true
          env:
            SEMGREP_APP_TOKEN: ${{ secrets.SEMGREP_APP_TOKEN }}
  ```
- [ ] **SARIF uploaded to GitHub code scanning** — Findings appear inline in PRs. Only *new* findings (vs baseline) are highlighted.

### Suppressing False Positives

- [ ] **Suppress with justification** — `// nosemgrep` inline comment with a reason and ticket reference:
  ```javascript
  // nosemgrep: js.crypto.ssl-insecure-version — ticket SEC-42, dev-only fallback
  const conn = tls.connect({ secureProtocol: 'SSLv23_method' });
  ```

---

## 4. CodeQL Setup

- [ ] **Add CodeQL workflow** —
  ```yaml
  # .github/workflows/codeql.yml
  name: "CodeQL"
  on:
    push:
      branches: [ main ]
    pull_request:
      branches: [ main ]
  jobs:
    analyze:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v4
        - uses: github/codeql-action/init@v3
          with:
            languages: javascript-typescript, python
        - uses: github/codeql-action/analyze@v3
  ```
- [ ] **Query packs selected** — `security-extended` for broader coverage beyond the default `security` pack. Tune per project.

---

## 5. Python: bandit

- [ ] **Install** — `pip install bandit`
- [ ] **Scan** — `bandit -r src/ -f json -o bandit-report.json`
- [ ] **Config** — `.bandit` or `pyproject.toml`:
  ```toml
  [tool.bandit]
  targets = ["src"]
  skips = ["B101"]  # assert in test code is fine
  ```

---

## 6. Go: gosec

- [ ] **Install** — `go install github.com/securego/gosec/v2/cmd/gosec@latest`
- [ ] **Scan** — `gosec ./...`
- [ ] **Config** — `.gosec` or environment variables for rule exclusions.

---

## 7. Triage & Tuning (The Hard Part)

> A scanner out-of-the-box will flag dozens to hundreds of findings. Without triage, teams lose trust and disable the scanner. Triage is where SAST delivers value.

- [ ] **First pass: bulk-close false positives** — Many findings are rule-specific false positives. Walk them once, suppress with `nosemgrep`/`#nosec` + a reason.
- [ ] **Fix High/Critical immediately** — SQLi, command injection, hardcoded secrets, crypto misuse. Don't queue them.
- [ ] **Set quality gates** — Block the PR on **new High/Critical** findings only. Don't block on pre-existing baseline — that erodes trust.
- [ ] **Set a baseline** — First full scan uploaded to GitHub code scanning. Future PRs are compared against this; only *new* findings block the PR.
- [ ] **Fix or suppress — never ignore** — Every finding is either (a) fixed, (b) suppressed with a reason and ticket, or (c) accepted as risk with an owner. Findings that sit in "ignored" accumulate as silent debt.
- [ ] **Tune quarterly** — Review suppressed findings: are they still false positives? Are new rules catching what manual review used to? Re-tune as the codebase and rules evolve.

---

## 8. Custom Rules (When Stock Rules Don't Catch Your Bugs)

- [ ] **Write a Semgrep custom rule for your dangerous pattern** —
  ```yaml
  # semgrep-rules/custom-no-eval.yaml
  rules:
    - id: mycompany-no-eval
      patterns:
        - pattern: eval(...)
        - pattern-not: eval("safe_expression")
      message: >-
        Do not use eval() with user input — command injection risk.
      languages: [javascript, typescript]
      severity: ERROR
  ```
- [ ] **Pattern after a real incident** — If your team keeps making the same mistake (e.g., string-built SQL, missing authZ on admin endpoints), write a Semgrep rule that catches it in PRs before it merges.

---

## 9. Anti-Patterns to Avoid

- [ ] **SAST as a one-time activity** — A scan done once during onboarding and never re-run. SAST runs on *every PR*, not on a schedule.
- [ ] **No triage** — Scanner flags 200 findings; team sees noise; team disables scanner. SAST without triage is worse than no SAST (it erodes trust).
- [ ] **No baseline** — Every scan flags the same pre-existing issues. The team can't see new findings through the noise. Baseline isolates *new* risk.
- [ ] **Blocking on all findings** — Low-severity findings block the pipeline; developers work around by suppressing everything. Block only on new High/Critical.
- [ ] **No custom rules** — The scanner catches generic issues but misses your team's recurring mistakes. Custom rules encode your coding standard.
- [ ] **Trusting SAST as "the security tool"** — SAST catches code patterns, not business logic. A 100% clean SAST report does not mean secure code. Pair with DAST and code review.

---

## Quick Sanity Check

- [ ] SAST tool selected per stack (Semgrep/CodeQL + native linter)
- [ ] SAST runs on every PR in CI, SARIF uploaded to GitHub code scanning
- [ ] First pass: baseline established, false positives suppressed with reasons
- [ ] Quality gate: PR blocked on new High/Critical findings only
- [ ] Custom rules written for your team's recurring mistakes
- [ ] Findings triaged quarterly; suppressed findings re-reviewed
- [ ] SAST paired with SCA (dependencies) and DAST (runtime) for full coverage

---

## Sources

- Master checklist: [[security]] §9.
- Deep references: [[02 Secure Coding Practices]], [[04 Vulnerability Management]].
- Tools: Semgrep, CodeQL, bandit, gosec, eslint-plugin-security, SonarQube.
- Standards: OWASP ASVS, OWASP Top 10.
