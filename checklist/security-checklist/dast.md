# DAST Checklist

> **Dynamic Application Security Testing (DAST)** attacks a running application from the outside — like an attacker would — to find runtime vulnerabilities: injection, authZ bypasses, misconfiguration, exposed endpoints.
> The master checklist ([[security]]) §9 says "DAST on staging." This file is *how* to set up and run DAST scans.
> Deep references: → [[03 API Security]], [[04 Vulnerability Management]].
> Last updated: 2026-08-07

---

## 1. What & Why

- [ ] **Core concept** — DAST sends HTTP requests to a running instance of your app and analyzes responses for vulnerability signatures. It doesn't read your code. It sees what an attacker sees: exposed endpoints, error messages, redirect chains, cookie behavior.
- [ ] **What DAST is good at** — Runtime injection (SQLi, XSS, command injection reflected in responses), security header gaps, cookie misconfiguration (missing HttpOnly/Secure/SameSite), exposed admin/debug endpoints, TLS misconfiguration, open redirects, directory traversal.
- [ ] **What DAST is NOT good at** — Code-level issues (crypto API misuse, hardcoded secrets — use SAST), dependency CVEs (use SCA), business logic flows that require multi-step state (use manual pentest), issues requiring authentication context it can't establish.
- [ ] **DAST vs SAST vs Pentest** — DAST finds what's exploitable *right now* in the running system. SAST finds what's in the code. Pentest finds what humans can chain together. They complement, not replace.

---

## 2. Tool Selection

| Tool | Type | Best For | Cost |
|---|---|---|---|
| **OWASP ZAP** | Open-source DAST | CI integration, automated scans, API scanning | Free |
| **Burp Suite Community** | Intercepting proxy + manual | Manual exploration, ad-hoc testing | Free |
| **Burp Suite Professional** | DAST + manual + extensions | Professional pentest, crawl-and-scan | Paid |
| **Nuclei** | Template-based scanner | Fast, targeted checks for known patterns | Free |
| **Salt/Noname/42Crunch** | API-specific DAST | API security testing (API1–API10) | Paid |

- [ ] **Start with ZAP** — Free, CI-friendly, covers the OWASP Top 10. Sufficient for most small-to-medium teams.
- [ ] **Add Nuclei for targeted checks** — Template-based, fast, good for checking known CVEs/misconfigurations across the attack surface.
- [ ] **Burp for manual exploration** — When DAST finds a category but can't confirm exploitability, Burp lets a human drive the investigation.

---

## 3. OWASP ZAP Setup

### Baseline Scan (CI Integration)

- [ ] **Install** — Docker is easiest:
  ```bash
  docker pull ghcr.io/zaproxy/zaproxy:stable
  ```
- [ ] **Baseline scan** — Quick scan of a running app (passive rules + active rules on root):
  ```bash
  docker run -t ghcr.io/zaproxy/zaproxy:stable \
    zap-baseline.py -t https://staging.example.com -J zap-report.json
  ```
- [ ] **Full scan** — Crawls and actively attacks the app:
  ```bash
  docker run -t ghcr.io/zaproxy/zaproxy:stable \
    zap-full-scan.py -t https://staging.example.com \
      -J zap-report.json -r zap-report.html
  ```
- [ ] **API scan** — For OpenAPI/Swagger-based APIs:
  ```bash
  docker run -t ghcr.io/zaproxy/zaproxy:stable \
    zap-api-scan.py -t https://staging.example.com/openapi.json \
      -f openapi -J zap-report.json
  ```

### GitHub Actions Integration

- [ ] **ZAP scan in CI** —
  ```yaml
  # .github/workflows/dast.yml
  name: DAST (OWASP ZAP)
  on:
    schedule:
      - cron: "0 6 * * 1"  # Weekly Monday 06:00 UTC
    workflow_dispatch: {}
  jobs:
    zap:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v4
        - name: ZAP Baseline Scan
          uses: zaproxy/action-baseline@v0.13.0
          with:
            target: https://staging.example.com
            cmd_options: '-a -j'  # Active rules + AJAX spider
  ```

### Authentication Configuration

> DAST without auth only sees the unauthenticated surface. For apps behind login, DAST needs to authenticate.

- [ ] **Auth method configured** — ZAP supports: form-based login, HTTP auth, script-based auth. Configure the login URL, credentials, and logged-in indicator (a string/regex that only appears when authenticated).
- [ ] **Session management** — Cookie-based (ZAP handles automatically) or token-based (configure header injection). Verify ZAP maintains the session across requests.
- [ ] **Context defined** — Tell ZAP which URLs are in scope (your app) vs. out of scope (third-party CDNs, analytics). Prevents scanning things you don't own.
- [ ] **Test credentials are dedicated** — ZAP uses a test account, not a real user's credentials. The test account has representative permissions (not admin, unless testing admin surface).

---

## 4. Nuclei Setup

- [ ] **Install** —
  ```bash
  go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest
  ```
- [ ] **Basic scan** —
  ```bash
  nuclei -u https://staging.example.com
  ```
- [ ] **Targeted templates** —
  ```bash
  nuclei -u https://staging.example.com -t exposures/
  nuclei -u https://staging.example.com -t misconfiguration/
  nuclei -u https://staging.example.com -t cves/
  ```
- [ ] **CI integration** — Runs fast, good for nightly pipeline:
  ```bash
  nuclei -u https://staging.example.com -severity high,critical -json -o nuclei-report.json
  ```

---

## 5. Scan Scope & Frequency

| Scan Type | Frequency | Environment | Depth |
|---|---|---|---|
| ZAP Baseline | Every PR (if fast) or daily | Staging | Passive + active on root |
| ZAP Full Scan | Weekly | Staging | Crawl + active attack |
| ZAP API Scan | Every API change | Staging | OpenAPI-driven |
| Nuclei | Nightly | Staging + Prod (read-only) | Template-based checks |
| Burp Manual | Before launch, quarterly | Staging | Human-driven exploration |

- [ ] **DAST runs against staging, not local** — DAST against localhost misses TLS, headers, proxy, CDN behavior. Always scan a deployed environment.
- [ ] **DAST is scheduled, not one-off** — A scan done once during launch and never re-run misses new vulnerabilities introduced by code changes. Schedule recurring scans.
- [ ] **Production scanning is read-only** — If scanning prod, use passive mode only. Active scans can break things (submit forms, modify data). Use a staging environment for active scanning.

---

## 6. Triage & Remediation

- [ ] **Triage every finding** — DAST finds both real vulns and noise (alerts about "X-Frame-Options not set" on an API endpoint that doesn't render frames). Separate signal from noise.
- [ ] **Confirm exploitability** — DAST flags *potential* injection. Manually verify before treating as Critical. A SQLi alert on a read-only endpoint with no sensitive data may be Low.
- [ ] **Fix or suppress** — Same triage as SAST: fix, suppress with reason, or accept with owner. No finding sits in "ignored."
- [ ] **Track to closure** — Each finding has an owner, severity, and due date (per the remediation SLA in [[security]] §5). Track in the ticket system, not in a spreadsheet that rots.

---

## 7. Anti-Patterns to Avoid

- [ ] **DAST without authentication** — The scanner only sees the login page. Most vulnerabilities are behind auth. Configure auth or you're wasting the scan.
- [ ] **DAST as the only security test** — DAST finds runtime issues but misses code-level defects (SAST), dependency CVEs (SCA), and business logic flaws (pentest). Layer them.
- [ ] **Active scanning in production** — Active DAST sends attack payloads. It can submit forms, modify data, trigger alerts. Only scan staging/preview with active mode. Production gets passive-only.
- [ ] **One-time scan at launch** — Code changes introduce new vulns. DAST must be scheduled and recurring, not a launch-day checkbox.
- [ ] **No scope defined** — ZAP follows links. Without a scope, it may follow links to third-party sites, analytics, or CDNs — scanning resources you don't own. Define context.
- [ ] **Ignoring findings** — DAST reports accumulate. Untriaged findings become noise. Triage every batch within a defined SLA.
- [ ] **Trusting DAST alone for API security** — DAST is HTTP-centric. API-specific risks (BOLA/IDOR, mass assignment, excessive data exposure) need API-focused testing or manual verification.

---

## Quick Sanity Check

- [ ] OWASP ZAP installed and running against staging
- [ ] Authentication configured (DAST sees behind login)
- [ ] Scan scope defined (only your app, not third-party resources)
- [ ] DAST scheduled in CI (at minimum weekly full scan)
- [ ] API scan running if the app exposes an API (OpenAPI-driven)
- [ ] Findings triaged within remediation SLA, tracked to closure
- [ ] No active scanning against production (passive only)

---

## Sources

- Master checklist: [[security]] §9.
- Deep references: [[03 API Security]], [[04 Vulnerability Management]].
- Tools: OWASP ZAP, Burp Suite, Nuclei.
- Standards: OWASP Top 10, OWASP API Security Top 10 (2023).
