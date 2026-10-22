---
title: "Reviewing AI Generated Code"
role: Software Engineer
capability_area: AI Assisted Engineering
topic: Reviewing AI Generated Code
status: complete
created: 2026-10-21
updated: 2026-10-21
tags:
  - career-path
  - software-engineer
  - ai-assisted-development
  - code-review
---

# Reviewing AI Generated Code

> **Core skill:** Verifying correctness, security, and style of generated changes

## What Changes from Junior to Mid

A junior engineer reviews generated code by reading it for fluency: it looks clean, the names are sensible, it must be right. A mid-level engineer treats generated code as untrusted input: the same category as a patch from an unknown contributor or data from a user. Fluent style is precisely what makes generated defects dangerous, so review runs against the spec, the tests, and the codebase's real conventions, not against how the code feels.

| Aspect | Junior | Mid |
|--------|--------|-----|
| Default stance | Code that reads well is probably right | Generated code is untrusted input until verified |
| Review basis | Does it look clean? | Does it satisfy the spec and the tests? |
| Assumptions | Takes stated behavior at face value | Traces claims against the actual codebase |
| Edge cases | Checks the happy path | Checks boundaries, errors, and empty inputs |
| Verdict | Accept or regenerate at random | Accept, fix forward, or reject wholesale with a stated reason |
| Review output | A merged diff | An explanation you can defend line by line |

## Core Frameworks

### The Review Checklist

| Axis | What to Check | Red Flags |
|------|---------------|-----------|
| Correctness vs spec | Every acceptance criterion is met, not just the headline behavior | Behavior matches the prompt's wording but not the requirement behind it |
| Edge cases | Empty inputs, boundaries, nulls, zero, negative, overflow, unicode | Only the happy path is handled; assumptions about input shape |
| Error handling | Failures are caught where they can be handled; no silent swallowing | Bare except blocks, ignored promise rejections, empty catch blocks |
| Security | Input validation, authz checks on every path, no injection or unsafe deserialization | String-built queries, missing permission checks, permissive defaults |
| Style and conventions | Naming, layering, file layout match the surrounding code | Correct code that belongs in a different codebase |
| License and provenance | No copied code with incompatible terms, no removed attribution | Verbatim blocks of recognizable third-party code, missing headers |
| Scope | Nothing changed outside the declared scope | Sneaky refactors, renamed files, drive-by dependency bumps |

### Plausible-but-Wrong Patterns

| Pattern | Why It Looks Fine | How to Catch It |
|---------|-------------------|-----------------|
| Hallucinated APIs | Method names are perfectly idiomatic for the ecosystem | Compile or type-check; open the dependency's actual docs |
| Wrong assumptions about your codebase | Code matches generic examples, not your layering | Check it against your real modules and conventions |
| Correct algorithm, wrong contract | The logic is textbook-perfect for a subtly different problem | Trace against the spec sentence by sentence |
| Invented requirements | Extra features or config options you never asked for | Diff against the scope boundary in the brief |
| Tests that restate the implementation | They pass by construction and prove nothing | Ask what defect each test would catch |
| Concurrency errors | Structure looks right in single-threaded review | Reason about interleavings; run under load or race detectors |
| Copy-paste of a known-vulnerable pattern | It mirrors code seen in countless tutorials | Check against current secure-coding guidance, e.g. OWASP guidance on AI-generated code risks |

### Reject Wholesale vs Fix Forward

| Situation | Decision | Rationale |
|-----------|----------|-----------|
| Core approach contradicts the spec or your architecture | Reject wholesale | Fixing forward on a wrong design bakes in the wrong design |
| You cannot explain why a section exists | Reject that section | Unexplainable code does not ship under your name |
| Right approach, local defects in details | Fix forward | The frame is sound; the gaps are reviewable one by one |
| Small convention drift in mostly good output | Fix forward | Cheaper to adjust than to regenerate and re-review |
| Output invents APIs or behaviors you cannot verify | Reject wholesale | Unverifiable code is unowned code |
| Second attempt repeats the same defect class | Stop and fix the brief | The context or spec is wrong, not the model's luck |

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Read the diff against the spec"] --> B["Check edge cases and errors"]
    B --> C["Check security and license provenance"]
    C --> D["Check style and scope"]
    D --> E["Decide accept, fix forward, or reject"]
    E --> F["Verify with tests and checks"]
    F --> G["Explain every accepted line in review"]
```

## In Practice

**Treat generated code as untrusted input.** It arrives fluent, formatted, and confident, and none of those qualities correlate with correctness. Review it with the same rigor you would apply to a patch from an anonymous contributor: verify behavior against the spec, not against the tone of the code. Fluency is a delivery mechanism, not evidence.

**Review against the spec, not against the prompt.** The prompt is what you asked for; the spec is what the system needs to do. Generated code optimizes for the literal request, and a literal request can be subtly wrong. Keep the acceptance criteria open in a second window and check each one as a separate claim.

**Assume the codebase assumptions are wrong until traced.** Agents guess at your layering, your error conventions, and your data shapes when those are not in context. Open the real modules the code touches and confirm the calls, types, and boundaries match. A call that would be correct in a stock project can be nonsense in yours.

**Check edge cases and error paths before polishing style.** The happy path is what generation handles best; the failures are where it quietly drops work. Look for swallowed exceptions, missing null handling, and boundary conditions like empty collections and zero values. Style fixes are cheap later; a missing error path is a production incident.

**Run a specific pass for security and license provenance.** Look for injection-prone string construction, missing authorization checks on new paths, unsafe deserialization, and secrets in code. Separately, check that any block resembling known third-party code carries compatible terms and attribution. OWASP guidance on AI-generated code risks is a sound checklist source for this pass.

**Decide explicitly between fix-forward and reject.** When the approach is wrong, patching details wastes review effort and ships a design you disagree with. When the approach is right and details are off, regenerating from scratch throws away good work. Name which situation you are in before you start editing; that decision belongs to you, not to habit.

**Explain accepted code in review as if you wrote it.** For every line you accept, be able to state what it does and why it belongs. If a colleague asks "why this approach?" in review and your answer is "the model produced it," the change is not ready to merge. Understanding is part of acceptance, not a later chore.

## Practical Exercise

Review a generated change as untrusted input, end to end:

1. Take a real generated patch, or generate one deliberately for a small feature with a written spec and acceptance criteria.
2. Print or open the diff and read it against the spec first, marking each acceptance criterion met, unmet, or unverifiable.
3. Make a second pass for edge cases: empty inputs, boundaries, nulls, and error paths; write down every case the patch does not handle.
4. Make a third pass for security and provenance: injection surfaces, authz on new paths, and any block that resembles third-party code.
5. Check conventions and scope: naming, layering, and nothing changed outside the declared boundary.
6. Decide per defect: fix forward or reject wholesale, with one sentence of rationale for each major decision.
7. Verify with tests you trust (including at least one test you wrote yourself that the generated code did not produce), then write the review comment explaining the accepted code's approach in your own words.

## Practical Applications

- [ ] Generated diffs are read against the spec and acceptance criteria, never on fluency
- [ ] Every API call in generated code is confirmed against the real dependency
- [ ] Edge cases and error paths get a dedicated review pass
- [ ] A security pass covers injection, authz, and unsafe patterns before merge
- [ ] License provenance is checked for anything resembling copied code
- [ ] Scope drift outside the brief is caught and removed
- [ ] I can explain every accepted line and the chosen approach in review
- [ ] At least one test I wrote myself verifies the generated behavior

Review notes template:

```text
# Review: <change or task name>

## Spec check
<each acceptance criterion: met / unmet / unverifiable, with evidence>

## Edge cases and errors
<cases found missing or handled; cases verified>

## Security and provenance
<injection, authz, secrets, license concerns, or "none found" with what you checked>

## Conventions and scope
<drift found; anything out of scope removed>

## Verdict
<accept / fix forward / reject wholesale, one sentence of rationale>

## Explanation
<the approach in my own words, ready for the PR description>
```

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| Judging quality by how clean it reads | Fluent style is exactly what hides defects | Review against spec and tests |
| Trusting generated tests to validate generated code | Both can share the same wrong assumption | Write at least one independent test yourself |
| Skipping error-path review | Generation handles happy paths best | Dedicated pass for failures and boundaries |
| Calling APIs from memory | Hallucinated methods compile nowhere and wrong overloads compile everywhere | Compile and open the real docs |
| Patching a wrong design until it "works" | You ship an architecture nobody chose | Reject wholesale when the approach is wrong |
| Merging code you cannot explain | Ownership is not transferable to a model | Understand, rewrite, or delete |
| Reviewing only the diff hunks | Assumptions live in unchanged surrounding code | Trace the touched call paths in context |

## Success Indicators

- You catch hallucinated APIs and wrong codebase assumptions before the build does
- Your review comments name spec mismatches, not just style nits
- You have rejected a plausible-looking patch wholesale and can say why
- Every AI-assisted merge passes your security and license checklist
- You can explain the approach of any accepted generated change without notes
- Colleagues ask you to review AI-assisted changes because you find what skims miss

## Related

- [[00_overview]]
- [[01_Working_With_Coding_Agents]]
- [[04_AI_Security_Licensing_and_Privacy_Risks]]
- [[07_An_AI_Assisted_Daily_Workflow]]
- [[career-path/01_Software_Engineer/02_Daily_Engineering_Craft/00_overview|Daily Engineering Craft]]
- [[career-path/02_Senior_Software_Engineer/05_Quality_Reliability_Security/00_overview|Quality, Reliability and Security]]

## Sources

- *Software Engineering at Google* (Titus Winters, Tom Manshreck, Hyrum Wright)
- *The Pragmatic Programmer* (David Thomas, Andrew Hunt)
- *Working Effectively with Legacy Code* (Michael Feathers)
- OWASP guidance on AI-generated code risks (owasp.org)
