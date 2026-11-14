# FinOps & Cloud Cost Checklist

> Framework-agnostic checklist for cloud cost discipline: visibility, accountability, optimization, and guardrails.
> Cost is an architecture property — if it's not designed, it drifts.
> Last updated: 2026-11-15 (created)

---

## 1. Visibility & Allocation

- [ ] **Tagging/labeling policy enforced at creation:** Required tags: `environment`, `team`, `service`, `cost-center` (plus `managed-by` where useful). Enforced by pipeline policy or org-level deny rules, not wiki pages. Untagged = unallocated = unmanageable. Provider-specific enforcement patterns in [[aws-infra]].
- [ ] **Per-service/per-team cost reporting that reconciles to the bill:** The sum of your per-team/per-service breakdowns should match the invoice within a known margin (credits and unallocated shared costs explained, not hidden).
- [ ] **Shared-cost allocation rules documented:** Networking, observability, platform, and security tooling costs are shared — allocate by a stated rule (even split, usage-weighted, headcount) via showback or chargeback. The rule matters less than it being written down and consistent.
- [ ] **Daily spend anomaly alerts:** Alert on spend deltas (day-over-day and against trailing baseline), not just monthly totals. A runaway resource discovered on day 28 is a postmortem; discovered on day 2 is a ticket.
- [ ] **Budget alerts per team and per environment with named owners:** Every alert routes to a human who can act, not a shared inbox. Thresholds set against forecast, not just the monthly ceiling.
- [ ] **Cost allocation tags activated in the billing console/exports:** Tags that exist on resources but aren't activated as cost allocation keys never appear in cost reports. Verify by slicing one month's bill by each required tag.
- [ ] **Raw billing export to queryable storage:** Daily billing export landed in object storage and queryable by the data stack, so cost questions are answered with SQL, not screenshots of a console ([[data]] for the warehouse side).

```yaml
# minimal required tags — enforced at creation, not audited later
environment: prod | staging | dev   # which lifecycle tier is this?
team: owning-team                   # who gets the bill line?
service: service-name               # what is this for?
cost-center: billing-code           # which budget absorbs it?
```

## 2. Budgets & Forecasting

- [ ] **Budget vs forecast vs actual reviewed on a cadence with recorded actions:** A review meeting without recorded decisions is a dashboard demo. Every variance that crosses a threshold gets an owner and an action.
- [ ] **Traffic→cost model (unit economics):** Cost per request, transaction, user, or job tracked over time. Total spend tells you nothing about efficiency; unit cost tells you whether growth is paying for itself. FinOps Foundation framing: inform → optimize → operate.
- [ ] **Variance explained, not just reported:** "Compute is up sharply" is a number; "compute is up because of the new indexing workload" is an explanation. Month-over-month deltas get a one-line cause or an investigation.
- [ ] **Actuals closed before the review:** Credits, commitment amortization, and billing adjustments settled before the meeting — reviewing mid-cycle actuals means reviewing noise.
- [ ] **Growth projections tied to product plans:** Forecast built from expected traffic/feature growth, not an extrapolation of last month. A launch next quarter should appear in next quarter's forecast before the code merges.
- [ ] **Forecast accuracy tracked:** Record what you forecast vs what landed; a forecasting model nobody scores is astrology. Misses get postmortemed the same way incidents do.
- [ ] **Seasonality and one-off events modeled separately:** Flash sales, batch windows, and training runs should be modeled lines, not "unexplained growth."

```
unit cost = total service cost / business volume   (requests, orders, users, jobs)
track it weekly — total spend can rise while unit cost falls (that's healthy growth)
```

## 3. Compute Optimization

- [ ] **Rightsizing from utilization data, not instinct:** Collect CPU/memory/network utilization over a meaningful window (weeks, not hours) before resizing. The cloud's rightsizing recommender-class tooling is a source of candidates, not gospel — spiky workloads look idle in averages.
- [ ] **Autoscaling bounds tuned to the demand curve — and downscaling tested:** Max bounds are a cost ceiling; min bounds are a reliability floor. Test scale-in explicitly: most teams only ever exercise scale-out, then discover pinned capacity months later.
- [ ] **Spot/preemptible capacity for fault-tolerant workloads only:** Batch jobs, CI runners, stateless workers, dev/staging. Never for single-instance stateful services. Interruption handling (checkpointing, graceful drain) verified before the discount is claimed.
- [ ] **Scheduled scaling for non-prod:** Dev/staging sized down or off outside working hours — for a typical team this is one of the largest single savings available. Automate the schedule; don't rely on people remembering.
- [ ] **Commitment-based discounts (reserved/savings-plan-class) with a coverage target that is deliberately below 100%:** Commit only to the stable baseline; keep burst on flexible capacity. Review coverage and utilization quarterly — an expiring commitment is a decision, not an auto-renew.
- [ ] **Zombie capacity hunted on a schedule:** Underused instances, orphaned disks, unattached IPs, stale snapshots, idle load balancers, forgotten managed clusters. Automated scan → weekly report → owner → cleanup, with a grace period and a "keep" tag for exceptions.
- [ ] **Instance family/processor mix reviewed:** Newer generation and ARM-class instances often deliver better price-performance for the same workload (verify compatibility). Re-evaluate family choice when rightsizing, not just the size.

## 4. Storage & Data Transfer

- [ ] **Lifecycle tiers (hot → warm → cold → archive) aligned to retention policy:** Tiering decisions are retention decisions; agree them with the data owners and encode them as lifecycle rules ([[database]] for retention mechanics, [[privacy]] for the retention-vs-minimization tension — keeping data forever is both a cost and a compliance liability).
- [ ] **Egress is the surprise line item:** Understand the charging patterns for cross-AZ, cross-region, and internet egress, and the NAT-gateway-class data processing charges. Keep chatty service-to-service traffic in one AZ/region where the architecture allows; use private endpoints for cloud API traffic.
- [ ] **Snapshot/backup retention tuned:** Infinite snapshots = infinite spend. Retention windows defined per data class; automated expiry; quarterly audit of the snapshot estate ([[database]]).
- [ ] **Warehouse/lake query cost control:** Partition pruning enforced, expensive queries reviewed, materialization strategy chosen deliberately (compute-on-read vs precomputed), per-query or per-team budgets on the analytical engine ([[data]]).
- [ ] **Log and telemetry retention tiered:** Hot retention for debugging windows, cheap archive beyond it; debug/verbose logging not shipped at production volume long-term (log volume is a cost multiplier [[infra]]).
- [ ] **Replication and redundancy are deliberate purchases:** Cross-region replication, multi-AZ standby, and extra read replicas are bought to hit a stated RPO/RTO — sized to the requirement, not to comfort ([[database]]).
- [ ] **Storage class review for infrequently accessed data:** Access patterns measured before choosing a tier; intelligent-tiering-class options for unpredictable patterns; incomplete multipart uploads and old artifact versions cleaned up automatically.
- [ ] **Data-transfer architecture reviewed in design:** CDN and edge caching for hot read paths (also cuts origin compute [[infra]]); bulk transfers planned around region boundaries rather than crossing them per-request.

## 5. Environment Hygiene

- [ ] **Dev/staging auto-stop schedules:** Nights/weekends off or scaled down, enforced by scheduler tooling, with an override for demos and crunches that itself expires.
- [ ] **Ephemeral preview environments with TTL:** Per-PR/per-feature environments are great for testing and terrible for cost unless every one has an expiry. TTL enforced by the platform, not by a Slack reminder ([[release]] for the delivery side).
- [ ] **Expired sandboxes and POCs cleaned up:** Sandboxes get creation dates and owners; anything past its declared end date is reclaimed or re-approved. "Temporary" infrastructure outlives the person who created it.
- [ ] **Non-prod downgraded to smaller families:** Staging that mirrors production architecture doesn't need production size. Parity in shape, not in volume ([[release]] for environment promotion).
- [ ] **Resource quotas per team:** Quotas on instances, storage, and expensive managed services per team/environment. A quota that must be raised is exactly the moment for a cost conversation.
- [ ] **Duplicate non-prod environments consolidated:** When three teams each run a full staging stack, check whether one shared staging plus ephemeral previews ([[release]]) is enough. Isolation is worth paying for only where it changes an outcome.
- [ ] **Experimentation accounts have spend caps:** Sandboxes and R&D accounts get hard or near-hard budget limits and near-real-time alerts; a research account with no ceiling is how five-figure surprises happen.

## 6. Architecture Cost Decisions

- [ ] **Managed-vs-self-hosted TCO modeled:** Your ops hours have a price. Compare the managed service bill against the fully loaded cost of running it yourself (people, on-call, upgrades, incidents), not just the instance bill.
- [ ] **Serverless-vs-always-on inflection points modeled:** Per-request pricing wins at low/variable volume and loses at high steady volume. Know your crossover; revisit it as traffic grows. "Serverless is cheaper" is a hypothesis, not a rule.
- [ ] **Caching/CDN on hot paths:** Cuts both compute and egress on the paths that dominate the bill ([[infra]]). Cache hit rate is a cost metric as much as a latency metric.
- [ ] **SaaS sprawl reviewed:** Seat-usage audits (how many assigned seats are actually used?), overlapping tools consolidated, auto-renewal dates owned by someone. SaaS is part of the technology cost even when it never touches the cloud bill.
- [ ] **Cost impact line in significant ADRs:** Architecture decision records for major choices state the expected cost shape (steady, usage-proportional, front-loaded) and the revisit trigger. See [[infra]] for the ADR/decision-log habit.
- [ ] **Data gravity respected in placement:** Move compute to the data, not data to the compute. A pipeline that cross-region-shuffles a large dataset daily is paying a transfer tax on every run ([[data]]).
- [ ] **"What does this cost at 10x?" asked before building:** Cost scaling modeled at projected growth, not current traffic. Non-linear cost growth (per-request pricing on hot paths, cross-AZ chatter) discovered on a whiteboard, not an invoice.

## 7. Guardrails

- [ ] **Policy-as-code blocks at provision time:** Deny oversized instance classes without approval, public snapshots/buckets, non-approved regions, and unapproved resource classes. Blocked at pipeline or org-policy level ([[aws-infra]] for provider-native enforcement patterns).
- [ ] **Pre-provision review for large resources:** Anything above a defined size/cost class gets a review before creation — the same way production access gets a review. Cheap to ask before, expensive to unwind after.
- [ ] **Tagging enforced by pipeline policy:** IaC modules apply required tags by default so humans can't forget them; drift or console-created untagged resources are detected and flagged ([[infra]] for IaC tagging discipline).
- [ ] **Budget actions that auto-remediate:** On threshold breach: notify → restrict new provisioning → (for non-prod) stop or downgrade. Escalating actions, not just escalating emails.
- [ ] **Commitment purchases gated by approval:** Reserved/savings-plan-class purchases are reviewed and approved (they are multi-year financial commitments), never ad-hoc console clicks by whoever noticed the bill.
- [ ] **Region and service allowlists:** Workloads run in an approved set of regions and on approved service classes; "it was cheaper in an unapproved region" is not a win if it's unmonitored.

```
# policy-as-code guardrails (conceptual — expressed in the org's policy tooling)
deny  instance_class > approved_max      unless approval_tag present
deny  region not in allowlist
deny  public_snapshot == true
warn  resource missing required_tags     -> route to owning team
```

## 8. Runaway-Cost Resilience

- [ ] **Retry storms and looping jobs have cost circuit-breakers:** Infinite retry = infinite bill. Max-attempt caps, exponential backoff, and a global kill switch on anything that scales with failure ([[ops]] for the incident patterns).
- [ ] **Per-job cost budgets for pipelines and backfills:** Data jobs, reprocessing, and backfills carry an estimated cost before they run and a hard ceiling during ([[data]]). A bad JOIN or an unpartitioned scan is a fiscal incident.
- [ ] **Anomaly alert → owner → kill switch documented:** When a spend anomaly fires at 2 AM, who is paged, what do they check first, and how do they stop the bleeding? Written as a runbook and rehearsed ([[ops]]).
- [ ] **Post-incident reviews include cost impact when relevant:** Incidents that burned money (retry storms, autoscaling runaway, data reprocessing) record the cost in the review — reliability and cost failures share root causes.
- [ ] **Concurrency and rate caps on unbounded work:** Autoscaling max, serverless concurrency limits, and queue consumer caps are cost ceilings. Set them explicitly instead of inheriting defaults.
- [ ] **Compromised-account spend anomalies treated as security events:** Crypto-mining and credential abuse show up first on the bill. Spend-anomaly alerts route to security as well as FinOps ([[ops]]).

## 9. FinOps Practice

- [ ] **Named owner per cost domain:** Compute, data, networking, SaaS — each has a person accountable for its trend line. The FinOps loop only works when "inform" has someone to inform.
- [ ] **Monthly cost review with tracked actions and realized savings:** Attendance is not an outcome. Every review ends with actions, owners, deadlines, and a check on last month's actions.
- [ ] **Savings initiatives measured (promised vs realized):** Rightsizing, tiering, and commitment initiatives record the projected saving and then the actual saving on the bill. An unmeasured saving is a hope.
- [ ] **Forecasting model revisited quarterly:** Business shape changes (new products, new markets, new architecture); the model must change with it. Ties back to section 2.
- [ ] **Peak-event cost planning:** For flash-sale-class events, model the event's cost before the event: pre-warming, autoscale headroom, and CDN spend are deliberate purchases. Model the event; don't discover the bill ([[ops]] for capacity planning).
- [ ] **FinOps is an engineering property, not a finance report:** Cost discussed in design and architecture reviews; engineers can answer "what does my service cost per day?" without filing a ticket. Inform → optimize → operate, as a loop, not a project.

```
monthly cost review agenda (keep it to 30 minutes)
  1. actual vs forecast vs budget — variances explained
  2. top movers since last review — up and down
  3. open savings actions: promised vs realized
  4. commitments: utilization now, expirations coming due
  5. decisions and owners recorded before the meeting ends
```

---

## Quick Sanity Check Before Release

> The non-negotiables. If any of these are "no," your cost picture is fiction.

- [ ] Every production resource carries `environment`/`team`/`service`/`cost-center` tags — verified by slicing one month's bill by each tag
- [ ] Per-team cost breakdown reconciles to the total invoice, with shared costs allocated by a written rule
- [ ] Daily spend anomaly alerts exist and have fired in a test (alert delivery verified, owner named)
- [ ] Non-prod auto-stop/scale-down schedules active and verified to actually stop things
- [ ] Commitment coverage target documented and reviewed within the last quarter (and deliberately below 100%)
- [ ] Runaway-cost kill switch (retry caps, concurrency caps, budget actions) documented and rehearsed
- [ ] Unit-economics metric (cost per request/transaction/user) defined, published, and trending
- [ ] Egress and cross-AZ/region traffic patterns understood — no unexplained line items in the top 5 cost drivers

---

## Sources

- FinOps Foundation — FinOps Framework (inform/optimize/operate, unit economics, showback/chargeback): https://www.finops.org/framework/
- AWS Cost Management documentation (Cost Explorer, Budgets, cost allocation tags, anomaly detection): https://docs.aws.amazon.com/cost-management/
- Microsoft Azure Cost Management + Billing documentation: https://learn.microsoft.com/azure/cost-management-billing/
- Google Cloud cost management documentation (billing reports, budgets, recommendations): https://cloud.google.com/cost-management

Related checklists: `→ [[infra]]`, `→ [[aws-infra]]`, `→ [[database]]` (storage tiers/retention), `→ [[release]]`, `→ [[ops]]` (runaway-cost incidents, capacity), `→ [[data]]` (warehouse query cost), `→ [[privacy]]` (retention vs minimization).

Infrastructure sibling: [[infra]].
