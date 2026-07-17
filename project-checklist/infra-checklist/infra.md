# DevOps & Infrastructure Checklist

> Cloud-agnostic checklist for production infrastructure.
> AWS, GCP, Azure, or self-hosted — the principles are the same. The services change.
> Last updated: 2026-07-17

---

## 1. Infrastructure as Code

- [ ] **IaC tool chosen** — Terraform (ecosystem, multi-cloud), Pulumi (real programming languages), CDK (AWS-native), OpenTofu (open-source Terraform fork). Pick one, commit to it. No ClickOps in production.
- [ ] **Remote state** — State file in shared storage (S3 + DynamoDB, GCS, Azure Blob, Terraform Cloud). Never local state for team projects. State contains secrets — encrypt at rest, restrict access.
- [ ] **State locking** — Prevent concurrent modifications. DynamoDB lock table (AWS), GCS native locking, or backend-native locking. Two `terraform apply` at the same time = corrupted state.
- [ ] **Modules for reusability** — DRY infrastructure. Reusable modules for common patterns (VPC, EKS cluster, RDS instance). Version-pinned modules (`source = "git::...?ref=v1.2.0"`). Internal module registry for the org.
- [ ] **Environments from same code** — dev/staging/prod use the same modules, different variables. Workspaces, directory structure (`envs/dev/`, `envs/prod/`), or Terragrunt. Drift between environments = incidents waiting to happen.
- [ ] **Plan before apply** — Every change shows a plan. PR shows diff. No blind `terraform apply`. CI runs `plan` on PR, `apply` only after merge + approval. `plan` output saved as artifact for audit.
- [ ] **PR-based infra changes** — Infrastructure changes go through code review like application code. PR → plan output as comment → approval → merge → apply. Atlantis, Spacelift, Env0, or CI pipeline.
- [ ] **Drift detection** — Scheduled `plan` to detect out-of-band changes. Alert on drift. Someone `kubectl edit`'d or clicked in the console — now your state is a lie. Reconcile or import.
- [ ] **Blast radius control** — Small, focused state files. Don't put VPC + RDS + EKS + IAM in one state. One blast = everything. Split by layer (network, compute, data) or by service boundary.
- [ ] **Policy as code** — OPA/Rego, Sentinel, Checkov, tfsec. Enforce guardrails: no public S3 buckets, no overly permissive security groups, encryption required. Block non-compliant plans in CI.
- [ ] **Import existing resources** — `terraform import` or `pulumi import` for brownfield. Don't recreate what already exists. Document imported resources. Validate state matches reality after import.
- [ ] **Cost estimation in PR** — Infracost or similar in CI. Show cost impact of infra changes before merge. "This PR adds $450/month." No surprise bills from merged PRs.
- [ ] **Tagging in IaC** — Default tags applied to all resources via provider default_tags or module inputs. Environment, team, service, managed-by. Never rely on humans remembering to tag — enforce at the code level.

## 2. Networking

- [ ] **VPC/VNET design** — Dedicated VPC per environment. CIDR planning that allows peering without overlap (`10.0.0.0/16` dev, `10.1.0.0/16` staging, `10.2.0.0/16` prod). Document IP allocation. Don't use the default VPC for anything production.
- [ ] **Public/private subnets** — Public subnets: load balancers, NAT gateways, bastions only. Private subnets: application servers, databases, internal services. Nothing with sensitive data directly internet-facing.
- [ ] **NAT gateways** — Private subnets need outbound internet for package installs, API calls, telemetry. NAT gateway (managed) or NAT instance (cheaper, more ops). One per AZ for HA. Monitor bandwidth — NAT is a cost center.
- [ ] **Security groups / NACLs** — Security groups: stateful, per-resource, allowlist only. NACLs: stateless, subnet-level, explicit deny capability. Principle of least privilege — only open what's needed. No `0.0.0.0/0` ingress unless it's a public LB.
- [ ] **DNS management** — Route53 / Cloud DNS / Azure DNS. Infrastructure-managed DNS records (IaC, not manually). TTL strategy: low for services that change (60s), high for stable records (3600s). Alias/CNAME for load balancers.
- [ ] **Load balancers** — ALB/NLB (AWS), Cloud Load Balancing (GCP), Application Gateway (Azure). TLS termination at LB. Health checks configured. Connection draining enabled. Access logs for debugging and compliance.
- [ ] **CDN** — CloudFront, Cloud CDN, Azure CDN, or Cloudflare. Static assets, media, and cacheable API responses. Cache invalidation strategy defined. Custom error pages. Origin shield to reduce origin load.
- [ ] **VPN / private connectivity** — Site-to-site VPN or Direct Connect/ExpressRoute/Interconnect for on-prem connectivity. No SSH over public internet to production. Bastion host or SSM Session Manager / IAP tunnel for emergency access.
- [ ] **Network segmentation** — Microservices can't all talk to everything. Network policies (K8s), security groups (VMs), service mesh mTLS. Database only reachable from application tier. Zero trust: verify every connection.
- [ ] **Transit gateway / hub-spoke** — For multi-VPC architectures. Central routing, shared services VPC, simplified peering. Not 50 point-to-point peering connections.
- [ ] **TLS certificate management** — ACM (AWS), Google-managed certs, or cert-manager (K8s). Auto-renewal mandatory — expired certs cause outages. Monitor certificate expiry (alert 30 days before). No self-signed certs in production. TLS 1.2 minimum, prefer 1.3.
- [ ] **Service discovery** — Internal DNS (Route53 Private Hosted Zones, Cloud DNS private zones), Consul, or K8s built-in DNS. Services find each other by name, not IP. No hardcoded IPs in config — IPs change, names don't.

## 3. Compute

- [ ] **Container orchestration** — Kubernetes (EKS/GKE/AKS), ECS, Cloud Run, Nomad. Pick based on complexity tolerance. K8s: powerful, complex, ecosystem. ECS/Cloud Run: simpler, less flexibility, faster to production. Don't run K8s if you don't need it.
- [ ] **Serverless where it fits** — Lambda/Cloud Functions/Azure Functions for event-driven, low-traffic, or spiky workloads. Not for long-running, high-throughput, or stateful. Cold starts matter for user-facing latency. Provisioned concurrency if cold starts are unacceptable.
- [ ] **VM management (if using)** — Immutable images (AMI/machine image baked in CI). No SSH to patch — rebuild and redeploy. Configuration management (Ansible/Chef) only if mutable servers are unavoidable. Golden image pipeline.
- [ ] **Auto-scaling policies** — CPU/memory-based for baseline. Custom metrics (queue depth, request count, active connections) for accuracy. Scale-out fast, scale-in slow (cooldown prevents flapping). Test that scaling actually works under load.
- [ ] **Spot/preemptible instances** — 60-90% cost savings for fault-tolerant workloads: batch processing, CI runners, dev/staging environments, stateless workers. Not for databases, not for single-instance services. Handle interruption gracefully (2-minute warning).
- [ ] **Right-sizing** — Don't guess instance types. Monitor actual CPU/memory utilization for 2+ weeks. Downsizing from m5.2xlarge to m5.large saves ~$2000/year per instance. Review quarterly. AWS Compute Optimizer, GCP Recommender.
- [ ] **Node groups / instance diversity** — Multiple instance types in auto-scaling groups. Avoid "no capacity" errors. Mix on-demand (baseline) + spot (burst). Separate node groups for different workload profiles (compute-heavy, memory-heavy).
- [ ] **Resource requests & limits (K8s)** — Every pod has CPU/memory requests (scheduling) and limits (protection). No limits = noisy neighbor. Requests too high = wasted capacity. Measure actual usage, tune accordingly.
- [ ] **Cluster upgrades planned** — K8s EOL is ~14 months per minor version. Stay within N-2 of latest. Test upgrades in staging. Blue-green node groups for zero-downtime upgrades. Subscribe to release notes for deprecations.

## 4. Storage & Databases

- [ ] **Managed database provisioning** — RDS, Cloud SQL, Azure Database. Not self-managed PostgreSQL on EC2 (unless you have a DBA team and a good reason). Multi-AZ for production. Appropriate instance size + storage type (gp3/io2). Maintenance windows scheduled.
- [ ] **Read replicas** — Offload read-heavy queries. Application-level routing (write → primary, read → replica). Be aware of replication lag (~ms for same-region, seconds for cross-region). Cross-region replicas for DR.
- [ ] **Automated backups** — Enabled with appropriate retention (7-35 days). Verify backups exist — check, don't assume. Backup window during low-traffic periods. Snapshot before risky operations (migrations, upgrades).
- [ ] **Point-in-time recovery** — PITR enabled (continuous WAL archiving). Know how to restore to a specific timestamp. Test the restore procedure quarterly. Document the steps in a runbook. Know the recovery time for your data size.
- [ ] **Connection pooling** — PgBouncer, RDS Proxy, ProxySQL. Database connections are expensive (~5-10MB RAM each). App-level pooling + proxy-level pooling. Don't exhaust `max_connections` (default 100-200 on most managed DBs). Monitor pool saturation.
- [ ] **Object storage** — S3/GCS/Azure Blob for files, backups, logs, artifacts. Not local disk (not durable, lost on termination). Bucket policies: private by default. Versioning for critical buckets. Cross-region replication for DR.
- [ ] **Encryption at rest** — All databases, all object storage, all EBS/persistent volumes. Managed keys (KMS) or customer-managed keys (CMK) for compliance. Encryption in transit (TLS) between app and database.
- [ ] **Lifecycle policies** — Old logs → Glacier/Coldline after 30 days → delete after 1 year. Old backups expire per retention policy. Incomplete multipart uploads cleaned up (7 days). S3 intelligent tiering for unpredictable access patterns.
- [ ] **Database upgrades planned** — Major version upgrades tested in staging first. Blue-green deployment for zero-downtime upgrades. Know your engine's EOL dates. Don't run unsupported versions in production.
- [ ] **Caching infrastructure** — Redis/Memcached/ElastiCache for application caching. Clustered mode for HA and scaling. Separate cache from session store. Eviction policy configured (allkeys-lru for cache, noeviction for sessions). Monitor hit rate — below 80% means cache isn't helping.

## 5. CI/CD Pipeline Infrastructure

- [ ] **Build agents** — Managed (GitHub Actions, GitLab SaaS runners, CodeBuild) for simplicity. Self-hosted for security/compliance, custom tooling, or cost at scale. Self-hosted = you maintain, patch, and scale them.
- [ ] **Artifact registry** — ECR/GAR/ACR for container images. Nexus/Artifactory/Harbor for generic artifacts. Private, vulnerability-scanned, lifecycle policies (delete untagged images after 7 days, keep last N tags).
- [ ] **Pipeline as code** — `.github/workflows/`, `.gitlab-ci.yml`, `Jenkinsfile`, `buildspec.yml`. Versioned, reviewed, tested. No UI-configured pipelines that disappear when someone leaves. Reusable workflow templates for consistency.
- [ ] **Deploy automation** — ArgoCD, Flux (GitOps), Spinnaker, or CI-driven `kubectl apply`/`helm upgrade`. No manual deployments to production. `git merge` → deploy. Human approvals for production gate, not manual execution.
- [ ] **Environment promotion** — Build once → deploy to dev → test → promote to staging → test → promote to production. Same artifact, different config. Image tag = git SHA, not `:latest`. Promotion is a config change, not a rebuild.
- [ ] **Rollback automation** — One-click (or one-merge) rollback. Revert the git commit, or redeploy previous image tag. ArgoCD: `argocd app rollback`. Helm: `helm rollback`. Tested, documented, practiced. Know rollback time.
- [ ] **Pipeline security** — Secrets not logged, not in plaintext. OIDC for cloud auth (no long-lived credentials in CI). Branch protection on deploy pipelines. Signed commits or verified actors for production deploys. Immutable build environments.
- [ ] **Build caching** — Docker layer caching, dependency caching (npm, Go modules, pip), build output caching (Turborepo, Gradle). 10-minute builds → 2-minute builds. Cache invalidation on lockfile change.
- [ ] **Canary / progressive delivery** — Flagger, Argo Rollouts, or cloud-native (CodeDeploy). Route 5% traffic → monitor error rate + latency → auto-promote or auto-rollback. Not big-bang deploys hoping nothing breaks.
- [ ] **Environment parity** — Staging mirrors production architecture (same services, same network topology, same config structure). Differences documented. If it works in staging, it should work in production. Use production-like data volumes for realistic testing.
- [ ] **Ephemeral environments** — Spin up full environment per PR or feature branch. Tear down after merge. Developers test in isolation without blocking shared staging. Infrastructure-as-code makes this possible.

## 6. Secrets & Configuration

- [ ] **Secrets manager** — HashiCorp Vault, AWS Secrets Manager, GCP Secret Manager, Azure Key Vault. Centralized, audited, access-controlled. Not environment variables in CI settings (no rotation, no audit trail, no versioning).
- [ ] **Never in code or git** — No credentials in source code. No `.env` files committed. `git-secrets`, `gitleaks`, or `trufflehog` in pre-commit hooks and CI. Scan git history — secrets committed and "removed" are still in history. Rotate if found.
- [ ] **Rotation policies** — Database passwords: 90 days max. API keys: 90 days. TLS certificates: auto-renewed (cert-manager, ACM, Let's Encrypt). Service account keys: avoid entirely (use OIDC/workload identity). Automate rotation — manual rotation is forgotten rotation.
- [ ] **Environment-specific config** — ConfigMaps (K8s), Parameter Store (AWS), runtime config (GCP). Not baked into images. Different values per environment, same code. Config validation at startup (fail fast on missing config).
- [ ] **External Secrets Operator** — Kubernetes: sync secrets from Vault/AWS SM/GCP SM into K8s Secrets automatically. No manual `kubectl create secret`. Secrets rotate without pod restart (if using Reloader or stakater/Reloader).
- [ ] **Least-privilege access to secrets** — Not every service reads every secret. Scope access by service/team/environment. Audit who accessed what and when. Alert on unusual access patterns (new accessor, bulk reads).
- [ ] **Secret zero problem** — How does the first secret get to the application? Instance metadata (IMDS), workload identity, or bootstrapped via CI with OIDC. Avoid turtles-all-the-way-down credential chains.

## 7. Identity & Access Management

- [ ] **Principle of least privilege** — Every role, policy, and permission: minimum needed to function. Start with zero access, grant specifically. Regularly review and revoke unused permissions. `Access Analyzer` (AWS), IAM Recommender (GCP).
- [ ] **Service accounts** — Dedicated identity per service. Not shared `admin` accounts. Not developer personal credentials in production. Workload identity (GKE), IRSA (EKS), or pod identity (AKS) for K8s. No key files on disk.
- [ ] **Role-based access** — IAM roles/policies, not inline permissions. Predefined roles for common patterns. Custom roles only when predefined are too broad. Group/team-level assignment, not per-user. Document role definitions.
- [ ] **No long-lived credentials** — No static access keys where possible. Use OIDC federation (CI/CD → cloud), instance profiles, workload identity. If you must have keys: rotate programmatically, alert on age > 90 days, disable unused keys.
- [ ] **Federated identity (SSO)** — SAML/OIDC integration with corporate identity provider (Okta, Azure AD, Google Workspace). Single source of truth for user lifecycle. Offboarded employee = access revoked everywhere within minutes, not days.
- [ ] **Audit trail** — CloudTrail (AWS), Cloud Audit Logs (GCP), Activity Log (Azure). Who did what, when, from where. Retained for compliance period (1-7 years). Alert on sensitive actions (IAM changes, security group modifications, root account login).
- [ ] **Break-glass procedures** — Emergency access for outages when normal access is down. Documented, audited, time-limited. Sealed credentials in physical safe or MFA-protected vault. Post-incident review of all break-glass usage. Test annually.
- [ ] **MFA everywhere** — Enforce MFA for all human access to cloud consoles, VPN, and production systems. Hardware keys (YubiKey) for admin accounts. TOTP minimum for all users. No exceptions for "convenience."
- [ ] **Permission boundaries** — Limit the maximum permissions any role can have, even if policies are attached. Prevents privilege escalation. AWS: permission boundaries. GCP: deny policies. Defense in depth for IAM.
- [ ] **Regular access reviews** — Quarterly review of who has access to what. Revoke unused access. Check for privilege creep (accumulating permissions over time without removal). Automate with Access Analyzer or custom scripts.
- [ ] **Multi-account strategy** — Separate AWS accounts (or GCP projects) per environment and workload type. Production isolated from dev. Security/audit in dedicated account. AWS Organizations / GCP folders for governance. Prevents blast radius of compromised credentials.

## 8. Monitoring & Alerting

- [ ] **Metrics collection** — Prometheus + Grafana, CloudWatch, Datadog, or New Relic. Infrastructure metrics (CPU, memory, disk, network) + application metrics (request rate, error rate, latency). Custom business metrics (signups, orders, revenue). The four golden signals: latency, traffic, errors, saturation.
- [ ] **Log aggregation** — Loki, ELK/OpenSearch, CloudWatch Logs, Datadog Logs. Centralized, searchable, retained per compliance. Structured JSON logs from applications. Log levels meaningful and consistent (ERROR = page someone, WARN = investigate soon, INFO = context for debugging).
- [ ] **Distributed tracing** — Jaeger, Tempo, X-Ray, Datadog APM. OpenTelemetry instrumentation (vendor-neutral). Trace requests across service boundaries. Correlate traces with logs and metrics via trace ID. Sampling strategy for high-traffic services.
- [ ] **Dashboards as code** — Grafana dashboards in git (JSON/Jsonnet/Grafonnet). Terraform-provisioned. Not hand-built in production UI that nobody can reproduce. Dashboard changes go through PR review. Standard template per service type.
- [ ] **Alert routing** — PagerDuty, OpsGenie, or Grafana OnCall. Severity-based routing: P1 (outage) → phone call + Slack + auto-incident. P2 (degraded) → Slack + email. P3 (needs attention) → ticket. Escalation policies: if not ack'd in 5 min → escalate.
- [ ] **SLO-based alerting** — Alert on error budget burn rate, not raw thresholds. "Burning 10x budget in 1h window" → page now. "Burning 2x budget in 6h window" → ticket. More signal, less noise than "CPU > 80% for 5 min." Define SLIs that reflect user experience.
- [ ] **On-call rotation** — Defined schedule, fair rotation, handoff process with context. On-call engineer has access to runbooks, architecture docs, escalation paths. Compensated (time-off or pay). Not the same person every night. Follow-the-sun for global teams.
- [ ] **Alert hygiene** — Every alert is actionable — if it fires and nobody needs to do anything, delete it or downgrade to dashboard. Alert fatigue kills response time. Review alerts quarterly: delete noisy ones, tune thresholds, add missing ones.
- [ ] **Synthetic monitoring** — External health checks from multiple regions (Pingdom, UptimeRobot, Checkly, CloudWatch Synthetics). Detect outages before users report them. Simulate critical user journeys. Alert within 60 seconds of failure.
- [ ] **Log retention & cost** — Define retention per log type: application logs 30 days hot / 1 year cold. Audit logs: 7 years (compliance). Avoid storing debug-level logs in production long-term — they're expensive and rarely useful after 7 days.

## 9. Disaster Recovery & Backup

- [ ] **RPO/RTO defined** — Recovery Point Objective: how much data loss is acceptable (5 min? 1 hour? 24 hours?). Recovery Time Objective: how fast must recovery be (minutes? hours?). Written down, stakeholder-agreed, budget-funded. Different tiers for different services.
- [ ] **Automated backups tested** — Backups exist AND have been restored successfully. Untested backups are Schrödinger's data — simultaneously there and not there. Test restore monthly. Measure actual restore time. Alert on backup job failures.
- [ ] **Cross-region replication** — Critical data replicated to another region. S3 cross-region replication, RDS cross-region read replica, or application-level sync. Survive a full region outage. Know the replication lag.
- [ ] **DR runbook** — Step-by-step recovery procedure. Who does what, in what order. DNS failover, database promotion, traffic rerouting. Accessible when primary region is down (stored in multiple locations, not only in primary region).
- [ ] **DR test quarterly** — Actually execute the DR plan in a realistic scenario. Measure real RTO. Fix gaps discovered. Document results and improvements. A plan that's never been tested is a fantasy, not a plan.
- [ ] **Failover automation** — DNS failover (Route53 health checks), database auto-failover (Multi-AZ), load balancer health checks route away from unhealthy targets. Manual steps minimized. Every manual step in recovery = more time + human error under pressure.
- [ ] **Data restore tested** — Restore a backup to a clean environment. Verify data integrity (row counts, checksums, application reads correctly). Measure restore time. Different from "backup exists" — proves end-to-end recovery works.
- [ ] **Backup isolation** — Backups stored in separate account or with immutable retention (Object Lock, Vault Lock). Ransomware that compromises production shouldn't reach backups. Air-gapped or write-once storage for critical data.
- [ ] **DNS-based failover** — Health-checked DNS records (Route53 failover routing, Cloud DNS health checks). If primary is unhealthy, DNS resolves to DR target within TTL. Low TTL on failover records (60s). Tested by deliberately failing primary.

## 10. Security & Compliance

- [ ] **Container image scanning** — Trivy, Snyk Container, ECR scanning, GCR vulnerability scanning. Scan in CI pipeline (block on critical/high CVEs). Scan in registry continuously (detect newly discovered CVEs in existing images). Base image updates automated via Renovate/Dependabot.
- [ ] **Dependency vulnerability scanning** — Dependabot, Renovate, Snyk, or native tools (`npm audit`, `govulncheck`, `pip audit`). Automated PRs for security patches. Block deployments with known critical vulnerabilities. Don't just scan — fix.
- [ ] **Network policies** — Kubernetes NetworkPolicies: default deny all ingress/egress, allow specific traffic explicitly. Not "all pods can talk to all pods." Calico, Cilium, or cloud-native implementation. Test that denied traffic is actually blocked.
- [ ] **WAF (Web Application Firewall)** — AWS WAF, Cloud Armor, Azure WAF, or Cloudflare. OWASP Top 10 managed rule set. Rate limiting at edge. Geo-blocking if applicable. Bot detection and mitigation. Custom rules for app-specific threats.
- [ ] **DDoS protection** — AWS Shield (Standard free, Advanced paid), Cloud Armor, Azure DDoS Protection, or Cloudflare. Always-on for production. Know your provider's SLA for attack mitigation time. Test that protection actually activates.
- [ ] **Compliance frameworks** — SOC 2, ISO 27001, HIPAA, PCI-DSS, GDPR — know which apply to your business. Evidence collection automated (AWS Config, GCP Security Command Center). Audit-ready year-round, not scrambling before auditor arrives.
- [ ] **Audit logging** — All administrative actions logged immutably. All data access logged for sensitive systems. Tamper-proof storage (S3 Object Lock, CloudWatch Logs with resource policy). Retained per compliance requirements (typically 1-7 years).
- [ ] **Penetration testing** — Annual minimum. After major architecture changes. External firm, not just internal vulnerability scans. Findings tracked to resolution with deadlines. Retest after fix to verify remediation.
- [ ] **Runtime security** — Falco, Sysdig, or cloud-native (GuardDuty, Security Command Center). Detect anomalous behavior: unexpected network connections, privilege escalation, file system modifications. Alert and respond in real-time.
- [ ] **Supply chain security** — Sign container images (Cosign/Notary). Verify signatures before deployment (admission controller). SBOM generation for every image. Pin base images by digest, not tag. Know what's in your containers.

## 11. Cost Management

- [ ] **Budget alerts** — Set per-account or per-project budgets. Alert at 50%, 80%, 100% of forecast. Catch runaway costs early (orphaned resources, crypto miners, misconfigured auto-scale). Separate alerts for anomaly detection (sudden spike vs gradual growth).
- [ ] **Resource tagging strategy** — Every resource tagged: `team`, `environment`, `service`, `cost-center`, `managed-by` (terraform/manual). Enforced via policy (AWS Config rules, Organization SCPs, GCP org policies). Untagged = non-compliant = flagged for review.
- [ ] **Unused resource cleanup** — Scheduled scans for: unattached EBS volumes, idle load balancers, stopped instances > 7 days, unused Elastic IPs, old snapshots, empty S3 buckets, orphaned ENIs. Automate cleanup or generate weekly report.
- [ ] **Reserved instances / savings plans** — Commit to 1-year or 3-year for stable baseline workloads (databases, core services). 30-60% savings vs on-demand. Review utilization quarterly. Don't over-commit — unused reservations are wasted money.
- [ ] **Spot instances for non-critical** — CI runners, dev/staging environments, batch processing, data pipelines, ML training. Spot fleet with instance diversity (multiple types + AZs). Graceful handling of interruptions. Karpenter (K8s) or Spot Fleet (AWS).
- [ ] **Cost allocation by team/project** — Tag-based cost breakdown visible to each team. Shared costs (networking, monitoring, platform) allocated fairly. Cost visibility drives cost-conscious engineering decisions. Publish monthly cost-per-team report.
- [ ] **Monthly cost review** — Review spend vs budget. Identify growth trends and anomalies. Act on right-size recommendations. Investigate unexpected increases. Finance + engineering together. Not a surprise at quarter-end.
- [ ] **Dev/staging cost controls** — Non-production environments scheduled (shut down nights/weekends saves 65%). Smaller instance types than production. Shorter data retention. Auto-cleanup of PR environments.
- [ ] **FinOps culture** — Engineers see cloud costs as part of their responsibility, not just finance's problem. Cost discussions in architecture reviews. "How much will this cost at 10x scale?" asked before building, not after the bill arrives.

## 12. Reliability & Scaling

- [ ] **Auto-scaling** — HPA (K8s, CPU/memory/custom metrics), KEDA (event-driven: queue depth, cron, external metrics), cloud auto-scale groups (target tracking policies). Scale on the metric that reflects actual load (requests per second, queue depth), not just CPU.
- [ ] **Load testing before launch** — k6, Locust, Gatling. Test at 2-3x expected peak traffic. Identify bottlenecks: connection pools, database queries, external API rate limits, DNS lookup latency. Load test the full stack, not just the app in isolation.
- [ ] **Chaos engineering** — Kill pods, fail AZs, inject latency, corrupt DNS, fill disks. Start in staging, graduate to production. Verify: auto-scaling triggers, failover works, alerts fire, runbooks are followed correctly. Tools: Litmus, Chaos Monkey, Gremlin, AWS FIS.
- [ ] **Graceful degradation** — Downstream failure ≠ total failure. Feature flags disable non-critical services. Circuit breakers prevent cascade. Return cached/stale/partial data over hard 500 errors. Users prefer "slightly stale" over "completely broken."
- [ ] **Circuit breakers at infra level** — Service mesh (Istio, Linkerd) circuit breaking configured per route. Or application-level per downstream (Resilience4j, gobreaker). Prevent one failing dependency from exhausting all connection pools and taking down everything.
- [ ] **Multi-AZ deployment** — Every production workload spans ≥ 2 availability zones (3 preferred). Database Multi-AZ with automatic failover. Load balancer cross-zone enabled. No single-AZ dependencies in the critical path. Test AZ failure recovery.
- [ ] **Health checks at every layer** — Load balancer → app health check (HTTP 200). K8s liveness (is process alive?) + readiness (can it serve?). Database connection validation in readiness. Upstream dependency checks. Unhealthy instance = no traffic routed.
- [ ] **Capacity planning** — Forecast growth based on business projections. Know your limits: max RPS per service, max DB connections, max storage. Plan for known events (product launches, marketing campaigns, seasonal peaks). Don't discover your ceiling in production.
- [ ] **Pod Disruption Budgets (K8s)** — PDB ensures minimum available pods during voluntary disruptions (node drain, cluster upgrade). `minAvailable: 50%` or `maxUnavailable: 1`. Without PDB, `kubectl drain` can take down your entire service.
- [ ] **Rate limiting at infrastructure level** — API Gateway rate limits, WAF rate rules, or ingress-level throttling. Protect backends from traffic spikes (intentional or malicious). Per-client limits, global limits, and burst allowances configured.
- [ ] **Immutable infrastructure** — Servers/containers are replaced, never patched in place. New version = new image = new deployment. No SSH to fix things — fix the image and redeploy. Ensures consistency and reproducibility across all instances.
- [ ] **Graceful shutdown** — Containers handle SIGTERM properly. Drain connections, finish in-flight requests, deregister from service discovery, then exit. K8s `terminationGracePeriodSeconds` exceeds app drain time. `preStop` hook for deregistration delay.
- [ ] **Blue-green for databases** — Schema changes backward-compatible (expand/contract pattern). Add column → deploy new code that uses it → remove old column. Never break the running version during migration. Zero-downtime DDL changes.

## 13. Documentation & Runbooks

- [ ] **Architecture diagrams** — Current-state architecture, not aspirational. Updated when infra changes (part of PR). Network topology, data flows, service dependencies. Version-controlled (Mermaid, Diagrams-as-code, draw.io in git, or Lucidchart with export).
- [ ] **Network topology documented** — VPC layout, subnet CIDRs, peering connections, VPN tunnels, DNS zones, NAT gateways, security group rules summary. Enough detail to troubleshoot connectivity issues at 3 AM without guessing.
- [ ] **Operational runbooks** — Step-by-step for common tasks: scale a service, failover database, rotate secrets, restore from backup, add a new service, onboard a new developer, upgrade Kubernetes. Tested by someone who didn't write them.
- [ ] **Incident response playbook** — Detection → triage → communication → mitigation → resolution → post-mortem. Roles defined (incident commander, communications lead, technical lead). Templates and Slack channels pre-created. Severity definitions agreed.
- [ ] **On-call handbook** — How to access systems (VPN, SSO, break-glass). Escalation paths with contact info. Known issues and workarounds. Where to find logs, metrics, traces. What to do first when paged. Context for each common alert.
- [ ] **Post-mortem process** — Blameless. Template: summary, impact, timeline, root cause, contributing factors, action items (with owners and deadlines). Published internally. Action items tracked in sprint backlog. Review completion in retro.
- [ ] **Change management** — Major infra changes documented before execution: what, why, risk assessment, rollback plan, communication plan, maintenance window. Not every PR — but VPC changes, database migrations, K8s upgrades, IAM policy changes.
- [ ] **Service catalog** — Every service documented: what it does, who owns it, where it runs, how to deploy, how to monitor, dependencies, SLA/SLO. New team member finds any service in < 5 minutes.
- [ ] **Decision log for infrastructure** — Why Kubernetes over ECS? Why this region? Why this database engine? Decisions documented with context, alternatives considered, and trade-offs accepted. Prevents re-litigating past decisions.
- [ ] **Dependency mapping** — Which services depend on what? What breaks if Redis goes down? If the payment gateway is slow? Service dependency graph maintained and used for impact analysis during incidents. Tools: Backstage, manual diagram, or auto-discovered from traces.

---

## Quick Sanity Check Before Production

> The non-negotiables. If any of these are "no," you're not production-ready.

- [ ] All infrastructure defined in code — no undocumented manual resources in production
- [ ] Remote state locked and encrypted — verified by attempting concurrent plan
- [ ] No long-lived credentials — service accounts use OIDC/workload identity, no static keys
- [ ] Secrets in secrets manager — `gitleaks` scan passes on full git history, no secrets anywhere in repo
- [ ] Multi-AZ for all production workloads — verified by simulating AZ failure in staging
- [ ] Automated backups enabled AND restore tested — within last 30 days, documented result
- [ ] Monitoring covers all layers — infra, app, business metrics — with actionable alerts that have fired in staging
- [ ] Auto-scaling tested under load — scales out within acceptable time, verified with k6/Locust
- [ ] DR plan documented AND tested — team knows what to do when a region goes down
- [ ] Network segmentation enforced — databases not reachable from public internet, verified by scan
- [ ] Container images scanned — no critical CVEs in production images, scanning runs in CI
- [ ] Cost alerts configured — budget overrun notified before end of month
- [ ] Runbooks exist for top 5 operational tasks — tested by someone other than the author
- [ ] On-call rotation defined — escalation path clear, handbook accessible, tested page delivery
- [ ] Rollback tested — can revert to previous version in < 5 minutes with zero data loss
- [ ] TLS everywhere — no unencrypted traffic in production, certificates auto-renewed
- [ ] MFA enforced — all human access to cloud console and production systems requires MFA

---

## Tool Quick Reference

| Category | Tools | When to Use |
|----------|-------|-------------|
| **IaC** | Terraform, OpenTofu | Multi-cloud, mature ecosystem, HCL, largest community |
| | Pulumi | Real programming languages (TS, Python, Go), complex logic in infra |
| | AWS CDK / CDKTF | AWS-native (CDK) or Terraform backend with TypeScript/Python (CDKTF) |
| | Terragrunt | DRY Terraform wrapper, multi-env management, dependency orchestration |
| **CI/CD** | GitHub Actions | GitHub-native, great marketplace, easy start, OIDC built-in |
| | GitLab CI | GitLab-native, mature, built-in registry + security scanning |
| | ArgoCD / Flux | GitOps for Kubernetes, declarative, drift detection, auto-sync |
| | Jenkins | Legacy/enterprise, max flexibility, high maintenance cost |
| | Argo Rollouts / Flagger | Progressive delivery, canary, blue-green with auto-promotion |
| **Monitoring** | Prometheus + Grafana | Open-source, K8s-native, flexible, PromQL, self-hosted |
| | Datadog | SaaS all-in-one (metrics, logs, traces, APM, RUM), expensive at scale |
| | CloudWatch / Cloud Monitoring | Cloud-native, zero setup, sufficient for smaller teams |
| | OpenTelemetry | Vendor-neutral instrumentation, switch backends without code changes |
| **Logging** | Loki + Grafana | Lightweight, label-indexed, pairs with Prometheus, cost-effective |
| | ELK / OpenSearch | Full-text search, powerful queries, resource-heavy to operate |
| | Datadog Logs | SaaS, correlates with metrics/traces, expensive at volume |
| **Tracing** | Jaeger / Tempo | Open-source, OpenTelemetry-native, Tempo is cheaper (object storage) |
| | Datadog APM / X-Ray | Managed, lower ops burden, cloud-integrated |
| **Secrets** | HashiCorp Vault | Multi-cloud, dynamic secrets, PKI, transit encryption, most features |
| | AWS Secrets Manager | AWS-native, automatic rotation for RDS, simple, pay-per-secret |
| | GCP Secret Manager | GCP-native, IAM-integrated, versioned, simple |
| | External Secrets Operator | K8s: sync any secrets manager → K8s Secrets automatically |
| **Container Orchestration** | Kubernetes (EKS/GKE/AKS) | Complex microservices, team has K8s experience, need flexibility |
| | ECS / Cloud Run | Simpler container ops, fewer features, faster time-to-production |
| | Nomad | Multi-workload (containers + VMs + batch), lighter than K8s |
| **Service Mesh** | Istio | Feature-rich (traffic, security, observability), complex, resource-heavy |
| | Linkerd | Lightweight, simple, Rust data plane, smaller deployments |
| | Cilium | eBPF-based networking + observability + mesh unified, high-performance |
| **Cost** | Infracost | Terraform cost estimation in PRs, shift-left on cost |
| | Kubecost / OpenCost | K8s-specific cost allocation per namespace/workload/label |
| | AWS Cost Explorer / GCP Billing | Native cost analysis and recommendations, no extra tooling |
| **Policy/Security** | OPA / Gatekeeper | Policy-as-code for K8s admission control |
| | Checkov / tfsec | Static analysis for Terraform/CloudFormation security misconfigs |
| | Trivy | Container + IaC + filesystem vulnerability scanning, fast, free |
| | Falco | Runtime security monitoring for containers and K8s |
