---
tags:
  - programming
  - fundamental
  - devops
  - docker
---

# Docker & Containerization

Containers revolutionized how we package and deploy applications. Docker makes it possible to build once and run anywhere — from a developer laptop to production clusters. Understanding containers is non-negotiable for modern backend engineering.

---

## VM vs Container

| Aspect | Virtual Machine | Container |
|---|---|---|
| **Isolation** | Full OS kernel per VM | Shared host kernel |
| **Size** | 1-10 GB | 10-500 MB |
| **Boot time** | Minutes | Milliseconds |
| **Overhead** | High (hypervisor) | Low (namespaces + cgroups) |
| **Density** | 10-20 per host | 100s per host |
| **OS** | Any (Windows, Linux) | Must match host kernel |

---

## Docker Core Concepts

| Concept | Description |
|---|---|
| **Image** | Read-only template with app code + dependencies + runtime |
| **Container** | Running instance of an image |
| **Registry** | Storage for images (Docker Hub, ECR, GCR) |
| **Volume** | Persistent data that survives container restarts |
| **Network** | Virtual network connecting containers |

---

## Dockerfile Best Practices

### Multi-Stage Build (Java Example)

```dockerfile
# Stage 1: Build
FROM maven:3.9-eclipse-temurin-21 AS builder
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline -B
COPY src ./src
RUN mvn clean package -DskipTests -B

# Stage 2: Run
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
COPY --from=builder /app/target/*.jar app.jar
USER appuser
EXPOSE 8080
HEALTHCHECK --interval=30s --timeout=3s CMD wget -q --spider http://localhost:8080/actuator/health || exit 1
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### Layer Caching Optimization

❌ **Copy everything at once:**
```dockerfile
# ❌ Any source change invalidates Maven dependency cache
COPY . .
RUN mvn package
```

✅ **Copy dependency files first:**
```dockerfile
# ✅ pom.xml changes rarely — dependencies cached in this layer
COPY pom.xml .
RUN mvn dependency:go-offline -B
# Source changes only invalidate from here down
COPY src ./src
RUN mvn package -DskipTests
```

### Running as Root

❌ **Default root user:**
```dockerfile
FROM eclipse-temurin:21
COPY target/app.jar app.jar
# ❌ Runs as root — security risk
ENTRYPOINT ["java", "-jar", "app.jar"]
```

✅ **Use non-root user:**
```dockerfile
FROM eclipse-temurin:21
RUN addgroup --system appgroup && adduser --system --ingroup appgroup appuser
COPY target/app.jar app.jar
USER appuser
# ✅ Runs as unprivileged user
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### .dockerignore

```
.git
.gitignore
target/
*.md
.idea/
.vscode/
node_modules/
.env
docker-compose*.yml
Dockerfile*
```

### Base Image Choices

| Image | Size | Use Case |
|---|---|---|
| `eclipse-temurin:21` | ~450 MB | Development, debugging |
| `eclipse-temurin:21-jre` | ~250 MB | Production (JRE only) |
| `eclipse-temurin:21-jre-alpine` | ~180 MB | Minimal production |
| `gcr.io/distroless/java21` | ~150 MB | Maximum security, no shell |

---

## Docker Compose

Full example with Java app + PostgreSQL + Redis:

```yaml
version: "3.9"

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "8080:8080"
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://postgres:5432/mydb
      SPRING_DATASOURCE_USERNAME: postgres
      SPRING_DATASOURCE_PASSWORD: ${DB_PASSWORD}
      SPRING_REDIS_HOST: redis
      SPRING_REDIS_PORT: 6379
    depends_on:
      postgres:
        condition: service_healthy
      redis:
        condition: service_started
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "wget", "-q", "--spider", "http://localhost:8080/actuator/health"]
      interval: 30s
      timeout: 5s
      retries: 3
      start_period: 40s

  postgres:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    ports:
      - "5432:5432"
    volumes:
      - postgres-data:/var/lib/postgresql/data
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql:ro
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis-data:/data
    command: redis-server --appendonly yes

volumes:
  postgres-data:
  redis-data:
```

---

## Networking

| Driver | Description | When to Use |
|---|---|---|
| **bridge** | Default. Containers on same host communicate via internal network | Most local development |
| **host** | Container shares host's network stack directly | High-performance, no port mapping needed |
| **none** | No networking at all | Batch jobs, security-critical isolation |

```bash
# Create a custom bridge network
docker network create --driver bridge my-network

# Run containers on the same network
docker run --network my-network --name app my-image
docker run --network my-network --name db postgres:16
# Now "app" can reach "db" by hostname
```

---

## Volumes

| Type | Description | Persisted? |
|---|---|---|
| **Named Volume** | Managed by Docker, stored in `/var/lib/docker/volumes/` | ✅ Yes |
| **Bind Mount** | Maps host directory into container | ✅ Yes (host path) |
| **tmpfs** | In-memory only, never written to disk | ❌ No |

```bash
# Named volume
docker volume create my-data
docker run -v my-data:/app/data my-image

# Bind mount
docker run -v /host/path:/container/path my-image
```

---

## Docker Security

| Practice | Description |
|---|---|
| ✅ Read-only filesystem | `--read-only` + `--tmpfs /tmp` |
| ✅ No privileged mode | Never use `--privileged` |
| ✅ Scan images | `trivy image myapp:latest` |
| ✅ Minimal base images | Use alpine or distroless |
| ✅ Non-root user | `USER appuser` in Dockerfile |
| ✅ Drop capabilities | `--cap-drop ALL --cap-add NET_BIND_SERVICE` |

```bash
# Scan for vulnerabilities
trivy image --severity HIGH,CRITICAL myapp:latest

# Run with security hardening
docker run \
  --read-only \
  --tmpfs /tmp \
  --cap-drop ALL \
  --security-opt no-new-privileges \
  --user 1000:1000 \
  myapp:latest
```

---

## Common Commands Cheat Sheet

| Command | Description |
|---|---|
| `docker build -t name:tag .` | Build image from Dockerfile |
| `docker run -d -p 8080:8080 name` | Run container in background |
| `docker ps` | List running containers |
| `docker ps -a` | List all containers (including stopped) |
| `docker logs -f container` | Follow container logs |
| `docker exec -it container bash` | Shell into running container |
| `docker stop container` | Gracefully stop container |
| `docker rm container` | Remove stopped container |
| `docker rmi image` | Remove image |
| `docker compose up -d` | Start all services in background |
| `docker compose down -v` | Stop and remove containers + volumes |
| `docker system prune -af` | Remove all unused data |
| `docker image prune -af` | Remove unused images |
| `docker stats` | Live resource usage |

---

## Docker in Production

### Health Checks

```dockerfile
HEALTHCHECK --interval=30s --timeout=5s --start-period=40s --retries=3 \
  CMD curl -f http://localhost:8080/health || exit 1
```

### Resource Limits (Compose)

```yaml
services:
  app:
    deploy:
      resources:
        limits:
          cpus: "2.0"
          memory: 512M
        reservations:
          cpus: "0.5"
          memory: 256M
      restart_policy:
        condition: on-failure
        delay: 5s
        max_attempts: 3
        window: 120s
```

### Logging

```yaml
services:
  app:
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "3"
```

### Restart Policies

| Policy | Behavior |
|---|---|
| `no` | Never restart (default) |
| `always` | Always restart, even on manual stop |
| `unless-stopped` | Restart unless explicitly stopped |
| `on-failure[:max]` | Restart only on non-zero exit, optional max retries |

---

## Related Notes

- [[07 Containers & Orchestration]]
- [[03 Container & Cloud Security]]

---

## Sources

- [Docker Documentation](https://docs.docker.com/)
- [Dockerfile Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- [Docker Compose Specification](https://docs.docker.com/compose/compose-file/)
- [Trivy Documentation](https://aquasecurity.github.io/trivy/)
