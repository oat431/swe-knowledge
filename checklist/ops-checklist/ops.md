# Operations & SRE Checklist

> Framework-agnostic checklist for steady-state operations: reliability targets, incident management, recovery, and operational readiness.
> The release gates get you to production; this keeps you healthy there.
> Last updated: 2026-11-15 (created)

---

## 1. SLIs, SLOs & Error Budgets

- [ ] **SLIs per user journey:** Service level indicators chosen for what users actually experience — latency (p95/p99 per journey), availability (successful requests / total), quality (freshness, correctness). Not CPU usage. One SLI set per user-facing journey, not per internal metric.
- [ ] **SLO targets with rationale:** Target chosen from evidence (past performance, user expectations, business cost of failure) and written down. "99.9% because that's what the contract says" is a reason; "99.99% because round numbers look good" is not. Aspirational SLOs produce permanent error-budget exhaustion and ignored alerts.
- [ ] **Error-budget policy that triggers action:** Written policy for what happens when the budget burns: freeze feature releases when the budget for the window is exhausted, prioritize reliability work. The policy must be referenced in release decisions → [[release]], or it's decoration.
- [ ] **Multi-window burn-rate alerting:** Fast window (e.g. 1h burn rate > 14x → page) plus slow window (e.g. 6h burn rate > 2x → ticket) catches both acute outages and slow bleeds without false-paging on single bad minutes. Standard pattern from the Google SRE book; adopt the thresholds or justify your own.

  ```
  fast burn:  1h window  > 14x budget consumption → page
  slow burn:  6h window  >  2x budget consumption → ticket
  both:       must trip on short AND long window to reduce noise
  ```
- [ ] **SLO dashboards per service:** One dashboard per service showing SLI vs SLO, remaining budget, and burn rate. Shared template so on-call reads every service the same way.
- [ ] **SLO review cadence:** Quarterly review of every SLO: is the target still right? Did we miss incidents the SLO didn't catch? Adjust targets deliberately, never silently during an incident.

## 2. Monitoring & Alerting

- [ ] **Alert on symptoms, not causes:** Page on what users feel (error rate up, latency up, journey failing), not on every possible cause (disk 80%, CPU 90% on one node). Cause-level signals belong on dashboards; only page when someone must act now → [[observability]].
- [ ] **Every page links a runbook:** No alert fires without an attached runbook or dashboard link telling the responder what to check first. An alert without a next step is a puzzle, not an alert.
- [ ] **Alert hygiene review:** Scheduled review (quarterly minimum): silence or delete alerts that have paged without action being needed, tune thresholds, split noisy alerts. Track pages-per-alert; the worst offenders get fixed or removed. Alert fatigue is a reliability defect.
- [ ] **Dashboards per service + dependency views:** Service dashboard (traffic, errors, latency, saturation) plus a dependency view showing downstream health. During an incident you need "is it me or them?" in seconds.
- [ ] **Logs, metrics, traces consistent:** The triad is correlated — trace ID present in logs, metrics derivable from the same instrumentation, all three covering the same service boundaries. If you can see the spike but can't find the request, the triad is broken → [[observability]].
- [ ] **Synthetic checks from outside:** Synthetic monitoring tool runs critical journeys (login, checkout, core API call) from external vantage points on a schedule. Catches DNS, TLS, CDN, and routing failures that inside-the-network monitoring never sees. Alert within a minute of two consecutive failures.

## 3. On-Call

- [ ] **Sustainable rotation:** Rotation large enough that no one is primary more than one week in four (two people is not a rotation). Handoff ritual at every shift change: open incidents, known degradations, active deploys, watch items.

  ```
  handoff = open incidents + degraded services + in-flight deploys
            + watch items + anything the pager surprised you with last shift
  ```
- [ ] **Escalation path with response targets:** Defined escalation chain with acknowledgement targets (e.g. ack within 5 min, escalate at 15 min). Secondary and management escalation documented and tested.
- [ ] **On-call handbook:** One page per team: how to get access (SSO, break-glass), where dashboards/logs/runbooks live, severity definitions, who to escalate to, what to do first when paged. Accessible without VPN (if the VPN is the thing that's down).
- [ ] **Paging test at rotation start:** New primary verifies paging actually reaches them (test alert at the start of every rotation). Silent pager = silent outage discovery.
- [ ] **Coverage depth:** Follow-the-sun where the org is global; otherwise a named backup for every shift and a rule for who picks up when the primary is unreachable. No single point of human failure.
- [ ] **Every page reviewed post-incident:** After each incident — or monthly for quiet teams — ask of every page: was it actionable? was it early enough? if not, fix the alert or the runbook. The page review is where alert hygiene actually happens.

## 4. Incident Management

- [ ] **Severity matrix with response targets:** Written matrix (e.g. SEV1: full outage or data loss; SEV2: major degradation; SEV3: minor/one customer) with response, communication, and postmortem requirements per level. Everyone uses the same words for the same things.

  | Severity | Example | Response | Comms | Postmortem |
  |----------|---------|----------|-------|------------|
  | SEV1 | Full outage, data loss | Page now, IC role | Status page every 30 min | Always, 5 days |
  | SEV2 | Major degradation | Page during hours | Stakeholder updates | Always |
  | SEV3 | Minor / one customer | Ticket | As needed | If pattern repeats |
- [ ] **Roles defined:** Incident commander (coordinates, decides), communications lead (status page, stakeholders), scribe (timeline). Named in the incident process and practiced in game days — during a real SEV1 nobody reads documentation for the first time.
- [ ] **Status-page updates:** User-visible incidents get status-page updates at a defined cadence (e.g. every 30 min for SEV1) from first acknowledgement through resolution. Silence during an outage generates support load and distrust → [[incident-response]] (the security-flavored sibling).
- [ ] **Customer comms templates ready:** Pre-written templates for outage start, update, and resolution — fill in blanks under pressure rather than compose prose. Reviewed by support and legal before they're needed.
- [ ] **Blameless postmortems within a fixed window:** Every SEV1/SEV2 gets a blameless postmortem within 5 business days. Focus on systems and conditions, not individuals. If people fear blame, you lose the honest timeline — and the timeline is where the fix lives.
- [ ] **Action items tracked to closure:** Every postmortem action has an owner and a due date, and lands in the same tracker the team already uses. Reviewed in sprint planning; "postmortem actions" that live only in the doc never get done.
- [ ] **Incident metrics tracked:** Time-to-detect, time-to-mitigate, time-to-recover trended over time. Name the metric explicitly when reporting: DORA renamed its deployment-recovery metric to "failed deployment recovery time" — plain "MTTR" is ambiguous (repair? recover? respond?), so say which one you mean.
- [ ] **Incident timeline preserved:** Scribe's timeline (detections, actions, decisions) kept with the postmortem. It is the raw material for every reliability improvement and for explaining to stakeholders what actually happened.

## 5. Backup & Recovery

- [ ] **RPO/RTO defined per system:** Recovery Point Objective (acceptable data loss) and Recovery Time Objective (acceptable downtime) agreed with the business and written per system. These numbers choose the backup technology — a 5-minute RPO rules out nightly dumps.

  | Tier | RPO | RTO | Typical mechanism |
  |------|-----|-----|-------------------|
  | Critical | minutes | minutes | Continuous replication + hot standby |
  | Standard | ≤ 1 hour | ≤ 4 hours | PITR + tested restore |
  | Tolerable | ≤ 24 hours | ≤ 24 hours | Daily backups, cold restore |
- [ ] **Restores tested on a schedule:** Restore drill monthly or quarterly per critical system, into a clean environment, results recorded with actual duration. A backup without a tested restore is a hope, not a backup.
- [ ] **Immutable/offsite copies:** At least one backup copy is immutable (write-once/locked retention) and offsite or in a separate account, so a compromise of production — or a ransomware-class event — cannot reach every copy at once.
- [ ] **Backup success monitored:** Backup jobs report success/failure into the monitoring stack; failures alert like any other production incident. Silent backup failure is a data-loss incident in slow motion.
- [ ] **Restore runbooks time-boxed and rehearsed:** Step-by-step restore procedure per system with an expected duration, rehearsed on the same schedule as the restore test. Per-engine database restore steps vary wildly (WAL shipping vs dumps vs snapshots) — link the engine-specific runbook where relevant → [[data]] (pipeline ops).

## 6. Resilience & Graceful Degradation

- [ ] **Dependency map:** Current map of what depends on what, and what breaks when each dependency dies (payment provider down → checkout down or checkout degraded?). Maintained, not aspirational → [[resilience]].
- [ ] **Degradation tiers defined:** Per critical journey, the fallback order is written down: shed load → serve cached/stale data → read-only mode → honest error. "Slightly stale" beats "completely broken"; decide the acceptable staleness before the incident.

  ```
  Tier 0: full service        Tier 2: cached / stale reads (max age: ___)
  Tier 1: shed load, queue    Tier 3: read-only mode     Tier 4: honest 503 + retry-after
  ```
- [ ] **Timeouts, retries, and budgets per dependency:** Every outbound call has a timeout (never unbounded), retries with backoff and jitter (never hot loops), and a retry budget so retries can't amplify an outage into a thundering herd → [[resilience]].
- [ ] **Game days on critical paths:** Scheduled fault-injection exercises (kill a dependency, add latency, fail a node) on the paths that matter, verifying that degradation tiers, alerts, and runbooks work as designed. Start in staging, graduate to production traffic slices.
- [ ] **Shutdown/drain tested:** Rolling restarts and node drains complete without dropped requests: SIGTERM handling, connection draining, deregistration delay verified under load. If a deploy causes error spikes, the drain procedure is broken.

## 7. Change Safety

- [ ] **Progressive deployment patterns:** Canary, blue-green, or flag-gated rollouts with defined promotion criteria and a fast rollback path → [[release]] covers the mechanics; operations owns that the pattern is actually used on every production change.
- [ ] **Config changes treated as risky as code:** Config and infrastructure tweaks go through review and gradual rollout exactly like code. The config change that took down production never went through a PR — make sure yours do.
- [ ] **Change freeze windows:** Known peak events (flash sales, marketing launches, holidays, regulatory dates) have declared freeze windows: no non-urgent production changes. The freeze is announced and enforced, with an exception process that isn't "just this once."
- [ ] **Feature-flag lifecycle managed:** Every flag has an owner and an expiry; flags are removed after the rollout completes. Permanent flags become an untested code maze — audit flags at least quarterly.
- [ ] **Post-deploy verification step:** Every change ends with an explicit verification (smoke checks, metric comparison against baseline) and a named person watching it. "Deployed" is not "done" → [[release]].

## 8. Capacity & Performance

- [ ] **Load model per service:** Known traffic shape per service (peak RPS, burst behavior, seasonality) and the load at which it breaks (measured, not guessed). The model is the input to every scaling and capacity decision.
- [ ] **Headroom policy:** Written policy (e.g. run at ≤ 60-70% of tested capacity at peak) so growth is absorbed before it becomes an incident. Breaching headroom is a capacity ticket, not a surprise outage.
- [ ] **Autoscaling bounds tested:** Scaling up *and* scaling down verified under realistic load — scale-up latency known (how long from trigger to serving), scale-in safe (cooldowns, connection draining). A ceiling misconfiguration turns autoscaling into a fixed fleet.
- [ ] **Load tests before known peaks:** Pre-peak load testing at expected peak plus margin → [[qa]] (load testing). Retail-class peaks (flash sale, Black-Friday-class) get a dedicated test, not a hopeful deploy.
- [ ] **Quarterly capacity review:** Trend usage vs capacity per critical system, compare to the load model, plan purchases/scaling before saturation. Findings feed the next quarter's plan.
- [ ] **Performance regressions tracked:** Latency/throughput per journey trended release-over-release; regressions are bugs with owners, not dashboard noise.

## 9. Maintenance Hygiene

- [ ] **Patch cadence with tracked backlog:** OS, runtime, and dependency patches applied on a defined cadence (e.g. monthly, security patches faster), with a visible backlog of what's pending and who owns it. Unpatched is a choice — make it a tracked one.
- [ ] **Certificate expiry tracked and alerted:** Every TLS certificate's expiry monitored with alerts well before expiry (30 days, 7 days, 1 day) — including certs nobody remembers (internal mTLS, webhook endpoints, load balancers). Expired certificates are the classic self-inflicted outage.
- [ ] **Secret rotation runbooks:** Documented and rehearsed rotation for every long-lived credential, with the rotation itself automated where possible → [[02 Secrets Management]].
- [ ] **Disk and log rotation:** Log rotation, retention, and disk-watermark alerts everywhere (including the nodes nobody watches: CI runners, bastions, log aggregators). A full disk at 3 AM is a preventable incident.
- [ ] **Config drift detection:** Periodic comparison of running configuration against the declared source of truth; drift alerts and reconciliation. Someone's console click should never be a discovery during an outage → [[infra]].
- [ ] **End-of-life tracking:** EOL/EOS dates for every runtime, database engine, and major dependency tracked with a planned upgrade date before the deadline (→ [[02 Dependency & Supply Chain]]-class tracking). "We'll upgrade before EOL" without a date is how unsupported software reaches production.

## 10. Access & Security Ops

- [ ] **Break-glass access exists and is tested:** Emergency access path for when normal auth is broken: documented, sealed/credential-protected, audited on use, reviewed after every use, and tested on a schedule. You cannot afford to invent it during an outage.
- [ ] **Infrastructure access logged:** All human access to production systems logged and retained (session recordings or command logs where feasible), alertable on sensitive actions → [[security]].
- [ ] **Least-privilege reviewed periodically:** Scheduled access review (quarterly): who has production access, is it still needed, any privilege creep? Revocations happen the same week they're identified.
- [ ] **Secrets rotation operational:** Rotation is a routine operation with a runbook and schedule, not an annual scramble → [[02 Secrets Management]].
- [ ] **Security patch SLAs:** Defined maximum time-to-patch per severity (e.g. critical internet-facing: days, not months) with the backlog visible to leadership. The SLA is measured; unmet SLAs escalate.

## 11. Human Factors

- [ ] **Toil budget with automation targets:** Define toil (manual, repetitive, automatable work) and set a budget (e.g. no more than ~30-50% of ops time). Over budget → automation work is planned, not hoped for. Measure it, or it's a slogan.
- [ ] **Runbooks reviewed after every real use:** After any runbook is used in anger: did it work? update it in the same week. A runbook's last successful use date is its most important metadata.
- [ ] **Game days and tabletop exercises:** Regular exercises — technical game days (see §6) and tabletop walkthroughs of hypothetical incidents (dependency outage, data breach, bad deploy at peak). Practicing the coordination matters as much as practicing the fix.
- [ ] **No single-person knowledge:** Pair on-call or shadow rotations so every critical system has at least two people who have handled it at 3 AM. Bus factor of one on the payment service is a production risk, documented like any other.
- [ ] **Ops load visible to planning:** On-call load, incident count, and open reliability work reported into product/engineering planning so reliability work competes fairly for capacity instead of being "extra."

## 12. Continuity

- [ ] **DR tiers per service:** Business-continuity tier per service (hot/warm/cold standby) chosen from its RPO/RTO (§5) — not every service deserves hot-hot, but every service deserves a *decision*.
- [ ] **Regional failover only claimed if tested:** "We can fail over to region B" counts only after a real failover test with measured RTO. Untested failover is a hope in the architecture diagram.
- [ ] **Business-continuity tabletop:** Annual (minimum) tabletop covering the non-technical halves: who talks to customers, regulators, and press; who authorizes a failover; where the money and people come from → [[privacy]] (breach notification timelines live here too).
- [ ] **Vendor-down plans:** For every critical third party: what do we do if they're down for a day? (queue and retry, degraded mode, manual fallback, switch provider). The answer exists before the vendor's status page goes red → [[finops]] (runaway costs from retries and fallbacks deserve an alert too).
- [ ] **Data pipeline continuity:** Batch and streaming pipelines have freshness SLAs, backfill procedures, and a named owner for recovery → [[data]].

---

## Quick Sanity Check Before Release

> The steady-state non-negotiables. If any of these are "no," production is running on luck.

- [ ] Every user-facing service has SLIs, an SLO with written rationale, and a working burn-rate alert
- [ ] Error-budget policy is real: the team can name the last time it changed a release decision
- [ ] Every page-worthy alert links a runbook, and the last hygiene review is less than a quarter old
- [ ] On-call rotation is staffed, paged, and tested this rotation; handbook is reachable without production access
- [ ] Restores tested within the last 90 days with recorded durations; at least one backup copy is immutable
- [ ] Degradation tiers and timeouts defined for the top dependencies; game day run in the last two quarters
- [ ] Cert expiry, secret rotation, and patch backlog all have owners and alerting — no silent deadlines
- [ ] Incident metrics (time-to-detect, time-to-recover) trended, postmortem actions closed on schedule

---

## Sources

- Google SRE book — *Service Level Objectives*, *Alerting on SLOs*, *Emergency Response*, *Managing Incidents*, *Postmortem Culture*: sre.google/sre-book
- Atlassian incident communication guides (status pages, incident comms templates, severity models): atlassian.com/incident-management
- NIST contingency planning guidance (SP 800-34, contingency/DR planning, RPO/RTO concepts): csrc.nist.gov
- Release gate: [[release]].
