---
tags:
- linux
- os
- programming
- async
---

# 06 I/O Models & Async

How a program waits for data that isn't there yet — and why epoll/io_uring are the reason a single Nginx worker handles 50k connections.

---

## The 5 I/O Models

> Classic split (Stevens, *UNIX Network Programming*): the waiting phase and the copying phase vary between models.

| Model | Wait for data | Copy data | Typical API | Verdict |
|-------|---------------|-----------|-------------|---------|
| **Blocking** | Blocks | Blocks | `read()`, `recv()` | Simple, 1 thread per connection |
| **Non-blocking polling** | Returns `EAGAIN` immediately | Blocks | `read()` + `O_NONBLOCK` | Busy-loop burns CPU |
| **I/O multiplexing** | Blocks in `select`/`poll`/`epoll_wait` | Blocks | `epoll_wait()` + `read()` | The workhorse: Nginx, Redis, Node |
| **Signal-driven** | Async via `SIGIO` | Blocks in handler | `sigaction`, `F_SETOWN` | Rarely used — signals are awful |
| **True async (AIO)** | Async | Async (kernel copies) | POSIX AIO, `io_uring` | POSIX AIO half-broken; io_uring is the real deal |

---

## The Blocking read() Path

Every `read()` from a socket costs three things — see [[01 System Calls & Kernel]]:

1. **Syscall overhead** — user→kernel mode switch (~100ns+, more with Spectre mitigations)
2. **Context switch** — thread sleeps in `sk_wait_queue`, scheduler picks another thread
3. **Data copy** — NIC → kernel socket buffer → user buffer (a full memcpy)

```
App: read(fd, buf, 4096)
  │ syscall (mode switch)
  ▼
Kernel: no data yet? → schedule() → thread sleeps
        data arrives (NIC → DMA → socket buffer) → wake_up()
        copy_to_user(kernel_buf → user_buf)
  ▼
App: read returns 4096
```

One blocking read is cheap. **10,000 threads each blocking on one read is not** — thread stacks, scheduler overhead, cache thrashing. That's the C10K problem.

---

## Non-blocking Polling — Why Busy-Loops Lose

```c
fcntl(fd, F_SETFL, O_NONBLOCK);
while (1) {
    n = read(fd, buf, sizeof buf);
    if (n == -1 && errno == EAGAIN)
        continue;   // ← 100% CPU, zero progress
}
```

Non-blocking I/O alone means "return `EAGAIN` instead of sleeping." Spinning on it wastes a full core. It's only useful combined with a multiplexer that tells you *when* to actually read.

---

## select / poll — First Generation

| Limitation | select | poll |
|------------|--------|------|
| Max fds | `FD_SETSIZE` = 1024 (compile-time) | Unlimited (array of `pollfd`) |
| fd passing | Full `fd_set` **copied into kernel every call** | Same, array copied every call |
| Kernel scan | **O(n)** over every fd, every call | **O(n)** |
| Result | Bitmask — you must **O(n) scan** to find ready fds | `revents` flags, still scan all |
| Edge notification | No — re-arms level state each call | No |

At 100k connections with 10 active, select/poll do 100k units of work per event loop iteration. `epoll` does ~10.

---

## epoll — The Reason Nginx/Redis/Node Scale

Three syscalls (Linux 2.5.44+):

```c
int epfd = epoll_create1(0);                 // create instance (its own fd)
struct epoll_event ev = { .events = EPOLLIN, .data.fd = conn_fd };
epoll_ctl(epfd, EPOLL_CTL_ADD, conn_fd, &ev); // register ONCE — kernel keeps it
struct epoll_event ready[MAX_EVENTS];
int n = epoll_wait(epfd, ready, MAX_EVENTS, -1); // blocks until something happens
for (int i = 0; i < n; i++)
    handle(ready[i].data.fd);                // only ready fds returned
```

**Why O(1)-ish:**
- fds registered **once** via `epoll_ctl` — no per-call copying of the whole interest set
- kernel maintains a **readiness list** (linked list of fired fds); device wake callbacks push fds onto it
- `epoll_wait` returns *only* ready fds — no scanning of 100k idle connections

### Level vs Edge Triggered

| Mode | Fires when | Read loop | Used by |
|------|------------|-----------|---------|
| **Level (default)** | Ready *while* data remains unread | Read once is fine; re-fires next wait | Redis, libevent default |
| **Edge (`EPOLLET`)** | Only on *state transition* (new data arrives) | Must drain until `EAGAIN` | Nginx |

Edge = fewer wakeups, but forget to drain the buffer and the event never re-fires → hung connection. Classic footgun.

**kqueue** is the BSD/macOS equivalent (`kqueue`/`kevent`, one syscall does both ctl and wait) — libev/libevent/Go's netpoller abstract both behind one API.

---

## Reactor vs Proactor

| | Reactor | Proactor |
|---|---------|----------|
| Notifies on | **Readiness** ("you can read now") | **Completion** ("read is done, data's in your buffer") |
| Who does the I/O | You, after the event | The kernel/OS, before the event |
| Examples | epoll/kqueue/select | Windows IOCP, POSIX AIO, io_uring |
| Built on it | Node/libuv, Netty, Redis, Nginx, libevent | SQL Server, io_uring-based runtimes (Tokio's io_uring forks, ScyllaDB) |

**Reactor pattern:** single-threaded event loop + registered callbacks.

```
        ┌──────────────────────────────────┐
        │            Event Loop            │
        │  epoll_wait ──► dispatch ──► cb  │
        └───────▲──────────────────────────┘
                │ register interest
   ┌────────┬───┴────┬─────────┐
 acceptor  handler  timer   signal handler
```

Node.js = libuv wrapping epoll + a thread pool (for file I/O and DNS, which epoll can't do — regular files are *always* "ready"). Netty = Java NIO Selector (epoll under the hood). Redis = ae event loop, single-threaded, which is why one `KEYS *` blocks everyone.

---

## io_uring — True Async, Finally (kernel 5.1+, usable 5.6+)

> Two shared-memory ring buffers between user and kernel: submissions (SQ) and completions (CQ). The ring *is* the interface — syscalls are optional.

```
User space                     Kernel
┌──────────────┐              ┌──────────────┐
│  SQ entries  │──(io_uring_enter, or none)──►│  workers poll SQ │
│  (write req) │              │  do the I/O  │
│  CQ entries  │◄────────────── (post result)│  (read/poll/etc)│
│  (completion)│              └──────────────┘
└──────────────┘
```

| Feature | Why it matters |
|---------|----------------|
| **Shared ring buffers (mmap)** | Submission = write struct to memory. `SQPOLL` mode: **zero syscalls** — kernel thread consumes the ring |
| **Batching** | One `io_uring_enter` submits hundreds of ops |
| **Completion events** | Proactor semantics: result + data delivered to CQ |
| **Works on regular files** | epoll can't — files are always "ready". io_uring does real async file I/O |
| **Fixed buffers / registered files** | Skip per-op buffer mapping — big win at high IOPS |
| **Linked ops** | Chain read→write in one submission |

Adopters: RocksDB, ScyllaDB, Postgres 17+ (async I/O), Tokio ecosystem, `cp`/`mv` in coreutils. Caveat: it has been a major CVE source (Google disabled it in Android/ChromeOS for a while) — huge, fast-moving attack surface.

---

## Zero-Copy

`sendfile(out_fd, in_fd, ...)` — file → socket **without the data ever entering user space**:

```
Classic read+write (4 copies, 2 syscalls):
disk →DMA→ page cache →CPU→ user buf →CPU→ socket buf →DMA→ NIC

sendfile (Linux 2.4+ with scatter-gather NICs):
disk →DMA→ page cache ────────────(descriptor only)──► NIC DMA
   = 0 CPU copies
```

`splice()` generalizes it via pipes between any two fds. Users: Nginx (`sendfile on;`), Kafka (topic = log files sent via sendfile), Java `FileChannel.transferTo`. See [[02 I O & Devices]] for DMA basics.

---

## Practical

```bash
# Watch a process multiplex
strace -f -e trace=epoll_wait,epoll_ctl -p <PID>
strace -c ./my_server          # count syscalls — read() dominating? wrong model

# epoll limits
cat /proc/sys/fs/epoll/max_user_watches   # default ~4% of RAM worth

# Prove io_uring works on your kernel
uname -r                        # ≥ 5.1, realistically ≥ 5.6/5.10
```

```python
# Python's selectors module — epoll under the hood, no C required
import selectors, socket
sel = selectors.DefaultSelector()   # → EpollSelector on Linux
srv = socket.socket(); srv.bind(('', 8080)); srv.listen(128)
sel.register(srv, selectors.EVENT_READ, data='accept')

while True:
    for key, mask in sel.select():   # blocks in epoll_wait
        if key.data == 'accept':
            conn, _ = key.fileobj.accept(); conn.setblocking(False)
            sel.register(conn, selectors.EVENT_READ, data=conn)
        else:
            data = key.fileobj.recv(4096)
            if data: key.fileobj.send(data)
            else: sel.unregister(key.fileobj); key.fileobj.close()
```

> Socket-level API detail (TCP states, backlog, `SO_REUSEPORT`) lives in [[031 Network Programming & Sockets]].

---

## Sources

- `man epoll(7)`, `man epoll_ctl(2)`, `man select(2)`, `man sendfile(2)`
- Kernel docs: `Documentation/filesystems/io_uring.rst`, io_uring docs — https://kernel.dk/io_uring.pdf
- LWN: "The io_uring subsystem" — https://lwn.net/Articles/810411/
- LWN: "Who's afraid of io_uring?" — https://lwn.net/Articles/951883/
- Stevens, Fenner, Rudoff. *UNIX Network Programming, Vol 1*, Ch. 6 (I/O models).
- The C10K problem — http://www.kegel.com/c10k.html
