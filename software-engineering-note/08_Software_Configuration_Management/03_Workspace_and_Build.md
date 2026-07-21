---
tags:
  - scm
  - build
  - integration
  - third-party
  - software-configuration-management
source: "Berczuk & Appleton, Software Configuration Management Patterns (2002), Chapters 8–10"
---

# 03 — Workspace and Build

> **Core Question:** How do you verify that individual changes integrate correctly into the whole system, and how do you manage dependencies on code you don't control?

Three patterns form the backbone of the build-and-integration discipline: **Private System Build** (build locally before check-in), **Integration Build** (centralized verification that everything holds together), and **Third Party Codeline** (track external dependencies with the same rigor as your own code).

---

## 1. Private System Build

### Problem
You make changes in your Private Workspace, insulated from others' changes. But your changes must work with the rest of the system — and you don't want to break the build for everyone else.

### Solution
**Before checking in, build the system locally using a process that mirrors the integration build as closely as possible.**

The private build should:

- Use the **same compiler, same versions of external components, and same directory structure** as the integration build.
- **Include all dependencies** and all components affected by the change.
- Differ from the product build only in minor, safe ways — e.g., it can skip packaging/installer steps, or be run from an IDE as long as the compiler is compatible.

> "Developers, test engineers, and release engineers should all use the same build tools." — Wingerd & Seiwald (1998)
>
> "If you do nothing else, make sure that every developer on a project compiles his or her software the same way, using the same tools, against the same set of dependencies." — Hunt & Thomas, *The Pragmatic Programmer*

### Key Decisions

| Decision | Guidance |
|---|---|
| **What to build** | The smallest set of components your changes affect, plus anything needed to run smoke tests. |
| **Full vs. incremental** | Do incremental builds day-to-day (faster); do a clean build when adding new files, making sweeping changes, or anytime dependency checking might be wrong. |
| **How to select what goes in** | Three approaches: (1) build everything in version control (simplest), (2) build everything except excluded items, (3) build only items explicitly listed in a release manifest. The key is that developers and the nightly build use the **same** selection rules. |
| **After building** | Update your workspace with the latest codeline and **rebuild** before check-in — someone else may have changed something in the meantime. |

### Anti-Patterns
- **Separate developer and release build procedures.** Two different systems mean two sets of bugs. A build that works locally but fails the nightly build wastes everyone's time.
- **Patching your workspace** by only rebuilding changed files and tweaking PATH/CLASSPATH — interactions you didn't anticipate will slip through.
- **Build-everything approach without a clear exclusion mechanism** discourages developers from using version control for work-in-progress code.

### What It Enables
A successful private build sets you up to run a [[Smoke Test]] and then check in with confidence. Any remaining integration issues are caught by the Integration Build.

---

## 2. Integration Build

### Problem
Individual developers cannot be 100% sure their changes integrate with everyone else's. Build environments drift — different compiler versions, different third-party component versions, different OS patches. One person's clean build is another's broken baseline.

### Solution
**Run a centralized, automated, reproducible build off checked-in code on a dedicated build machine.**

The integration build must be:

| Property | What It Means |
|---|---|
| **Reproducible** | Anyone can check out the labeled sources and produce the same artifacts. |
| **Close to the product build** | Ideally *identical*. At minimum, use the same compiler, libraries, and directory layout. |
| **Automated** | No manual intervention needed. If your VCS supports triggers, run on every check-in. |
| **Self-reporting** | Notify immediately on failure — the sooner an error is found, the easier it is to track to its source. |

### Build Frequency
- **Builds quickly** → run on every check-in (continuous integration).
- **Builds slowly** or product is static → at least a **daily** build, with the option for on-demand builds.
- **Multi-platform** → run the integration build on all supported platforms.

### The Central Principle
The integration build catches what **falls through the cracks** of private builds. Don't add more pre-checkin gates for failures that happen once — only codify recurring failures into the private build process. Label every integration build in version control so any version can be reconstructed.

### Related Patterns
- Follow the integration build with a [[Smoke Test]] to verify the build is usable.
- If the build is published as a stable baseline, also run a [[Regression Test]].

---

## 3. Third Party Codeline

### Problem
Your product depends on external components (libraries, frameworks, tools) whose release cycles are **not synchronized with yours**. You may need to customize vendor code. You still need to reconstruct any historical build — including the exact third-party versions it used.

### Solution
**Treat third-party code as a first-class codeline in your version control system.** Archive vendor releases and your customizations side by side, using branching to track their independent evolution.

### The Process

```
Vendor Release 1.0 ──→ Vendor Release 2.0
        │                      │
        ├── Your Branch R1     ├── Your Branch R2
        │   (custom fixes)     │   (merge R1 fixes + new customizations)
```

1. **Import the vendor release** into a vendor codeline directory, exactly as unpacked.
2. **Label it** with product and version identifiers.
3. **Branch immediately.** All projects that use this version work off the branch — this is where customizations live.
4. **When a new vendor release arrives**, add it to the vendor mainline, create a new branch, and **merge relevant custom changes** from the prior branch into the new one.
5. **Label the third-party codeline** with the same release label as your product release.

### What Counts as Third-Party Code?
Any code supplied by someone outside your organization, plus fixes/enhancements to that code. Plugins and extensions you write *on top of* a third-party framework are **product code**, not third-party code — they live in your main codeline.

### Special Cases

| Scenario | Approach |
|---|---|
| **Binary-only components** (no source) | Still check them into version control. You lose delta information but gain traceability. |
| **Build-time tools** (compilers, etc.) | Track in a third-party codeline only if you ship them as runtime components. Otherwise, version dependencies can be tracked via Makefile flags. |
| **Shared runtime components** (COM, system-wide DLLs) | Decide: upgrade the system-wide version, install your version alongside (via PATH/CLASSPATH isolation), or require a specific version. Multiple copies cost disk but simplify verification. |
| **Interpreted language runtimes** (Python, Perl) | Install a private copy in a special path, or embed the interpreter. Overwriting the system interpreter affects all users. |

### Why Bother?
- **Traceability:** you can answer "what version of library X shipped with release Y?"
- **Reconstructability:** checking out a release label gives you everything — your code and the third-party code it was built against.
- **Customization isolation:** vendor changes and your changes stay on separate branches, reducing merge complexity through divide-and-conquer.

---

## How They Fit Together

```
┌─────────────────────────────────────────────────────┐
│                  Third Party Codeline                │
│  (vendor code + custom branches, under version ctrl) │
└───────────────────────┬─────────────────────────────┘
                        │ provides dependencies to
                        ▼
┌─────────────────────────────────────────────────────┐
│                 Integration Build                    │
│  (centralized, automated, reproducible — daily/CI)   │
│  ← catches what private builds miss                  │
└──────────────▲──────────────────────────────────────┘
               │ each developer runs before check-in
               │
┌──────────────┴──────────────────────────────────────┐
│               Private System Build                   │
│  (local build mirroring integration build process)   │
│  ← first line of defense against breaking the build  │
└─────────────────────────────────────────────────────┘
```

1. A developer works in their **Private Workspace**, making changes.
2. Before check-in, they run a **Private System Build** — locally, using the same process as the integration build.
3. They check in. The **Integration Build** runs centrally (on every commit or nightly) to catch anything the private build missed.
4. Both builds pull dependencies from the **Third Party Codeline**, ensuring the exact right versions of external components are used — every time, for every build, past or present.

---

## Key Takeaways

- **One build process, not two.** Developer builds and release builds should use the same tools, same dependencies, same scripts. Divergence = debugging nightmares.
- **Build locally before you check in.** Catching your own mistakes costs minutes; having the whole team blocked costs hours.
- **Automate the centralized build.** Manual builds get skipped. Automated builds with immediate failure notification make problems cheap to fix.
- **Version-control everything you depend on.** Third-party code is still a dependency — if you can't rebuild an old release, you can't support it.
- **Branch vendor code; merge your changes forward.** Don't hack vendor source in place. Isolate customizations on a branch so you can merge them into future vendor releases cleanly.

## See Also

- [[Private Workspace]] — the workspace that isolates you before the private build.
- [[Smoke Test]] — run this after a successful build to verify the system is functional.
- [[Regression Test]] — required when publishing an integration build as a stable baseline.
- [[Codeline Policy]] — governs when and how changes enter the mainline.
