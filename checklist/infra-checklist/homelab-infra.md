# Homelab Infrastructure Checklist

> Self-hosted infrastructure checklist for homelabs running on bare metal, Proxmox, or Docker.
> From hardware to services — everything you need to run reliable self-hosted infrastructure at home.
> Last updated: 2026-07-17

---

## 1. Hardware & Base Platform

- [ ] **Hardware choice** — Mini PC (Minisforum, Beelink, Intel NUC) for quiet/efficient, or refurbished enterprise (Dell OptiPlex, HP ProDesk) for value. 32GB RAM minimum for a useful stack, 64GB for dense workloads.
- [ ] **Storage strategy** — SSD for OS + containers (fast, reliable). HDD for bulk data (media, backups). Consider RAID or ZFS mirror for critical data. Separate boot drive from data drives.
- [ ] **UPS (Uninterruptible Power Supply)** — Protect against power cuts. Auto-shutdown script on low battery (`apcupsd` or `nut`). Even a small UPS prevents filesystem corruption from sudden power loss.
- [ ] **Base OS** — Ubuntu Server LTS, Debian, or Proxmox VE. Headless (no desktop environment). Auto-updates for security patches (`unattended-upgrades`). SSH access only.
- [ ] **Proxmox VE (if virtualizing)** — Type-1 hypervisor. VMs for full isolation (different OS, kernel-level separation). LXC containers for lightweight Linux services (less overhead than VMs, more isolation than Docker). Snapshots before risky changes.
- [ ] **Docker (container runtime)** — Docker Engine + Docker Compose for service management. One `docker-compose.yml` per service or per stack. Named volumes for persistent data. Never store data in the container layer.
- [ ] **Resource monitoring** — Know your limits: CPU, RAM, disk, temperature. `htop`, `btop`, `glances`, or Netdata for real-time. Set alerts before you hit capacity.

## 2. Networking

- [ ] **Static IP for the server** — DHCP reservation in router or static config on server. Services must have predictable addresses.
- [ ] **Local DNS** — Pi-hole or AdGuard Home for ad-blocking + local DNS resolution. Map `service.home.lab` → `192.168.1.x`. No more remembering port numbers.
- [ ] **VLAN segmentation (if router supports)** — Separate IoT devices from server from personal devices. IoT VLAN can't reach server management ports. Defense in depth.
- [ ] **Reverse proxy** — Traefik, Nginx Proxy Manager, or Caddy. Single entry point for all services. Routes by subdomain (`grafana.home.lab`, `git.home.lab`). Terminates TLS.
- [ ] **Wildcard TLS/SSL** — HTTPS for all services, even internally. Wildcard cert via DNS challenge (Cloudflare, Let's Encrypt). No browser warnings. Browsers are increasingly blocking HTTP.
- [ ] **Firewall** — `ufw` or `iptables`/`nftables`. Default deny inbound. Only expose what's needed: 80/443 (reverse proxy), 22 (SSH, key-only). Close everything else.
- [ ] **No port forwarding (if possible)** — Use Cloudflare Tunnel or Tailscale to expose services without opening router ports. Zero ports open = zero attack surface from the internet.
- [ ] **Tailscale / WireGuard** — Mesh VPN for remote access. Access your homelab from anywhere without exposing to public internet. Tailscale = zero-config. WireGuard = more control.

## 3. Remote Access & Exposure

- [ ] **Cloudflare Tunnel (recommended for public services)** — Expose selected services (blog, portfolio, API) through Cloudflare without opening ports. DDoS protection included. Free tier.
- [ ] **Tailscale (recommended for private access)** — Access everything from your phone/laptop/tablet over encrypted mesh. No port forwarding. Works behind CGNAT. Free for personal use.
- [ ] **SSH hardened** — Key-only auth (disable password). Non-standard port (optional, security through obscurity is weak but reduces noise). `fail2ban` for brute force protection. `AllowUsers` whitelist.
- [ ] **No direct public IP exposure** — Your server should never be directly internet-facing. Always behind Cloudflare Tunnel, VPN, or at minimum a hardened reverse proxy.
- [ ] **Wake-on-LAN (optional)** — If you power down the server to save electricity: WoL allows remote wake. Tailscale + WoL = turn on from anywhere.

## 4. Container Management

- [ ] **Docker Compose per service** — One `docker-compose.yml` per logical service. Keeps services independent. Easy to start/stop/update individually.
- [ ] **Named volumes** — `volumes: data:/var/lib/service`. Not bind mounts to random directories. Organized, predictable, easy to backup.
- [ ] **Restart policy** — `restart: unless-stopped` for all services. Survives reboots. Only manually stopped services stay down.
- [ ] **Resource limits** — `mem_limit` and `cpus` in compose. One misbehaving container shouldn't OOM-kill everything else.
- [ ] **Container updates** — Watchtower (auto-update, risky but convenient) or manual `docker compose pull && docker compose up -d`. Pin image tags for critical services (not `:latest`).
- [ ] **Docker network isolation** — Create custom networks per stack. Services only communicate with what they need. Default bridge network = everything can talk to everything.
- [ ] **Portainer (optional)** — Web UI for Docker management. Useful for quick overview, logs, container restarts. Not a replacement for compose files.

## 5. Storage & Backups

- [ ] **Backup strategy (3-2-1 rule)** — 3 copies of data, 2 different media, 1 offsite. Local backup (external drive or NAS) + cloud backup (Backblaze B2, rclone to cloud).
- [ ] **Automated backups** — Cron job or systemd timer. Daily for critical data (databases, configs). Weekly for bulk data. Don't rely on manual backups.
- [ ] **Docker volume backup** — Script that stops container → tar volume → restart. Or use `docker-volume-backup` container. Backup the data, not the container image.
- [ ] **Database backups** — `pg_dump` / `mysqldump` before volume backup. Point-in-time recovery for critical databases. Test restore quarterly.
- [ ] **Config backup (GitOps)** — All compose files, env files (secrets excluded), and configs in a private Git repo. If server dies, you can recreate everything from the repo.
- [ ] **Backup testing** — Restore a backup to verify it works. Untested backups are assumptions, not backups. Schedule a test quarterly.
- [ ] **Encryption at rest** — LUKS for full disk encryption (or per-partition). Especially important if server is physically accessible or could be stolen.

## 6. Monitoring & Observability

- [ ] **System metrics** — Prometheus + Node Exporter (CPU, RAM, disk, network, temperature). Or Netdata for zero-config instant monitoring.
- [ ] **Container metrics** — cAdvisor for per-container resource usage. Know which service is eating your RAM.
- [ ] **Dashboards** — Grafana for visualization. Pre-built dashboards for Node Exporter, Docker, Traefik. Accessible at `grafana.home.lab`.
- [ ] **Log aggregation** — Loki + Promtail (lightweight, Grafana-native) or Dozzle (simple Docker log viewer). Centralized logs = don't SSH into each container to debug.
- [ ] **Uptime monitoring** — Uptime Kuma (self-hosted). Monitors all your services. Alerts via Telegram/Discord/email when something goes down.
- [ ] **Alerting** — Grafana alerts or Uptime Kuma notifications. Service down → notification within 1 minute. Disk > 80% → warning. Temperature spike → alert.
- [ ] **Health checks in compose** — `healthcheck` directive in docker-compose. Docker marks unhealthy containers. Integrates with monitoring.

## 7. Security

- [ ] **Automatic security updates** — `unattended-upgrades` (Debian/Ubuntu). Security patches applied automatically. Reboot scheduled weekly if kernel updated.
- [ ] **Non-root containers** — Run container processes as non-root user where possible. `user: 1000:1000` in compose. Reduces blast radius of container escape.
- [ ] **No default passwords** — Every service has a strong, unique password. Password manager (Vaultwarden self-hosted, or Bitwarden). Never `admin:admin`.
- [ ] **Secrets management** — Docker secrets or `.env` files (chmod 600, gitignored). Never hardcode passwords in compose files committed to git.
- [ ] **fail2ban** — Monitors SSH, reverse proxy, and service logs. Bans IPs after failed auth attempts. Auto-unban after timeout.
- [ ] **Crowdsec (optional upgrade from fail2ban)** — Community-driven threat intelligence. Shares attack patterns across users. More sophisticated than fail2ban.
- [ ] **Container image scanning** — `trivy image myimage:tag` before deploying. Know your vulnerabilities. Don't run ancient, unpatched images.
- [ ] **Regular audits** — Monthly: check running containers (`docker ps`), check open ports (`ss -tlnp`), review who has SSH access, check for unused services.

## 8. Services to Self-Host

### Essential (every homelab should have)

- [ ] **Reverse proxy** — Traefik or Nginx Proxy Manager. Entry point for everything.
- [ ] **DNS + ad-blocking** — Pi-hole or AdGuard Home. Network-wide ad blocking + local DNS.
- [ ] **Monitoring** — Uptime Kuma (simple) or Prometheus + Grafana (full stack).
- [ ] **Backup solution** — Duplicati, Restic, or Borgmatic. Automated, encrypted backups.
- [ ] **Git server (optional)** — Gitea or Forgejo. Host your own repos. Lightweight alternative to GitHub.

### Developer-focused

- [ ] **Container registry** — Harbor or Docker Registry. Store your own images locally.
- [ ] **CI/CD runner** — Gitea Actions, Drone CI, or self-hosted GitHub Actions runner. Build and deploy from your own infra.
- [ ] **Database server** — PostgreSQL or MySQL in container. Shared by multiple services. Regular backups.
- [ ] **Redis / Valkey** — Caching and session storage for your apps.

### Productivity / Personal

- [ ] **Password manager** — Vaultwarden (Bitwarden-compatible). Self-hosted, encrypted, synced across devices.
- [ ] **Note sync** — Obsidian Sync alternative (Syncthing, LiveSync plugin, or CouchDB). Keep your notes synced.
- [ ] **File sync** — Nextcloud or Syncthing. Self-hosted Dropbox replacement.
- [ ] **Media** — Jellyfin (video), Navidrome (music). Your own streaming services.

## 9. Maintenance & Operations

- [ ] **Runbook** — Document: how to restart each service, how to restore from backup, how to update the OS, how to replace a failed drive, how to add a new service.
- [ ] **Update schedule** — OS: automatic security patches daily, manual major upgrades quarterly. Containers: review and pull updates weekly/monthly.
- [ ] **Changelog** — Track what you changed and when. A simple log file or git commit messages. "Why is X broken?" → check what changed recently.
- [ ] **Disaster recovery plan** — Server dies completely. How long to rebuild? What do you need? Answer: git repo with all configs + latest backup = rebuild in hours, not days.
- [ ] **Power consumption** — Know your watt draw. Kill-A-Watt meter. Mini PCs: 15-35W idle. Enterprise server: 100-300W idle. Affects electricity bill.
- [ ] **Noise management** — If in living space: mini PCs are silent. Rack servers are not. Fan curves, schedule heavy workloads for daytime.

---

## Quick Sanity Check Before "Production"

- [ ] All services accessible via `service.home.lab` (not IP:port)
- [ ] HTTPS on all services (wildcard cert, no browser warnings)
- [ ] SSH is key-only (password auth disabled)
- [ ] No ports open on router (Cloudflare Tunnel or Tailscale for remote access)
- [ ] Backups automated and tested (can you actually restore?)
- [ ] Monitoring alerts you when something goes down (test by stopping a service)
- [ ] Firewall default-deny (only 80/443/22 open on server, nothing else)
- [ ] All compose files in git (could rebuild from scratch with repo + backup)
- [ ] Docker containers have restart policies (survive reboot)
- [ ] No default passwords anywhere
- [ ] Disk space monitored with alert at 80%
- [ ] UPS protects against sudden power loss
- [ ] You know your monthly power cost

---

## Platform Quick Reference

### Virtualization / Base

| Platform | Best for |
|----------|----------|
| **Proxmox VE** | Multiple VMs + LXC containers, snapshot/restore, web UI |
| **Bare metal + Docker** | Simplest setup, one OS, all containers |
| **TrueNAS** | Storage-focused (ZFS), apps via containers |
| **Unraid** | Mixed drives, Docker + VM support, user-friendly |

### Reverse Proxy

| Tool | Best for |
|------|----------|
| **Traefik** | Auto-discovery (Docker labels), Let's Encrypt built-in, powerful |
| **Nginx Proxy Manager** | Web UI for config, beginner-friendly, visual |
| **Caddy** | Simplest config, auto-HTTPS, minimal setup |

### Remote Access

| Tool | Best for |
|------|----------|
| **Tailscale** | Zero-config mesh VPN, access from anywhere, free tier |
| **Cloudflare Tunnel** | Expose public services without port forwarding, DDoS protection |
| **WireGuard** | Manual VPN, full control, lightweight, fast |

### Monitoring

| Tool | Best for |
|------|----------|
| **Uptime Kuma** | Simple status page + alerts, 5-minute setup |
| **Prometheus + Grafana** | Full metrics stack, dashboards, alerting |
| **Netdata** | Zero-config, real-time, per-process metrics |

### Backup

| Tool | Best for |
|------|----------|
| **Restic** | Deduplication, encryption, supports S3/B2/local |
| **Borgmatic** | Borg wrapper with YAML config, scheduled backups |
| **Duplicati** | Web UI, encryption, cloud targets, beginner-friendly |
| **rclone** | Sync to any cloud provider (B2, S3, GDrive, OneDrive) |
