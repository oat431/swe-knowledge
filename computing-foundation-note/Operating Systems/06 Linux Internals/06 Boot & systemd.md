---
tags:
- linux
- os
- programming
- systemd
---

# 06 Boot & systemd

From pressing the power button to a running multi-user system — and why PID 1 owns services, logs, cgroups, and increasingly everything else.

---

## The Boot Chain

```
Power on
  ▼
UEFI firmware (POST, device init)      ← old world: legacy BIOS
  ▼
Bootloader: GRUB2 / systemd-boot       ← picks kernel, loads it + initramfs into RAM
  ▼
Kernel decompresses, initializes, mounts initramfs as rootfs
  ▼
initramfs: loads drivers, finds & mounts the REAL root (LVM/RAID/LUKS)
  ▼
switch_root → /sbin/init == systemd    ← PID 1, never exits
  ▼
default.target (e.g. multi-user.target / graphical.target)
  ▼
Socket/D-Bus activation, parallel service start → login prompt
```

Kernel knows almost nothing about *your* disk layout at first — it can't even read `/etc/fstab`. That's the initramfs's whole job.

---

## UEFI vs Legacy BIOS

| | Legacy BIOS | UEFI |
|---|-------------|------|
| Partition table | MBR (max 4 primary, 2 TiB disk limit) | **GPT** (128 partitions default, ZB-scale) |
| Boot code location | MBR first 512 bytes → chainloading | `.efi` executables on the **ESP** (FAT32, `/boot/efi`) |
| Mode | 16-bit real mode start | 32/64-bit protected mode from the jump |
| Bootloader install | `grub-install /dev/sda` (MBR) | NVRAM boot entries, `efibootmgr` |
| Secure Boot | No | Yes — only signed loaders/kernels (shim → GRUB → kernel); one line: it verifies signatures, it does **not** make the OS secure |

---

## initramfs

> A small root filesystem in RAM. Its purpose: assemble the environment needed to mount the real root.

- Loads storage drivers (NVMe, virtio), filesystem modules
- Unlocks **LUKS** encryption, assembles **RAID/mdadm**, activates **LVM** volume groups
- Then `switch_root` — pivot to the real root and exec `/sbin/init`

Built by **dracut** (RHEL/Fedora/SUSE) or `mkinitramfs` (Debian/Ubuntu):

```bash
lsinitrd | head                 # inspect current initramfs
dracut -f /boot/initramfs-$(uname -r).img $(uname -r)   # rebuild
```

---

## Kernel Command Line

Passed by GRUB to the kernel — visible at `/proc/cmdline`:

| Param | Meaning |
|-------|---------|
| `root=UUID=...` | Where the real root is (initramfs target) |
| `ro` | Mount root read-only first (fsck safety), remount rw later |
| `quiet` | Suppress most boot messages (pairs with `splash`) |
| `crashkernel=256M` | Reserve RAM for kdump kernel — crash dumps |
| `console=ttyS0` | Serial console — cloud VMs, headless servers |
| `systemd.unit=rescue.target` | Override boot target (single-user fix-it mode) |

**Edit in `/etc/default/grub`** (`GRUB_CMDLINE_LINUX=`), then regenerate:

```bash
# Debian/Ubuntu:            # RHEL/Fedora (BIOS):        # UEFI:
update-grub                 grub2-mkconfig -o /boot/grub2/grub.cfg
                            grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg
```

Never hand-edit the generated `grub.cfg` — it's overwritten on kernel updates.

---

## systemd Architecture — Units

> Everything is a **unit**: a declarative config object. systemd manages dependencies and ordering between units in parallel.

| Unit type | Suffix | Manages |
|-----------|--------|---------|
| service | `.service` | Daemons/processes |
| socket | `.socket` | Listening fds → start service on first connection |
| target | `.target` | Groups of units (sync points, "runlevels") |
| timer | `.timer` | Cron replacement — triggers a same-named `.service` |
| mount | `.mount` | Filesystems (`/etc/fstab` auto-generates these) |
| path | `.path` | inotify-based triggers |

### Unit File Anatomy

```ini
[Unit]
Description=My API server
After=network-online.target postgresql.service
Wants=network-online.target

[Service]
Type=notify
ExecStart=/usr/bin/myapi --config /etc/myapi/config.yaml
Restart=on-failure
RestartSec=2s
User=myapi
MemoryMax=512M
Environment=PORT=8080

[Install]
WantedBy=multi-user.target
```

- `Restart=on-failure` — restart on non-zero exit/signal, **not** on clean exit (`always` restarts regardless; `on-abnormal` skips non-zero codes)
- `[Install]` + `WantedBy=` is what `systemctl enable` uses to create the symlink
- Unit files: `/usr/lib/systemd/system/` (vendor) vs `/etc/systemd/system/` (admin — **wins**)

### Service Types

| Type | Semantics | Example |
|------|-----------|---------|
| `simple` (default) | Started == running; exec'd process is *the* service | Most modern daemons |
| `notify` | Service sends `READY=1` via sd_notify — systemd knows it's *actually* up | nginx, postgres, anything sd_notify-aware |
| `forking` | Classic: parent forks, parent exits, child is the daemon — needs `PIDFile=` | Legacy sysv-style daemons |
| `oneshot` | Run to completion, then done (can stay "active/exited") | Migrations, sysctl setup |
| `exec` | Like simple but waits for exec() to succeed | simple, stricter |

---

## Targets vs Runlevels

| Old (SysV) | systemd target | Meaning |
|------------|----------------|---------|
| runlevel 0 | `poweroff.target` | Off |
| runlevel 1 | `rescue.target` | Single user |
| runlevel 3 | `multi-user.target` | Full system, no GUI — **servers live here** |
| runlevel 5 | `graphical.target` | Desktop |
| runlevel 6 | `reboot.target` | Reboot |

```bash
systemctl get-default                  # current default target
systemctl set-default multi-user.target
```

---

## Dependencies & Ordering

Two **independent** axes — confusing them is the #1 unit-file bug:

| Directive | Meaning |
|-----------|---------|
| `Requires=X` | Hard dep: X starts too; if X fails/is stopped, this unit stops |
| `Wants=X` | Soft dep: try to start X, don't care if it fails |
| `After=X` | **Ordering only**: start after X finishes starting. No dependency created! |
| `Before=X` | Inverse ordering |

`After=network.target` alone doesn't pull network in; you almost always want `Wants=network-online.target` + `After=network-online.target`.

```bash
systemd-analyze verify my.service      # lint a unit file
systemctl list-dependencies my.service # dependency tree
```

---

## journald

Structured binary logs (fields, not text lines) written to `/run/log/journal` (volatile — lost on reboot) or `/var/log/journal` (persistent — enable with `mkdir /var/log/journal && systemctl restart systemd-journald`, or `Storage=persistent` in `journald.conf`).

```bash
journalctl -u nginx -f                 # follow one unit
journalctl --since "10 min ago"
journalctl -p err -b                   # errors this boot
journalctl -k                          # kernel ring buffer (replaces dmesg)
journalctl -o json-pretty -u myapi     # structured output — every field
```

Units log to stdout/stderr; journald captures it — no log files to configure. Rate-limits per service by default (`RateLimitBurst=`), which has bitten more than one debugging session.

---

## Socket & D-Bus Activation

`.socket` unit owns the listening fd; matching `.service` starts **on first connection** — services can boot in parallel without racing. Same idea as inetd, and how D-Bus works: calling a method on an inactive service wakes it (`dbus.socket`/`.service`). Also enables zero-downtime restarts of the listener.

---

## systemd & cgroups

> Every unit runs in its **own cgroup** — that's how systemd tracks processes (no PID-file games), and how resource limits in unit files work.

- Hierarchy: **slices** (resource budget containers: `system.slice`, `user.slice`) contain **scopes** (external processes) and **services**
- `MemoryMax=512M` in a unit file → `memory.max=536870912` in `/sys/fs/cgroup/system.slice/my.service/` — the same cgroup v2 controllers containers use; see [[05 Containers - Namespaces & Cgroups]]
- `systemd-run --scope -p MemoryMax=200M stress-ng --vm 1` — ad-hoc cgroup for any command

---

## Practical systemctl

| Command | Use |
|---------|-----|
| `systemctl status nginx` | State, last logs, cgroup, memory |
| `systemctl daemon-reload` | **After editing any unit file** — reread from disk |
| `systemctl enable --now nginx` | Symlink at boot + start immediately |
| `systemctl edit nginx` | Create a drop-in override (see below) |
| `systemctl edit --full nginx` | Copy the whole unit to /etc (heavier hammer) |
| `systemctl list-dependencies nginx` | What it pulls in |
| `systemctl --failed` | What broke at boot |
| `systemd-analyze blame` | Per-unit start time |
| `systemd-analyze critical-chain` | The slowest path to default.target |

### Drop-in Overrides — Never Edit Vendor Units

`/usr/lib/systemd/system/` files are replaced on package upgrade. Use drop-ins:

```bash
systemctl edit nginx
# opens /etc/systemd/system/nginx.service.d/override.conf:
[Service]
MemoryMax=1G
LimitNOFILE=65536
```

Drop-ins merge with the vendor unit (arrays like `Environment=` append; `ExecStart=` must be reset with `ExecStart=` empty line first). Inspect the merged result: `systemctl cat nginx`.

---

## Sources

- `man systemd(1)`, `man systemd.unit(5)`, `man systemd.service(5)`, `man journalctl(1)`, `man kernel-command-line(7)`
- systemd docs: `systemd.index` — https://www.freedesktop.org/software/systemd/man/
- The Boot Loader Interface (systemd-boot spec) — https://systemd.io/BOOT_LOADER_SPECIFICATION/
- dracut docs — https://github.com/dracutdevs/dracut
- LWN: "Control groups, version 2" — https://lwn.net/Articles/606925/
