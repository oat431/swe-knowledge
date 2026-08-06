# Incident Response Checklist

> **Incident response (IR)** is the operating loop for when security fails: detect → triage → contain → eradicate → recover → learn. The goal isn't to never have incidents — it's to detect fast, contain fast, and get better every time.
> The master checklist ([[security]]) §10 says "incident response plan, IR practiced." This file is *how* to structure the plan, run tabletop exercises, and conduct blameless postmortems.
> Deep references: → [[04 Monitoring & Incident Response]], [[Incident-Response-Plan]] template.
> Last updated: 2026-08-07

---

## 1. What & Why

- [ ] **Core concept** — An incident is a confirmed or suspected security event that impacts confidentiality, integrity, or availability. IR is the structured process to respond. Not every alert is an incident — triage determines what escalates.
- [ ] **The IR loop** — Prepare → Detect → Triage → Contain → Eradicate → Recover → Learn → (feed back into Prepare). It's a loop, not a line. Each incident improves the next response.
- [ ] **Speed matters** — Mean Time To Detect (MTTD) and Mean Time To Contain (MTTC) are the two metrics that determine incident impact. The attacker is active from the moment they gain access; every hour of undetected presence increases damage.
- [ ] **Preparation is 80% of IR** — Most teams fail at IR not because they lack technical skill, but because they lack a plan, roles, and practice. You can't improvise an incident response under pressure.

---

## 2. The IR Plan (Before an Incident)

### Structure

- [ ] **IR plan written and accessible** — Not in someone's head, not in a draft. Written, versioned, accessible offline (the wiki may be down during an incident). → [[Incident-Response-Plan]] template.
- [ ] **Severity matrix defined** —

  | Severity | Definition | Response | Examples |
  |---|---|---|---|
  | **SEV1 (Critical)** | Active threat, data breach, production down | Page on-call immediately, assemble full team, executive comms | Confirmed breach, ransomware, prod DB exfiltrated |
  | **SEV2 (High)** | Likely threat, limited impact, needs containment | Page on-call, assemble within 1 hour | Suspected breach, single-service outage from attack |
  | **SEV3 (Medium)** | Potential issue, investigation needed | Next business hours | Anomaly alert, policy violation, unpatched critical on prod |
  | **SEV4 (Low)** | Informational, minor | Handle in queue | Failed login spike (auto-mitigated), policy drift |

- [ ] **Roles defined** — Not job titles, *incident roles*:

  | Role | Responsibility | When |
  |---|---|---|
  | **Incident Commander (IC)** | Leads the response, makes decisions, prioritizes | Every SEV1/SEV2 |
  | **Tech Lead / SME** | Investigates, contains, eradicates, recovers | Every incident |
  | **Comms Lead** | Internal updates, customer comms, regulatory notifications | SEV1/SEV2 |
  | **Scribe** | Documents timeline, decisions, actions taken | SEV1/SEV2 |
  | **Executive Sponsor** | Unblocks resources, approves extreme actions (shutdown) | SEV1 |

- [ ] **Escalation paths defined** — Who to page, in what order, with backup contacts. Not "call the security team" — call *this person*, backup is *this person*.
- [ ] **Contact list maintained** — On-call rotation, executives, legal, PR, law enforcement contact, cyber insurance provider. Reviewed quarterly (people change roles).
- [ ] **Out-of-band communication** — If the network is compromised, Slack/Teams/email may be monitored or down. Have an out-of-band channel (Signal group, phone tree).

### Pre-Authorized Actions

- [ ] **Pre-authorized containment actions** — The IC can take these without seeking approval: isolate a host, block an IP, rotate a secret, disable a user account, roll back a deployment. Waiting for approval during an incident costs time.
- [ ] **Extreme actions need approval** — Shutting down production, cutting internet access, mass password reset. These need executive sign-off (from the Executive Sponsor role).

### Legal & Regulatory

- [ ] **Breach notification obligations mapped** — Regulatory deadlines for your frameworks:
  - GDPR: 72 hours to supervisory authority
  - PCI-DSS: Immediately to acquirer
  - HIPAA: 60 days to individuals; HHS notification
  - Sector-specific: SEC (public companies), FCA (UK finance), PDPA (Thailand)
- [ ] **One owner decides "is this reportable?"** — Not a committee debate during the incident. A named role (often Legal or DPO) makes the call based on pre-defined criteria.
- [ ] **Cyber insurance contacted early** — Many policies require notification within 24–72 hours. Late notification can void coverage.

---

## 3. Detection (During Normal Operations)

> You can't respond to what you don't detect. Detection quality determines IR effectiveness.

- [ ] **Security-relevant events logged** — Failed logins, authZ denials, rate-limit hits, privilege changes, secret access, admin actions, configuration changes. Audit trail with timestamps + actor → [[04 Monitoring & Incident Response]].
- [ ] **Logs centralized** — Shipped to a central SIEM/log platform (Splunk, Elastic, Loki, Sentinel, Chronicle). Not only local disk — the attacker deletes local logs first.
- [ ] **Log integrity protected** — Append-only / WORM storage / signed. A log the attacker can edit is evidence they will delete.
- [ ] **Anomaly detection** — Alerts on: auth failure spikes, unusual data egress, new admin accounts, config drift, unexpected service-to-service calls, off-hours access.
- [ ] **Alerts tuned** — False-positive rate measured and reduced. High-noise alerts suppressed or improved, not ignored. Alert fatigue causes real alerts to be missed.
- [ ] **Detection coverage mapped** — You know which attack techniques (MITRE ATT&CK) your detections cover. Gaps are documented and prioritized.

---

## 4. Triage (First Minutes of an Incident)

- [ ] **Is this actually an incident?** — Triage determines: Is the alert real (not a false positive)? Is it security-relevant (not an ops issue)? What's the scope and severity?
- [ ] **Severity assigned** — Using the severity matrix. Start conservative (assume SEV2 until proven otherwise). Can downgrade later; upgrading after delay costs time.
- [ ] **Incident Commander assigned** — One person leads. Not a committee. The IC may not be the most technical person — they coordinate and decide.
- [ ] **Incident channel opened** — Dedicated Slack/Teams channel or bridge call. All incident comms in one place. Side conversations fragment the response.
- [ ] **Scribe starts documenting** — Timeline of events, decisions, actions taken, who said what. The scribe's log becomes the postmortem input.
- [ ] **Scope assessed** — What systems, data, users are affected? Is the attacker still active? Is the spread contained or expanding?

---

## 5. Containment (Stop the Bleeding)

> Goal: limit the blast radius. Not fix everything — just stop the attacker from doing more damage.

- [ ] **Short-term containment** — Isolate the affected host, block the attacker's IP, disable the compromised account, rotate the leaked secret. Fast, reversible actions.
- [ ] **Long-term containment** — Apply patches, remove backdoors, reset credentials across the affected system. Actions that last beyond the immediate incident.
- [ ] **Evidence preserved** — Before wiping a compromised host: memory dump, disk image, relevant logs. You may need this for forensics, insurance, or law enforcement. Destroying evidence prematurely can void insurance or legal cases.
- [ ] **Forensic image taken** — Of compromised systems, before rebuilding. Chain of custody documented (who imaged what, when, how stored).
- [ ] **Don't tip off the attacker** — If the attacker doesn't know they're detected, you can observe their behavior and understand the full scope. Aggressive containment before understanding scope can cause the attacker to burrow deeper or trigger destructive actions.

---

## 6. Eradication & Recovery

- [ ] **Root cause identified** — Not just "the attacker exploited CVE-X" but *how* they got to the point where CVE-X was exploitable: unpatched system, exposed service, compromised credential, phishing. Fix the root cause, not just the symptom.
- [ ] **Attacker access removed** — All backdoors, persistence mechanisms, and unauthorized accounts removed. Not just the one you found first — assume there are more.
- [ ] **Systems rebuilt from known-good** — Don't patch in place on a compromised host. Rebuild from a known-good image. Patching over malware leaves uncertainty.
- [ ] **Credentials rotated** — All credentials that the attacker could have accessed are rotated. Not just the confirmed leaked one — the entire blast radius.
- [ ] **Validation before recovery** — Before bringing systems back online: the vulnerability is patched, the backdoor is removed, the monitoring is in place to detect a return. Recovery without validation invites re-compromise.
- [ ] **Gradual recovery** — Bring systems back incrementally with monitoring. Watch for the attacker returning. A sudden full recovery can miss re-infection.

---

## 7. Postmortem (Learning)

> The incident isn't over when systems are back. It's over when the team has learned and improved.

- [ ] **Blameless postmortem** — Focus on *what* went wrong and *how the system* failed, not *who* is to blame. Blame suppresses honesty; honesty is required for learning → pattern in [[Release]] §10.
- [ ] **Timeline reconstructed** — From alert to containment to recovery. What happened, when, what was decided, what was done. Based on logs and the scribe's notes.
- [ ] **Root cause analysis** — The chain of failures that allowed the incident. Not just the proximate cause (the exploit) but the enabling conditions (unpatched, unmonitored, no detection).
- [ ] **What went well** — Detections that fired, decisions that helped, controls that held. Positive findings validate existing investments.
- [ ] **What went poorly** — Detection gaps, communication failures, missing runbooks, slow containment. These are improvement opportunities, not blame.
- [ ] **Action items with owners and dates** — Every action item has a named owner and due date. Tracked to completion. An action item without an owner is a wish.
- [ ] **Action items verified** — Not just "done" — verified. A new detection is tested against the original attack pattern. A patched system is scanned. A new runbook is exercised.
- [ ] **Postmortem shared** — Within the team and relevant stakeholders. Not buried. Transparency builds trust and spreads learning.

---

## 8. Tabletop Exercises (Practice Before the Real Thing)

> You can't improvise incident response under pressure. Tabletop exercises build the muscle memory.

- [ ] **Annual tabletop minimum** — At least once per year, the team walks through a simulated incident. At least quarterly for production-grade and mission-critical.
- [ ] **Scenario is realistic** — Based on your threat model and real incidents in your industry. Not a generic "you got hacked" — a specific scenario: "an employee clicked a phishing link and their OAuth tokens were stolen."
- [ ] **Roles assigned** — Participants play their actual IR roles (IC, Tech Lead, Comms, Scribe). Tests whether people know their role.
- [ ] **Injects introduce complications** — Mid-exercise, the facilitator adds: "The attacker just deleted the SIEM logs" or "Legal says GDPR 72-hour clock starts now." Tests adaptability.
- [ ] **Lessons documented** — What the team did well, what was missing (runbook, contact, detection, authority). Action items created and tracked.
- [ ] **Detection gaps fed to detection engineering** — If the team couldn't detect the attack in the scenario, that's a detection gap to fix.

---

## 9. Anti-Patterns to Avoid

- [ ] **No IR plan** — "We'll figure it out when it happens." Under pressure, without roles or runbooks, the response is chaotic, slow, and incomplete.
- [ ] **Plan exists but never tested** — The IR plan is a 50-page document. Nobody has read it since it was written. The on-call rotation changed. The contact list is stale.
- [ ] **No pre-authorized actions** — Every containment action needs approval, which needs a meeting, which needs the executive who's in a flight. The attacker has hours of free time.
- [ ] **Wiping evidence prematurely** — The compromised host is rebuilt immediately. No memory dump, no disk image. Forensics, insurance, and legal cases are compromised.
- [ ] **Blame-focused postmortem** — "Who clicked the link?" suppresses honesty. The real question is "why was a single click able to compromise the system?" — that's a system failure, not a human failure.
- [ ] **Action items not tracked** — The postmortem identified 10 improvements. Nobody owns them. Six months later, the same incident happens again.
- [ ] **Alert fatigue** — The SIEM generates 10,000 alerts/day. The team ignores them. The real alert is buried in noise.
- [ ] **Assuming the attacker is gone** — The system is back online, so the incident is over. The attacker left a persistence mechanism and returns next week.
- [ ] **No out-of-band comms** — The incident channel is on Slack. The attacker has access to Slack (via stolen token). They can read your response.
- [ ] **Cyber insurance notified too late** — The policy required notification within 48 hours. The team handled it internally for a week. Coverage voided.

---

## Quick Sanity Check

- [ ] IR plan written, versioned, accessible offline; severity matrix + roles defined
- [ ] Pre-authorized containment actions documented; extreme actions need exec approval
- [ ] Contact list maintained (on-call, execs, legal, PR, insurance) — reviewed quarterly
- [ ] Breach notification obligations mapped; one owner decides "reportable?"
- [ ] Security logs centralized, tamper-resistant, with anomaly detection
- [ ] Tabletop exercise run in the last 12 months (quarterly for prod-grade)
- [ ] Postmortem process is blameless; action items have owners, dates, and verification
- [ ] Out-of-band communication channel exists (not just Slack/Teams)

---

## Sources

- Master checklist: [[security]] §10.
- Deep references: [[04 Monitoring & Incident Response]], [[Incident-Response-Plan]], [[Digital-Forensics-Report]] templates.
- Standards: NIST SP 800-61 (Computer Security Incident Handling Guide), SANS Incident Handler's Handbook, MITRE ATT&CK.
