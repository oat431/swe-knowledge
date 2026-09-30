---
tags:
- linux
- os
- programming
- security
---

# 06 OS Security Model: Capabilities, SELinux & Seccomp

Classic Unix security is one bit per process: root or not. Everything here (capabilities, seccomp, LSMs, user namespaces) is the kernel growing finer-grained controls so a compromised process doesn't own the machine. This is the security layer containers stand on; see [[05 Containers - Namespaces & Cgroups]] and [[01 System Calls & Kernel]] for the syscall path these mechanisms hook.

---

## The Blunt Root Problem

> DAC (discretionary access control): every file has an owner + permission bits, and the kernel checks them on each access. One exception: **UID 0 bypasses nearly every check.**

The trouble: a web server that needs to `bind(80)` gets all of root; read any file, load kernel modules, reboot the box, change the firewall. One RCE in the app = total compromise. Capabilities, seccomp, and MAC all exist to split that single bit into pieces.

---

## Linux Capabilities

> Root's power is split into ~40 independent bits. Each process carries three sets: **Permitted** (may use), **Effective** (kernel actually checks), **Inheritable** (survives execve).

### The Important Ones

| Capability | Grants |
|------------|--------|
| `CAP_SYS_ADMIN` | ~"the new root" ;  mount, namespaces, bpf, dozens of unrelated ioctls. Giant grab-bag; never grant casually |
| `CAP_NET_ADMIN` | Network config: interfaces, routing, iptables/nftables, `SO_PRIORITY` |
| `CAP_NET_BIND_SERVICE` | `bind()` to ports < 1024 **without root** |
| `CAP_NET_RAW` | Raw sockets ;  ping, packet crafting (also sniffing) |
| `CAP_DAC_OVERRIDE` | Bypass file read/write/execute permission checks |
| `CAP_DAC_READ_SEARCH` | Bypass read + directory-search checks |
| `CAP_CHOWN` | Change file ownership arbitrarily |
| `CAP_FOWNER` | Bypass "must own the file" checks (chmod, utimes…) |
| `CAP_SETUID` / `CAP_SETGID` | Arbitrary UID/GID transitions |
| `CAP_KILL` | Signal any process |
| `CAP_SYS_PTRACE` | ptrace anything ;  read other processes' memory (container-escape vector) |
| `CAP_SYS_MODULE` | Load kernel modules = total root, permanently |
| `CAP_SYS_RESOURCE` | Raise rlimits past hard limits |

```bash
capsh --print                       # current process's sets
getcap /usr/bin/ping                # cap_net_raw+ep /usr/bin/ping
setcap 'cap_net_bind_service=+ep' /usr/bin/myapp   # bind :80 as non-root user
getpcaps <pid>                      # per-process caps
```

### Docker's Default

Docker **drops ALL** capabilities, then adds back a small default set (~14: `CHOWN`, `DAC_OVERRIDE`, `FSETID`, `FOWNER`, `MKNOD`, `NET_RAW`, `SETGID`, `SETUID`, `SETFCAP`, `SETPCAP`, `NET_BIND_SERVICE`, `SYS_CHROOT`, `KILL`, `AUDIT_WRITE`). Notably **absent:** `SYS_ADMIN`, `NET_ADMIN`, `SYS_MODULE`, `SYS_PTRACE`.

```bash
docker run --cap-drop=ALL --cap-add=NET_BIND_SERVICE myapp
docker inspect -f '{{.HostConfig.CapDrop}} {{.HostConfig.CapAdd}}' <c>
```

**Ambient capabilities** (kernel 4.3+): the fourth set that survives execve of a *non-privileged* binary, lets a non-root child keep e.g. `CAP_NET_BIND_SERVICE` without file caps. `systemd` `AmbientCapabilities=` uses this.

### no_new_privs

A one-way per-process bit (`prctl(PR_SET_NO_NEW_PRIVS, 1)`): setuid-root binaries and file capabilities **stop granting anything** on exec from this process tree. Docker sets it when you drop all caps / use user namespaces; systemd `NoNewPrivileges=yes` is a cheap hardening win. Once set, never unsettable.

---

## seccomp-bpf

> Filter the *syscall interface itself*: attach a cBPF program to a process; every syscall runs through it. Allowed return codes: permit, `SECCOMP_RET_ERRNO` (fake an error), kill (`SECCOMP_RET_KILL_PROCESS`), trap, log.

```
app: read(fd,...) ──► syscall entry ──► BPF filter ──► allowed? ──► real syscall
                                        │
                                        └── denied: returns EPERM instantly,
                                            kernel code never runs
```

- Two modes: strict (only read/write/exit/sigreturn) and **filter** (`SECCOMP_MODE_FILTER`, the one everyone means); loaded via `prctl(PR_SET_SECCOMP)` or `seccomp(2)`
- **Why it matters:** the kernel has ~450 syscalls; the average server app uses ~50. Every unused syscall is unexercised attack surface; seccomp deletes it. Log4Shell-style lesson: **apps have bugs; assume breach, then make the kernel say no.** A container escape RCE that can't call `keyctl`, `bpf`, `kexec_load`, `init_module`, `ptrace`, or `mount` has most of its exploit toolkit confiscated
- **Docker's default profile** (~default.json, bundled with the engine) blocks ~44 syscalls: `keyctl/add_key/request_key` (kernel keyring exploits), `kexec_load*` (load a new kernel!), `bpf` (load programs), `mount/umount2`, `init_module`, `ptrace` (pre-4.8), `reboot`, `unshare/clone` with new namespaces (blocks nested privilege escalation)
- Real users: **Chromium** (each renderer sandboxed), **systemd** (`SystemCallFilter=` in units), Firefox sandboxes, gVisor-style userspace kernels intercept everything

---

## LSM: Linux Security Modules

> A framework of **hook points** inside the kernel (on open, exec, bind, mount, ptrace…). After DAC passes, every loaded LSM gets a veto. LSMs can only *deny*; never grant past DAC.

### SELinux vs AppArmor

| | SELinux | AppArmor |
|---|---------|----------|
| Model | **Label-based MAC** ;  every object (file, socket, process) has a security context in extended attributes | **Path-based**, profiles written against file paths/globs |
| Policy | Massive typed policy: domains × types × classes × actions | Per-application profile, simpler to read/write |
| Default on | RHEL, Fedora, CentOS, Android | Ubuntu, Debian, SUSE |
| Config lives | `/etc/selinux/`, policy in `/etc/selinux/targeted/` | `/etc/apparmor.d/` |
| Gotcha | Label survives rename/move; **copied files can get wrong labels** (`cp` vs `mv` matters!) | Renaming a binary silently breaks (or loosens) its profile |

### SELinux Mechanics

```bash
getenforce                       # Enforcing | Permissive | Disabled
sestatus                         # detail
ls -Z /var/www/html/             # contexts: user:role:type:level
ps -Z                            # process contexts
# system_u:system_r:httpd_t:s0
#    │         │       │     └ MLS level
#    │         │       └ type/domain ← the part that matters
#    │         └ role
#    └ user
```

| Mode | Behavior |
|------|----------|
| **Enforcing** | Policy applied; denials block + log |
| **Permissive** | Everything allowed; denials **logged only** ;  the debugging mode |
| **Disabled** | Off (requires reboot to re-enable; labels gone stale) |

```bash
ausearch -m avc -ts recent       # denial log entries
audit2allow -a -M myfix          # generate policy module from denials
semanage fcontext -a -t httpd_sys_content_t "/srv/www(/.*)?"
restorecon -Rv /srv/www          # relabel properly
```

**Why `chcon` quick-fixes are wrong:** `chcon` sets the label *now*, but `restorecon`/relabel-on-boot reverts it (and it papers over *why* the file had the wrong label. Fix the fcontext rule, then `restorecon`. Same for blindly feeding everything to `audit2allow`: it will happily write a rule that allows the **attack you're being alerted about**) read each denial first; many are genuine bugs (wrong label, wrong directory) not policy gaps.

---

## MAC vs DAC vs RBAC

| Model | Decides by | Example | Linux |
|-------|-----------|---------|-------|
| **DAC** | Owner-set permission bits on objects | `rwxr-xr-- alice` | Classic Unix modes, ACLs |
| **MAC** | System-wide policy, **not** changeable by owners | Type enforcement rules | SELinux, AppArmor, Smack |
| **RBAC** | Role → permission assignment | "deployer role can restart services" | sudo roles, IAM; in-kernel loosely via SELinux RBAC roles |

DAC = "my file, my rules." MAC = "admin's rules, regardless of what owners set." They stack: access requires DAC **and** every LSM to pass.

---

## How It Stacks in Containers

Defense in depth; each layer assumes the one below leaked:

```
        ┌───────────────────────────────┐
        │ App bugs exist. Assume breach.│
        ▼                               ▼
┌─────────────────────────────────────────────┐
│ 5. User namespace: container "root" = unpriv│  ← escape lands you as nobody
├─────────────────────────────────────────────┤
│ 4. LSM (SELinux/AppArmor): label/path veto  │  ← even as "root" can't touch
│    (svirt_lxc_net_t, docker-default profile)│     other containers' files
├─────────────────────────────────────────────┤
│ 3. seccomp-bpf: ~44 syscalls gone           │  ← no kexec/bpf/mount/keyctl
├─────────────────────────────────────────────┤
│ 2. Capabilities: root split, most dropped   │  ← no CAP_SYS_ADMIN/NET_ADMIN
├─────────────────────────────────────────────┤
│ 1. Namespaces + cgroups: what you see/use   │  ← isolation & limits
└─────────────────────────────────────────────┘
```

---

## Practical Commands

```bash
capsh --print                          # my capability sets, decoded
getcap $(which ping)                   # file capabilities
docker inspect -f '{{json .HostConfig}}' <c> | grep -o '"Cap[^]]*]'
grep Seccomp /proc/<pid>/status        # 0=off, 2=filter mode
ausearch -m avc -ts today              # SELinux denials
aa-status                              # AppArmor: profiles, modes
sysctl kernel.unprivileged_bpf_disabled   # 1/2 = no unpriv bpf() (eBPF was a CVE farm)
sysctl kernel.dmesg_restrict kernel.kptr_restrict
```

---

## Sources

- `man 7 capabilities`, `man 2 seccomp`, `man 8 getcap`, `man 8 selinux`, `man 8 audit2allow`
- Kernel docs: `Documentation/security/` (SELinux, AppArmor, LSM): https://docs.kernel.org/security/
- Docker default seccomp profile: https://github.com/moby/moby/blob/master/profiles/seccomp/default.json
- Chrome OS / Chromium seccomp-bpf sandbox docs: https://chromium.googlesource.com/chromium/src/+/main/docs/design/sandbox.md
- LWN: "LSM stacking" (https://lwn.net/Articles/777036/; "A seccomp overview") https://lwn.net/Articles/656067/
- National Cyber Security Centre / Red Hat SELinux guides
