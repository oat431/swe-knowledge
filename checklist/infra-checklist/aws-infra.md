# AWS Infrastructure Checklist

> Production-ready AWS infrastructure checklist based on the Well-Architected Framework 6 pillars:
> Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, and Sustainability.
> From account setup to disaster recovery — everything for running reliable workloads on AWS.
> Covers: multi-account strategy, VPC networking, compute (ECS/EKS/Lambda), RDS/DynamoDB/S3,
> CI/CD pipelines, secrets management, IAM, monitoring, security services, DR, and cost control.
> Last updated: 2026-07-17

---

## 1. Account & Organization

- [ ] **Multi-account strategy** — Separate accounts per environment and function: workload-dev, workload-staging, workload-prod, security, shared-services, log-archive. Blast radius containment. One account compromised ≠ everything compromised. (Pillar: Security, Operational Excellence)
- [ ] **AWS Control Tower** — Automated landing zone with guardrails. Account Factory for standardized new accounts. Preventive + detective guardrails out of the box. Customizations for Control Tower (CfCT) for org-specific rules.
- [ ] **Service Control Policies (SCPs)** — Org-wide restrictions: deny leaving organization, deny disabling CloudTrail, deny public S3, deny unused regions, deny root account usage. SCPs are your safety net — they override IAM allows.
- [ ] **Billing alerts & budgets** — AWS Budgets per account and per service. Alert at 80% and 100% of monthly target. Anomaly detection enabled. Budget actions (auto-stop) for non-prod. No bill surprises.
- [ ] **Root account lockdown** — Hardware MFA (YubiKey), no access keys, no daily use. Break-glass only. Root email is a distribution list, not a person. Store MFA device securely (safe/vault). (Pillar: Security)
- [ ] **IAM Identity Center (SSO)** — Single sign-on for all human access. Federate with your IdP (Okta, Google Workspace, Azure AD). No IAM users with passwords. No console-created credentials. MFA enforced.
- [ ] **Naming & tagging conventions** — Enforced tags: `Environment`, `Team`, `Service`, `CostCenter`. Account naming: `company-env-purpose` (e.g., `acme-prod-workload`). Tag policies in Organizations. Deny untagged resource creation via SCP.
- [ ] **Account contacts** — Security, billing, and operations contacts configured per account. AWS uses these for security notifications. Not having them = missing critical alerts.

## 2. Networking (VPC)

- [ ] **VPC CIDR planning** — Non-overlapping CIDRs across all VPCs and accounts. Plan for VPC peering and Transit Gateway. Use /16 per VPC, /24 per subnet. Document the IP plan in a shared wiki/sheet.
- [ ] **Multi-AZ subnet tiers** — Public subnets (ALB, NAT Gateway), private subnets (compute, containers), isolated subnets (databases, no internet route). Minimum 2 AZs, prefer 3. (Pillar: Reliability)
- [ ] **NAT Gateway** — One per AZ for high availability ($0.045/hr + data processing). Private subnets route outbound internet through NAT. Budget alert on NAT data charges — they surprise people.
- [ ] **Security Groups vs NACLs** — Security Groups: stateful, instance-level, default-deny inbound, allow-all outbound. NACLs: stateless, subnet-level, use sparingly for broad blocks. SG is your primary firewall tool.
- [ ] **VPC Flow Logs** — Enabled on all VPCs (at minimum reject logs). Ship to CloudWatch Logs or S3 in log-archive account. Detect anomalies, debug connectivity, support incident forensics. (Pillar: Security)
- [ ] **Transit Gateway** — Multi-VPC and multi-account connectivity hub. Replace N×N VPC peering. Centralized routing tables. Share via Resource Access Manager (RAM). Segment with route domains.
- [ ] **VPC Endpoints / PrivateLink** — Gateway endpoints for S3 and DynamoDB (free). Interface endpoints for ECR, Secrets Manager, KMS, CloudWatch. No internet transit for AWS API calls. Reduces NAT costs. (Pillar: Security, Cost Optimization)
- [ ] **Route 53** — Hosted zones for public and private DNS. Health checks on endpoints. Failover routing for DR. Latency-based routing for multi-region. Alias records for AWS resources (free queries).
- [ ] **DNS resolution** — Enable DNS hostnames and DNS resolution in VPC. Private hosted zones associated with VPCs. Route 53 Resolver for hybrid DNS (on-prem ↔ AWS).

## 3. Compute

- [ ] **Compute platform choice** — ECS Fargate (serverless containers, simplest ops), EKS (full Kubernetes, when you need the ecosystem), EC2 (VMs, when you need kernel access or GPU). Default: Fargate unless you have a reason not to. (Pillar: Operational Excellence)
- [ ] **Task/pod right-sizing** — Size based on actual metrics, not guesses. Start small, monitor with Container Insights, adjust. CPU and memory are independent on Fargate. Use Compute Optimizer for EC2.
- [ ] **Auto-scaling configured** — Target tracking (CPU 60-70%, memory 70-80%) or custom metrics (queue depth, request count, concurrent connections). Scale on the metric that predicts saturation. Scale-in cooldown to prevent flapping. (Pillar: Reliability, Performance Efficiency)
- [ ] **Spot / Fargate Spot** — Use for non-critical workloads: batch processing, CI/CD runners, dev/staging environments. 50-70% savings. Handle interruptions gracefully (2-min warning). Diversify instance families.
- [ ] **Graviton (ARM) instances** — 20-40% better price-performance than x86. ECS, EKS, Lambda, RDS all support Graviton. Multi-arch Docker images (buildx). Default choice unless software requires x86. (Pillar: Cost Optimization, Sustainability)
- [ ] **Lambda for event-driven** — Short-lived (<15 min), bursty, event-triggered workloads. API Gateway + Lambda for low-traffic APIs. Not for long-running or high-throughput. Watch cold starts. Provisioned concurrency for latency-sensitive paths.
- [ ] **SSM Session Manager** — No SSH keys, no bastion hosts, no port 22 open. Audit trail via CloudTrail. IAM-controlled access. Use for all instance/container access. ECS Exec for container debugging. (Pillar: Security)
- [ ] **Health checks** — ALB health checks with appropriate thresholds (interval, timeout, healthy/unhealthy count). ECS health check grace period for slow-starting apps. Separate liveness vs readiness when on EKS.

## 4. Storage & Databases

- [ ] **RDS Multi-AZ** — Managed Postgres or MySQL. Multi-AZ for production (synchronous standby, automatic failover <60s). Read replicas for read-heavy workloads. Never self-manage a database on EC2. (Pillar: Reliability)
- [ ] **Automated backups & PITR** — Enabled by default. Set retention to 7-35 days (cost increases with retention). Test point-in-time restore quarterly. A backup you've never restored from is an assumption, not a backup.
- [ ] **RDS Proxy** — Connection pooling for Lambda and bursty workloads. Prevents connection exhaustion (Postgres max_connections). IAM auth supported. Essential for serverless architectures.
- [ ] **DynamoDB** — Key-value and document at scale. Single-digit millisecond latency. On-demand (pay-per-request) for unpredictable traffic, provisioned + auto-scaling for steady. Global tables for multi-region active-active.
- [ ] **S3 hardened** — Account-level Block Public Access enabled. Bucket policies (least privilege). SSE-S3 or SSE-KMS encryption. Versioning for critical buckets. Lifecycle rules: IA → Glacier → Deep Archive. Object Lock for compliance. (Pillar: Security, Cost Optimization)
- [ ] **ElastiCache (Redis/Valkey)** — Caching (reduce DB load, cut latency), session storage, rate limiting, pub/sub. Multi-AZ with automatic failover. Cluster mode for horizontal scaling. Graviton nodes for cost savings.
- [ ] **Aurora Serverless v2** — Scales compute in fine-grained increments (0.5 ACU). Scales to near-zero in dev/staging. Production-ready with Multi-AZ. Best for variable/unpredictable workloads. Compatible with standard Postgres/MySQL.
- [ ] **Database parameter tuning** — Don't use defaults blindly. Tune `shared_buffers`, `work_mem` for Postgres. Enable Performance Insights (free tier available) to identify slow queries. Enhanced Monitoring for OS-level metrics.

## 5. CI/CD & Deployment

- [ ] **ECR with lifecycle policies** — Private container registry per account/region. Lifecycle rules: keep last N tagged images, expire untagged after 7 days. Immutable tags for production images. Cross-account pull via resource policy. (Pillar: Operational Excellence)
- [ ] **Pipeline-as-code** — GitHub Actions, CodePipeline, or GitLab CI defined in the repo. Never console-configured pipelines. Reproducible, reviewable, version-controlled. Code review required for pipeline changes.
- [ ] **Zero-downtime deployments** — ECS rolling update (minimum healthy 100%, maximum 200%) or blue/green via CodeDeploy with traffic shifting. Health check grace period configured. Automatic rollback on failure (alarm-triggered).
- [ ] **Separate infra and app pipelines** — Terraform/CDK changes in their own pipeline with plan → approve → apply. App deployments are independent. Infra changes don't block feature deployments. Drift detection scheduled.
- [ ] **Image tag = git SHA** — Every image traceable to a commit. Never `:latest` in production. Enables instant rollback by redeploying previous SHA. Image metadata (labels) with build info. (Pillar: Operational Excellence)
- [ ] **Environment promotion** — dev → staging → prod. Same artifact, different config (env vars, secrets). Staging mirrors prod (same instance sizes, same scaling policies). No "works in staging" surprises.
- [ ] **Rollback strategy** — Automated rollback on CloudWatch alarm trigger (5xx spike, error rate). Manual rollback = redeploy previous image SHA. Database rollbacks planned (backward-compatible migrations only). Know your MTTR.

## 6. Secrets & Configuration

- [ ] **Secrets Manager** — DB passwords, API keys, third-party tokens. Auto-rotation enabled (Lambda rotation function). Versioned. Audit access via CloudTrail. Cost: $0.40/secret/month — worth it. (Pillar: Security)
- [ ] **SSM Parameter Store** — Non-secret config: feature flags, endpoint URLs, tunables. Free tier (standard parameters, up to 10,000). Hierarchical paths: `/app/prod/db-host`. Change notifications via EventBridge.
- [ ] **Never in code or baked images** — Secrets never in source code, Dockerfiles, or task definition environment variables. Always fetched at runtime. Scan repos with git-secrets, truffleHog, or GitHub secret scanning.
- [ ] **Runtime secret injection** — ECS: `valueFrom` in container definition referencing Secrets Manager ARN. EKS: External Secrets Operator syncing to K8s secrets. Lambda: environment variable resolved from Secrets Manager.
- [ ] **KMS (Key Management Service)** — Customer-managed keys (CMKs) for sensitive data encryption. Key policies control who can use/manage. Automatic annual rotation. Cross-account key sharing via grants. Separate keys per environment.
- [ ] **Secret access auditing** — CloudTrail logs every Secrets Manager and KMS API call. Alert on unexpected access patterns (new principal, unusual time, bulk retrieval). Integrate with GuardDuty for anomaly detection.

## 7. Identity & Access Management (IAM)

- [ ] **Least privilege** — Start with zero permissions, add only what's demonstrated as needed. Use CloudTrail + IAM Access Analyzer to identify actually-used permissions. Remove unused permissions quarterly. (Pillar: Security)
- [ ] **Roles, not keys** — EC2 instance profiles, ECS task roles, Lambda execution roles. Services assume roles via STS. Never create access keys for service-to-service communication. Credential exposure = immediate incident.
- [ ] **IAM Identity Center for humans** — Federate with corporate IdP. Permission sets map to IAM roles per account. Short-lived credentials (1-12 hour sessions). No IAM users with long-lived passwords or access keys.
- [ ] **No long-lived access keys** — If unavoidable (legacy integration): rotate every 90 days, scope to minimum permissions, use external ID for cross-account. Prefer IAM Roles Anywhere for on-prem workloads needing AWS access.
- [ ] **Permission boundaries** — Cap maximum permissions for delegated administrators. Teams can create roles but can't exceed the boundary. Prevents privilege escalation. Required for any self-service IAM.
- [ ] **IAM Access Analyzer** — Detect external access to your resources (S3, KMS, SQS, Lambda). Find unused permissions to right-size policies. Policy validation before deployment. Run continuously, review findings weekly. (Pillar: Security)
- [ ] **SCPs for guardrails** — Organization-level deny: prevent disabling CloudTrail, prevent public S3, prevent leaving org, restrict to approved regions, prevent root usage. SCPs override IAM allows — they're your last line of defense.
- [ ] **Separate task execution role vs task role (ECS)** — Execution role: pulls images from ECR, writes logs to CloudWatch. Task role: your app's permissions (access S3, DynamoDB, etc.). Never combine them. Principle of least privilege at every layer.

## 8. Monitoring & Observability

- [ ] **CloudWatch Metrics & Dashboards** — Standard metrics (CPU, memory, request count) + custom business metrics (orders/min, signups). One dashboard per service. Automated dashboards via CDK/Terraform, not manually clicked. (Pillar: Operational Excellence)
- [ ] **CloudWatch Logs** — Structured JSON logging from all services. Retention policies set (14-30 days hot, archive to S3 for long-term). Metric filters for error patterns. Log Insights for ad-hoc queries. Never infinite retention
- [ ] **CloudWatch Alarms** — Error rate > threshold, latency p99 > SLA, 5xx count spike, unhealthy host count > 0, queue depth growing. Alarm → SNS → PagerDuty/Slack/OpsGenie. Composite alarms to reduce noise.
- [ ] **Distributed tracing** — X-Ray or OpenTelemetry (ADOT Collector) for request tracing across microservices. Identify bottlenecks, map service dependencies, debug latency. Service map visualization. Sample rate appropriate for volume.
- [ ] **CloudTrail** — Enabled in all regions, all accounts. Management events always. Data events for S3/Lambda on sensitive resources. Ship to central log-archive account. S3 Object Lock (immutable). (Pillar: Security)
- [ ] **AWS Health + EventBridge** — Subscribe to AWS service health events relevant to your stack. Automate responses (trigger failover, notify team). Know about AWS issues before your customers report them.
- [ ] **Container Insights** — Per-task CPU, memory, network, disk metrics for ECS/EKS. Performance log events with detailed container data. Application-level metrics without custom instrumentation. Worth the cost.
- [ ] **SLIs/SLOs defined** — Service Level Indicators (latency p99, availability, error rate) measured. Service Level Objectives set (99.9% availability, p99 < 200ms). Burn rate alerts when error budget consumption accelerates.

## 9. Security

- [ ] **GuardDuty** — Threat detection enabled in all accounts, all regions. Detects compromised instances, credential exfiltration, crypto mining, S3 anomalies. Alert on all HIGH/CRITICAL findings immediately. (Pillar: Security)
- [ ] **Security Hub** — Aggregates findings from GuardDuty, Inspector, IAM Access Analyzer, AWS Config. CIS Benchmark and AWS Foundational Security Best Practices enabled. Central security posture view. Delegate to security account.
- [ ] **AWS Config** — Track all resource configuration changes (who changed what, when). Managed rules: S3 not public, EBS encrypted, required tags present. Auto-remediation via SSM for common drift. (Pillar: Security)
- [ ] **WAF on ALB/CloudFront** — Rate limiting (protect against DDoS and credential stuffing). AWS Managed Rules (Core, Known Bad Inputs, SQL injection, Linux/POSIX). Geo-blocking if needed. Log all requests to S3/CloudWatch.
- [ ] **Centralized security logs** — VPC Flow Logs + CloudTrail + GuardDuty findings + Config changes → dedicated security/log-archive account. Immutable storage (Object Lock). SIEM integration for correlation.
- [ ] **ECR image scanning** — Scan on push (Inspector enhanced scanning for OS + language packages). Pipeline gate: block deployment of images with CRITICAL/HIGH CVEs. Accept risk explicitly and document exceptions.
- [ ] **Block Public Access (S3)** — Account-level setting enabled on ALL accounts. No exceptions without security review, documented justification, and time-bound approval. Periodic re-validation. (Pillar: Security)
- [ ] **TLS everywhere** — TLS 1.2+ minimum (prefer 1.3). ACM for free managed certificates with auto-renewal. End-to-end encryption: client → ALB → target. No plaintext in transit, even internal. Security policies on ALB/CloudFront.
- [ ] **Inspector for vulnerability management** — Continuous scanning of EC2, Lambda, and ECR for software vulnerabilities. Prioritize by exploitability (EPSS score). Feed into Security Hub. Patch within SLA (Critical: 48h, High: 7d).

## 10. Disaster Recovery & Reliability

- [ ] **Multi-AZ everything** — RDS Multi-AZ, ALB across AZs, ECS tasks spread across AZs (spread placement). Single-AZ = single point of failure. No exceptions for production workloads. (Pillar: Reliability)
- [ ] **RPO/RTO defined** — Document per service. Critical DB: RPO < 1 hour, RTO < 30 min. Non-critical: RPO < 24 hours. Design, budget, and architecture decisions flow from these targets.
- [ ] **Cross-region backup** — S3 Cross-Region Replication for critical data. RDS cross-region read replica (or automated cross-region backups) for fast regional failover. Critical workloads: pilot light or warm standby in secondary region.
- [ ] **Route 53 failover** — Health checks on primary region endpoints. Failover routing to secondary region. TTL low enough for fast failover (60s). Test failover quarterly. Document the runbook for manual failover.
- [ ] **Chaos engineering** — AWS Fault Injection Service (FIS): terminate tasks, stress CPU, inject latency, fail AZ. Verify auto-recovery and alarm triggers. Run during business hours with team watching. Build confidence iteratively. (Pillar: Reliability)
- [ ] **Test restores quarterly** — RDS point-in-time restore to a test instance. S3 versioned object recovery. Full DR failover drill. If you haven't tested it, assume it doesn't work. Document results.
- [ ] **ECS service auto-recovery** — Desired count maintained automatically. Health check failures trigger task replacement. Deployment circuit breaker enabled (stop deploying broken images after N failures). Minimum healthy percent = 100%.
- [ ] **Dependency failure isolation** — Circuit breakers in application code (or App Mesh). Timeouts on all external calls. Retry with exponential backoff + jitter. Graceful degradation when dependencies are down.

## 11. Cost Optimization

- [ ] **AWS Budgets** — Alert at 80% and 100% of monthly target. Per-account and per-service breakdowns. Anomaly detection for unexpected spikes. Budget actions to auto-stop non-prod resources. Zero-spend budgets for sandbox accounts. (Pillar: Cost Optimization)
- [ ] **Cost Explorer weekly review** — Know your top 5 cost drivers. Track month-over-month trends. Identify spikes early. Assign cost ownership to teams via tags. Enable hourly granularity for detailed analysis.
- [ ] **Savings Plans / Reserved Instances** — Compute Savings Plans for steady-state Fargate/Lambda/EC2 (1-year: ~30% savings, 3-year: ~60%). Commit only to what's consistently used (check 30-day minimum usage pattern).
- [ ] **Spot everywhere possible** — Fargate Spot for dev/staging, CI/CD runners, batch workers. EC2 Spot for stateless workloads. Diversify instance types and AZs. Handle interruptions gracefully. Never Spot for databases.
- [ ] **Graviton by default** — ARM instances: 20-40% cheaper, same or better performance. Supported by ECS, EKS, Lambda, RDS, ElastiCache. Multi-arch builds in CI. Switch unless x86 is specifically required. (Pillar: Sustainability)
- [ ] **Right-sizing** — AWS Compute Optimizer recommendations reviewed monthly. Downsize over-provisioned instances/tasks. Right-size RDS instance class. Lambda Power Tuning tool for memory optimization. (Pillar: Performance Efficiency)
- [ ] **S3 lifecycle management** — Intelligent-Tiering for unknown access patterns. Standard → IA (30d) → Glacier Instant (90d) → Glacier Flexible (180d) → Deep Archive (365d) for known patterns. Delete incomplete multipart uploads.
- [ ] **Clean up waste** — Unused EBS volumes, orphaned snapshots, idle load balancers, unattached Elastic IPs, empty ECR repos, stopped EC2 instances. Automate detection with Config rules or Trusted Advisor. Schedule monthly cleanup.
- [ ] **Tagging enforced** — `Environment`, `Team`, `Service`, `CostCenter` required on all resources. Tag policies in Organizations. SCPs deny resource creation without required tags. Cost allocation tags activated in billing.
- [ ] **Data transfer costs** — Understand inter-AZ ($0.01/GB), inter-region, and internet egress pricing. Use VPC endpoints to avoid NAT charges. CloudFront for egress reduction. S3 Transfer Acceleration only when needed.

## 12. Compliance & Governance

- [ ] **AWS Config compliance rules** — Managed rules: encryption enabled, public access blocked, required tags present, approved instance types, security group restrictions. Custom rules for org-specific requirements. Conformance packs for standards. (Pillar: Security)
- [ ] **Immutable audit trail** — CloudTrail logs shipped to log-archive account. S3 Object Lock (compliance mode, not governance mode). 90+ day online retention, 1+ year archived. Separate account = compromised workload can't delete audit trail.
- [ ] **Data residency** — Know exactly which region(s) your data lives in. GDPR: EU data stays in EU regions. PDPA (Thailand): understand cross-border transfer rules. S3 bucket region ≠ CloudFront edge cache location. Document data flow maps.
- [ ] **AWS Backup** — Centralized backup policies across services (RDS, EBS, DynamoDB, EFS, S3). Lifecycle rules (warm → cold). Cross-account backup vault in security account for ransomware protection. Backup compliance reports.
- [ ] **Incident response playbook** — Documented: who does what, escalation path, communication plan, containment steps. Contact list updated quarterly. Tested annually via tabletop exercise or game day. Post-incident review (blameless).
- [ ] **Change management** — All production changes through pipeline (no console clicks). Change records linked to tickets/PRs. Approval gates for critical infrastructure. Drift detection alerts when config doesn't match IaC.

---

## Well-Architected Framework Pillars (Summary)

| Pillar | Core Question | Key AWS Tools |
|--------|--------------|---------------|
| **Operational Excellence** | How do you run and monitor systems? | CloudWatch, X-Ray, Systems Manager, Config |
| **Security** | How do you protect information and systems? | IAM, GuardDuty, Security Hub, KMS, WAF |
| **Reliability** | How do you recover from failure? | Multi-AZ, Auto Scaling, Route 53, Backup |
| **Performance Efficiency** | How do you use resources efficiently? | Graviton, Auto Scaling, CloudFront, ElastiCache |
| **Cost Optimization** | How do you avoid unnecessary costs? | Cost Explorer, Budgets, Savings Plans, Spot |
| **Sustainability** | How do you minimize environmental impact? | Graviton, right-sizing, serverless, managed services |

---

## Quick Sanity Check Before Production

> These 15 items are non-negotiable for any production AWS workload. If any are unchecked, you have a gap.

- [ ] Multi-AZ enabled for all stateful services (RDS, ElastiCache, ECS tasks spread across AZs)
- [ ] No IAM access keys for services — everything uses roles. Humans use SSO (Identity Center)
- [ ] GuardDuty + CloudTrail enabled in ALL accounts and ALL regions (not just your primary)
- [ ] S3 Block Public Access enabled at the account level (all accounts, no exceptions)
- [ ] Secrets in Secrets Manager with rotation — not in code, not in env vars, not in SSM plaintext
- [ ] Auto-scaling configured for compute (ECS services, Lambda concurrency, DynamoDB capacity)
- [ ] Backups automated AND restore tested within the last quarter (can you actually recover?)
- [ ] CloudWatch Alarms firing to an on-call channel (PagerDuty/Slack, not just email)
- [ ] VPC endpoints for S3 and ECR — no public internet transit for internal AWS API traffic
- [ ] Image tag = git SHA — never `:latest` in production. Every container traceable to a commit
- [ ] WAF on every internet-facing ALB/CloudFront with rate limiting and managed rule sets
- [ ] Budgets set with alerts at 80% and 100% of monthly target per account
- [ ] TLS 1.2+ everywhere — ACM certificates auto-renewing, no expired certs, no plaintext
- [ ] Container images scanned for CVEs before deployment — pipeline blocks CRITICAL findings
- [ ] Zero-downtime deployment strategy configured with automatic rollback on alarm trigger

---

## AWS Service Quick Reference

### Compute

| Use Case | Recommended Service |
|----------|-------------------|
| Containers (simplest ops) | **ECS Fargate** |
| Full Kubernetes | **EKS** (Fargate or managed node groups) |
| Event-driven, short-lived | **Lambda** |
| VMs with full control | **EC2** (Graviton/ARM preferred) |
| Batch processing | **AWS Batch** on Fargate Spot |
| GPU / ML training | **EC2** (P/G instances) or **SageMaker** |

### Database

| Use Case | Recommended Service |
|----------|-------------------|
| Relational (Postgres/MySQL) | **RDS** or **Aurora** |
| Variable relational workload | **Aurora Serverless v2** |
| Key-value / document at scale | **DynamoDB** |
| Caching / sessions | **ElastiCache** (Redis/Valkey) |
| Connection pooling (Lambda) | **RDS Proxy** |
| Full-text search | **OpenSearch Service** |
| Time-series data | **Timestream** |

### Storage

| Use Case | Recommended Service |
|----------|-------------------|
| Object storage | **S3** (+ lifecycle rules) |
| Shared file system (Linux) | **EFS** |
| Shared file system (Windows) | **FSx for Windows** |
| Block storage for EC2 | **EBS** (gp3 default) |
| Archival (infrequent access) | **S3 Glacier** / Deep Archive |
| Backup management | **AWS Backup** |

### Networking

| Use Case | Recommended Service |
|----------|-------------------|
| DNS + health checks | **Route 53** |
| Load balancing (HTTP/HTTPS) | **ALB** (Application Load Balancer) |
| Load balancing (TCP/UDP) | **NLB** (Network Load Balancer) |
| CDN + edge caching | **CloudFront** |
| Multi-VPC connectivity | **Transit Gateway** |
| Private AWS API access | **VPC Endpoints / PrivateLink** |
| DDoS protection | **Shield** (Standard free, Advanced paid) |

### Security

| Use Case | Recommended Service |
|----------|-------------------|
| Threat detection | **GuardDuty** |
| Security posture | **Security Hub** |
| Vulnerability scanning | **Inspector** |
| Web application firewall | **WAF** |
| Secrets storage | **Secrets Manager** |
| Encryption keys | **KMS** |
| TLS certificates | **ACM** (Certificate Manager) |
| SSO for humans | **IAM Identity Center** |

### Monitoring

| Use Case | Recommended Service |
|----------|-------------------|
| Metrics + dashboards + alarms | **CloudWatch** |
| Distributed tracing | **X-Ray** / OpenTelemetry (ADOT) |
| API audit trail | **CloudTrail** |
| Resource compliance | **AWS Config** |
| Container metrics | **Container Insights** |
| Cost analysis | **Cost Explorer** + **Budgets** |

### CI/CD

| Use Case | Recommended Service |
|----------|-------------------|
| Container registry | **ECR** |
| CI/CD pipelines | **CodePipeline** / GitHub Actions |
| Blue/green deployments | **CodeDeploy** |
| Infrastructure as code | **CDK** / Terraform / CloudFormation |
| Artifact storage | **S3** + **ECR** |
| Feature flags | **AppConfig** (via SSM) |

### Messaging & Integration

| Use Case | Recommended Service |
|----------|-------------------|
| Message queue (decoupling) | **SQS** (Standard or FIFO) |
| Pub/sub (fan-out) | **SNS** |
| Event bus (event-driven) | **EventBridge** |
| Workflow orchestration | **Step Functions** |
| API management | **API Gateway** (REST or HTTP API) |
| Streaming (high-throughput) | **Kinesis Data Streams** or **MSK** |

---

> **Guiding principle:** Managed services over self-managed. Serverless over servers.
> ARM (Graviton) over x86. Multi-AZ over single-AZ. Automation over manual.
> Least privilege over convenience. Monitoring over hoping.
>
> Start with what you need. Add complexity only when the simplest option fails.
> ECS Fargate + RDS + S3 covers 80% of workloads. Add services when you outgrow the basics.
