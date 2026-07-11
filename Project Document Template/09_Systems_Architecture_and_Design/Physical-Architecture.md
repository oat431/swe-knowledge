---
document_type: Physical Architecture
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
architect: "[Solution Architect]"
classification: "Internal / Confidential"
tags: [physical-architecture, deployment, infrastructure, sebok, iso-42010]
standard_ref:
  - SEBoK v2 — System Architecture & Design (ISO/IEC/IEEE 15288 §6.4.4)
  - ISO/IEC/IEEE 42010 — Architecture Description
---

# Physical Architecture

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document defines the physical architecture — how logical components are deployed on actual infrastructure, including hardware, networking, and hosting.

## 2. Deployment Architecture

```mermaid
flowchart TB
    subgraph Internet["Internet"]
        CUST([Customer])
        CDN[CDN]
    end

    subgraph Cloud["Cloud Platform — Region: ap-southeast-1"]
        subgraph Public["Public Subnet"]
            LB[Load<br>Balancer]
        end

        subgraph App["Application Subnet"]
            WEB1[Web Server 1]
            WEB2[Web Server 2]
            API1[API Server 1]
            API2[API Server 2]
            WORKER1[Worker 1]
            WORKER2[Worker 2]
        end

        subgraph Data["Data Subnet"]
            DB_PRIMARY[(DB Primary)]
            DB_REPLICA[(DB Replica)]
            CACHE[(Redis<br>Cache)]
            DW[(Data<br>Warehouse)]
        end

        subgraph Storage["Storage"]
            S3[Object<br>Storage]
        end
    end

    subgraph External["External"]
        ERP[(ERP System)]
        PAY[Payment<br>Gateway]
        EMAIL[Email<br>Service]
    end

    CUST --> CDN
    CDN --> LB
    LB --> WEB1
    LB --> WEB2
    WEB1 --> API1
    WEB2 --> API2
    API1 --> DB_PRIMARY
    API2 --> DB_PRIMARY
    DB_PRIMARY --> DB_REPLICA
    API1 --> CACHE
    API2 --> CACHE
    WORKER1 --> DB_PRIMARY
    WORKER2 --> DB_PRIMARY
    API1 --> S3
    API1 --> ERP
    API1 --> PAY
    WORKER1 --> EMAIL
    API1 --> DW

    style Internet fill:#e3f2fd,color:#000
    style Cloud fill:#e8f5e9,color:#000
    style External fill:#fff3e0,color:#000
```

## 3. Infrastructure Components

| Component | Technology | Specification | Quantity | Environment | Cost/Month |
|-----------|-----------|--------------|----------|-------------|-----------|
| [Load Balancer] | [AWS ALB / Azure LB] | [Managed, auto-scaling] | [1] | [All] | $[X] |
| [Web Servers] | [Container — Node.js] | [2 vCPU, 4GB RAM] | [2] | [All] | $[X] |
| [API Servers] | [Container — Node.js] | [4 vCPU, 8GB RAM] | [2] | [All] | $[X] |
| [Workers] | [Container — Node.js] | [2 vCPU, 4GB RAM] | [2] | [All] | $[X] |
| [Database] | [PostgreSQL — managed] | [4 vCPU, 16GB RAM, 500GB] | [1 primary + 1 replica] | [All] | $[X] |
| [Cache] | [Redis — managed] | [2 vCPU, 4GB RAM] | [1] | [All] | $[X] |
| [Data Warehouse] | [Snowflake / BigQuery] | [On-demand] | [1] | [Prod] | $[X] |
| [Object Storage] | [S3 / Blob Storage] | [Standard, 100GB] | [1] | [All] | $[X] |
| [CDN] | [CloudFront / Azure CDN] | [Global edge] | [1] | [Prod] | $[X] |
| [Container Registry] | [ECR / ACR] | [Managed] | [1] | [All] | $[X] |

## 4. Network Architecture

| Zone | Subnet | Components | Security |
|------|--------|-----------|---------|
| [Public] | [10.0.1.0/24] | [Load Balancer] | [Internet-facing, WAF] |
| [Application] | [10.0.2.0/24] | [Web, API, Workers] | [Internal only, LB ingress] |
| [Data] | [10.0.3.0/24] | [DB, Cache, DW] | [App subnet only] |
| [Management] | [10.0.4.0/24] | [Bastion, monitoring] | [VPN only] |

## 5. Environment Architecture

| Environment | Purpose | Infrastructure | Data |
|------------|---------|---------------|------|
| [Development] | [Local development] | [Docker Compose] | [Synthetic] |
| [Staging] | [Pre-production testing] | [Cloud — scaled down] | [Anonymized prod copy] |
| [Production] | [Live system] | [Cloud — full scale] | [Real data] |

## 6. High Availability & Disaster Recovery

| Component | HA Strategy | DR Strategy | RTO | RPO |
|-----------|-----------|------------|-----|-----|
| [Web/API Servers] | [Multi-AZ, auto-scaling] | [Cross-region failover] | [5 min] | [0] |
| [Database] | [Multi-AZ, auto-failover] | [Cross-region replica] | [1 hour] | [1 hour] |
| [Cache] | [Cluster mode] | [Rebuild from DB] | [15 min] | [N/A] |
| [Object Storage] | [Cross-region replication] | [Automatic] | [Immediate] | [0] |
| [Load Balancer] | [Multi-AZ] | [DNS failover] | [5 min] | [0] |

## 7. Security Architecture

| Layer | Control | Implementation |
|-------|---------|---------------|
| [Network] | [Firewall, WAF, DDoS protection] | [Security groups, AWS WAF, Shield] |
| [Transport] | [TLS 1.3 everywhere] | [ACM certificates, HTTPS only] |
| [Application] | [OAuth2, JWT, rate limiting] | [API Gateway, Auth Service] |
| [Data] | [Encryption at rest, in transit] | [KMS, TLS] |
| [Access] | [IAM, RBAC, MFA] | [IAM policies, Auth Service] |
| [Audit] | [All actions logged] | [CloudTrail, application audit log] |

## 8. Monitoring & Observability

| Component | Tool | Metrics | Alerts |
|-----------|------|---------|--------|
| [Infrastructure] | [CloudWatch / Azure Monitor] | [CPU, memory, disk, network] | [Threshold alerts] |
| [Application] | [Prometheus + Grafana] | [Request rate, latency, errors] | [SLO alerts] |
| [Logs] | [ELK Stack / CloudWatch Logs] | [Structured JSON logs] | [Error patterns] |
| [Traces] | [Jaeger / X-Ray] | [Distributed traces] | [Slow traces] |
| [Uptime] | [Pingdom / UptimeRobot] | [Synthetic monitoring] | [Downtime alerts] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Logical Architecture]] | Logical components deployed here |
| [[System Architecture Description]] | Comprehensive architecture |
| [[Interface Control Document (ICD)]] | Interface specifications |
| [[Infrastructure-as-Code]] | IaC definitions |

---

> **Template Standard:** Based on SEBoK v2, ISO/IEC/IEEE 42010
> **Usage:** The physical architecture is *technology-specific* — it shows where things actually run. Use it for infrastructure planning, cost estimation, and security design.
