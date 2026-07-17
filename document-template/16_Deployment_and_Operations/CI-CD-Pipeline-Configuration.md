---
document_type: CI/CD Pipeline Configuration
version: "1.0"
status: Active
author: "[DevOps Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [ci-cd, pipeline, github-actions, devops]
standard_ref:
  - SWEBOK v4 — Operations
---

# CI/CD Pipeline Configuration

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines the CI/CD pipeline — build, test, and deployment automation that ensures consistent, reliable releases.

## 2. Pipeline Overview

```mermaid
flowchart LR
    PUSH[Git Push] --> LINT[Lint]
    LINT --> TYPE[Type Check]
    TYPE --> TEST[Test]
    TEST --> BUILD[Build]
    BUILD --> IMAGE[Docker Image]
    IMAGE --> SCAN[Security Scan]
    SCAN --> PUSH_REG[Push to Registry]
    PUSH_REG --> DEPLOY_STG[Deploy Staging]
    DEPLOY_STG --> SMOKE[Smoke Test]
    SMOKE --> DEPLOY_PROD[Deploy Production]

    style PUSH fill:#2196F3,color:#fff
    style LINT fill:#FF9800,color:#fff
    style TEST fill:#4CAF50,color:#fff
    style BUILD fill:#9C27B0,color:#fff
    style SCAN fill:#f44336,color:#fff
    style DEPLOY_PROD fill:#4CAF50,color:#fff
```

## 3. Pipeline Configuration

```yaml
# .github/workflows/ci-cd.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run lint
      - run: npm run type-check

  test:
    needs: lint
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_DB: test
          POSTGRES_PASSWORD: test
        ports: ['5432:5432']
      redis:
        image: redis:7
        ports: ['6379:6379']
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
      - run: npm ci
      - run: npm test -- --coverage --ci
      - uses: codecov/codecov-action@v3

  build:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v4
      - uses: docker/setup-buildx-action@v3
      - uses: docker/login-action@v3
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
      - uses: docker/build-push-action@v5
        with:
          push: true
          tags: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:${{ github.sha }}

  deploy-staging:
    needs: build
    runs-on: ubuntu-latest
    environment: staging
    steps:
      - run: kubectl set image deployment/app app=${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:${{ github.sha }}
      - run: npm run test:smoke

  deploy-production:
    needs: deploy-staging
    runs-on: ubuntu-latest
    environment: production
    steps:
      - run: kubectl set image deployment/app app=${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:${{ github.sha }}
      - run: npm run test:smoke
```

## 4. Pipeline Stages

| Stage | Purpose | Duration | Failure Action |
|-------|---------|---------|---------------|
| [Lint] | [Code style check] | [< 1 min] | [Block merge] |
| [Type Check] | [TypeScript validation] | [< 1 min] | [Block merge] |
| [Test] | [Unit + integration tests] | [< 5 min] | [Block merge] |
| [Build] | [Docker image build] | [< 3 min] | [Block merge] |
| [Security Scan] | [Vulnerability scan] | [< 2 min] | [Block on critical] |
| [Deploy Staging] | [Deploy to staging] | [< 2 min] | [Alert team] |
| [Smoke Test] | [Verify critical paths] | [< 5 min] | [Rollback] |
| [Deploy Prod] | [Deploy to production] | [< 2 min] | [Rollback] |

## 5. Environment Configuration

| Environment | Branch | Auto-Deploy | Approval | URL |
|------------|--------|------------|---------|-----|
| [Development] | [develop] | [Yes] | [No] | [dev.project.com] |
| [Staging] | [main] | [Yes] | [No] | [staging.project.com] |
| [Production] | [main] | [No] | [Required] | [project.com] |

## 6. Secrets Management

| Secret | Environment | Rotation |
|--------|-----------|---------|
| [DATABASE_URL] | [All] | [Quarterly] |
| [JWT_SECRET] | [All] | [Quarterly] |
| [API_KEYS] | [All] | [Monthly] |
| [CLOUD_CREDENTIALS] | [Staging, Prod] | [Quarterly] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Deployment-Plan]] | Deployment procedures |
| [[Build-Scripts]] | Build configuration |
| [[Infrastructure-as-Code]] | Infrastructure definition |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** The pipeline is *the single path to production*. If it's not in the pipeline, it doesn't ship.
