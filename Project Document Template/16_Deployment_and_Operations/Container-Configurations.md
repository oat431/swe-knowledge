---
document_type: Container Configurations
version: "1.0"
status: Active
author: "[DevOps Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [docker, containers, kubernetes, swebok]
standard_ref:
  - SWEBOK v4 — Operations
---

# Container Configurations

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines Docker and Kubernetes configurations — images, deployments, services, and resources.

## 2. Docker Configuration

### Dockerfile

```dockerfile
# Multi-stage build
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build

FROM node:20-alpine AS runtime
WORKDIR /app
RUN addgroup -g 1001 -S nodejs && adduser -S nodejs -u 1001
COPY --from=builder --chown=nodejs:nodejs /app/dist ./dist
COPY --from=builder --chown=nodejs:nodejs /app/node_modules ./node_modules
COPY --from=builder --chown=nodejs:nodejs /app/package.json ./
USER nodejs
EXPOSE 3000
HEALTHCHECK --interval=30s --timeout=3s CMD curl -f http://localhost:3000/health || exit 1
CMD ["node", "dist/main.js"]
```

## 3. Kubernetes Configuration

### Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app
  labels:
    app: project
spec:
  replicas: 2
  selector:
    matchLabels:
      app: project
  template:
    metadata:
      labels:
        app: project
    spec:
      containers:
        - name: app
          image: ghcr.io/org/project:latest
          ports:
            - containerPort: 3000
          resources:
            requests:
              cpu: 250m
              memory: 256Mi
            limits:
              cpu: 500m
              memory: 512Mi
          livenessProbe:
            httpGet:
              path: /health
              port: 3000
            initialDelaySeconds: 30
            periodSeconds: 10
          readinessProbe:
            httpGet:
              path: /ready
              port: 3000
            initialDelaySeconds: 5
            periodSeconds: 5
          env:
            - name: NODE_ENV
              value: production
            - name: DATABASE_URL
              valueFrom:
                secretKeyRef:
                  name: app-secrets
                  key: database-url
```

### Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: app-service
spec:
  selector:
    app: project
  ports:
    - port: 80
      targetPort: 3000
  type: ClusterIP
```

### HPA (Horizontal Pod Autoscaler)

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: app
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70
```

## 4. Resource Specifications

| Component | CPU Request | CPU Limit | Memory Request | Memory Limit |
|---------|-----------|----------|---------------|-------------|
| [Web Pods] | [250m] | [500m] | [256Mi] | [512Mi] |
| [API Pods] | [500m] | [1000m] | [512Mi] | [1Gi] |
| [Worker Pods] | [250m] | [500m] | [256Mi] | [512Mi] |

## 5. Health Checks

| Probe | Path | Initial Delay | Period | Timeout |
|-------|------|-------------|--------|---------|
| [Liveness] | [/health] | [30s] | [10s] | [3s] |
| [Readiness] | [/ready] | [5s] | [5s] | [3s] |
| [Startup] | [/health] | [0s] | [5s] | [3s] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Infrastructure-as-Code]] | IaC definitions |
| [[Deployment-Plan]] | Deployment procedures |
| [[Container-Configurations]] | This document |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** Container configs are *versioned code*. Keep them in the repo. Never configure production manually.
