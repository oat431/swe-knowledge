# Privacy & Data Protection Checklist

> Framework-agnostic checklist for privacy-by-design and data-protection compliance (Thai PDPA, EU GDPR, and similar regimes).
> Applies to any product that handles personal data — web, mobile, API, or bot.
> Last updated: 2026-11-15 (created)

---

## How to Use This Checklist

- Run it twice: once at design time (§1–§2, §11 drive the feature's privacy shape) and once before release (the Quick Sanity Check is the gate).
- Where a line cites a statute, the citation is the floor, not the ceiling — local implementing rules (PDPC notifications, member-state derogations, US state regulations) layer on top; verify current wording at the regulator before legal reliance.
- Companion files: [[security]] owns the security controls this checklist leans on; [[data]], [[bot]], [[ops]], [[mobile]], [[web]], [[qa]] own their domain-specific facets.

---

## 1. Data Inventory & Purpose
> **Note:** this section is the backbone — §3 (rights), §4 (retention), and §7 (transfers) all fail gracefully only when the inventory is right. Budget the most time here.

- [ ] **Personal data inventory complete:** every field of personal data you collect, generate, or infer is cataloged (identity, contact, behavioral, financial, location); shadow collections (debug flags, analytics events) included.
- [ ] **Data flows mapped:** where personal data travels — services, databases, third parties, backups, warehouses, logs, support tools; drawn as a flow diagram, not prose → [[security]] §8.
- [ ] **ROPA maintained (living record of processing activities):** per processing purpose: data categories, subjects, recipients, retention, security measures, transfer mechanism (PDPA s.39-style records; GDPR Art. 30). Updated when processing changes, reviewed at least yearly.
- [ ] **Lawful basis recorded per purpose:** consent vs contract vs legitimate interest vs legal obligation — one basis per purpose, decided before build, not retrofitted after launch.
- [ ] **Purpose limitation stated up front:** the privacy notice declares each purpose at collection time; new purpose = new review (and usually new consent); no silent scope creep.
- [ ] **Legitimate-interest basis documented:** where used, a short balancing test exists (purpose, necessity, impact on the data subject, safeguards) — not just the word "LIA" on a slide.
- [ ] **Inventory refresh triggers defined:** new vendor, new data field, new market, new regulation — each triggers a ROPA update the same way architecture changes trigger threat-model refresh → [[security]] §1.
- [ ] **High-risk processing flagged in the inventory:** sensitive data, large-scale profiling, children's data, and cross-border flows are tagged so §9 and the DPIA trigger (§11) can find them mechanically.

## 2. Consent & Notices
> **Note:** consent is a product surface — test the withdrawal path like you test the signup path; regulators ask to see it work, not to read about it.

- [ ] **Consent is informed, granular, and freely given:** one toggle per purpose (marketing ≠ analytics ≠ essential); no bundling with terms acceptance; no service denial for refusing non-essential processing.
- [ ] **Consent is recorded:** who consented, when, to what version of which notice; stored queryably — "we have consent" without an audit trail fails an audit.
- [ ] **Withdrawal is as easy as opt-in** (PDPA s.19 words it exactly this way — "as easy as to giving consent"; GDPR Art. 7(3) equivalent): one click from the same place; honored in every downstream system within the same SLA as collection.
- [ ] **Withdrawal consequences explained** where withdrawal affects service (PDPA s.19); withdrawal never retroactively invalidates prior lawful processing.
- [ ] **Privacy notice in plain language at every collection point:** what is collected, why, legal basis, retention, recipients, rights, contact channel (PDPA s.23 notice content; GDPR Arts. 13–14); Thai/local-language where the audience needs it.
- [ ] **Cookie/tracking consent where applicable:** non-essential cookies and trackers off until opt-in; consent banner has equal-weight accept/reject (no dark patterns — no hidden reject, no forced scrolling, no pre-ticked boxes).
- [ ] **Consent obtained through deceit or misleading framing is treated as a defect** (PDPA s.83 explicitly fines obtaining consent by deceiving or misleading about purposes) — notices match actual behavior.

## 3. Data Subject Rights
> **Note:** treat rights requests as an incident-grade workflow with an owner and a clock — an ignored request is itself a compliance breach.

- [ ] **Rights workflows exist and are tested:** access, correction, erasure, portability, objection, restriction — each with a documented intake channel, owner, and runbook (GDPR Ch. III, Arts. 15–22; PDPA ss.30–36).
- [ ] **Response SLA enforced:** without undue delay and within one month of receipt (GDPR Art. 12 — extension of two further months for complex requests, with reasons); PDPA s.30: without delay, not exceeding 30 days for access requests.
- [ ] **Identity verification step:** requesters verified proportionally before disclosure (no account enumeration during verification); verification data itself minimized and deleted after the request closes.
- [ ] **Erasure reaches every copy:** primary DBs, read replicas, warehouses, analytics tools, backups (cascade delete, suppression list, or documented backup-expiry strategy) → [[data]]; "deleted" in the app but alive in the warehouse is the classic failure.
- [ ] **Erasure vs retention obligations documented:** where law or contract requires keeping data (tax, anti-fraud), the basis is written down and the data is restricted, not silently retained.
- [ ] **Automated-decision opt-out where relevant:** meaningful information about the logic, human review on request where decisions have legal or similarly significant effects (GDPR Art. 22-style; per applicable law) — verify applicability for your feature.
- [ ] **Rights request log maintained:** request, identity check, action, response date — evidence for regulators, and the input for your SLA dashboard.

## 4. Collection & Retention
> **Note:** retention is where most programs quietly rot — the schedule must be executable code (jobs), not just policy text.

- [ ] **Data minimization enforced at the form/API level:** every field justified by a stated purpose ("might need it later" is not a purpose); optional fields rare and deliberate.
- [ ] **Retention schedule per data class:** defined max age (e.g. account data = life of account + N months; logs = N days), owner per class, and the legal/business reason.
- [ ] **Deletion jobs actually run:** scheduled purge is deployed, monitored, and alerting on failure; a retention policy without a running job is fiction.
- [ ] **Analytics uses anonymized or pseudonymized data:** user IDs hashed/keyed, IP truncated or dropped, no raw identifiers in event streams; pseudonymization keys access-controlled separately.
- [ ] **No PII in logs, debug dumps, crash reports, or tracking events:** structured logging redacts; test by grepping prod logs for known test values → [[security]] §4; the debug log is the most common accidental disclosure.
- [ ] **Non-production uses masked or synthetic data** — never copied production DBs with real PII → [[security]] §8.
- [ ] **Backups respect retention:** the deletion job's story includes backups (purge, re-encrypt, or expiry-based overwrite) and the gap is documented — a deleted user resurrected by a restore is a reportable event.

## 5. Storage & Access Security
> **Note:** privacy-specific hardening here; the general security baseline lives in [[security]] §8 — run both.

- [ ] **Encryption at rest on all PII stores:** databases, object storage, backups, exports; managed keys (KMS), keys separated from data.
- [ ] **Encryption in transit:** TLS everywhere (→ [[security]] §6); application-level encryption for the most sensitive fields where threat warrants it.
- [ ] **Least-privilege access to PII:** role-based, column/table-level restrictions where the store supports it; shared accounts forbidden; access reviewed periodically.
- [ ] **PII access audit-logged:** who read/changed/exported personal data, when, from where; logs tamper-resistant and monitored for anomalies (mass export, off-hours access).
- [ ] **Secrets hygiene:** no credentials, API keys, or tokens in code or configs; store in vault/KMS; scanning in CI → [[02 Secrets Management]].

## 6. Breach Response
> **Note:** the notification clock starts at *awareness* by anyone in the chain, not at root-cause certainty — rehearse the first hour. Details in [[incident-response]].

- [ ] **Detection → triage → impact assessment runbook exists:** what counts as a personal-data breach (including confidentiality loss, not just exfiltration), who decides severity, what evidence is preserved.
- [ ] **Regulatory notification within the statutory deadline:** GDPR Art. 33: without undue delay, where feasible within 72 hours of awareness (with reasons if late); PDPA s.37(4): notify the PDPC Office without delay and, where feasible, within 72 hours — confirm current filing mechanics at pdpc.or.th before relying on it.
- [ ] **Affected users notified when risk is high:** GDPR Art. 34 (without undue delay); PDPA s.37(4) requires notifying data subjects without delay when the breach is likely to result in high risk — message drafted with the same care as the incident fix.
- [ ] **Processor→controller breach relay:** contracts obligate processors to notify you without undue delay (GDPR Art. 33(2) on the processor); clock starts when anyone in the chain becomes aware.
- [ ] **Breach register maintained:** every incident (notified or not) with dates, data involved, decision rationale for non-notification — regulators ask for the register first.
- [ ] **Drills done before they're needed:** tabletop exercise at least yearly, notification templates pre-approved → [[incident-response]], [[ops]].

## 7. Cross-Border Transfers
> **Note:** "the cloud chose the region" is not a transfer decision — every border crossing needs a named mechanism.

- [ ] **Transfer mechanism per destination documented:** adequacy-style recognition where it exists, standard-contractual-clause-style contracts as the workhorse, consent-based transfers only as a last resort with explicit notice of the weaker protection (PDPA s.28 lists exactly these exceptions).
- [ ] **GDPR Chapter V covered where EU data is in scope:** Art. 44–46-style safeguards; transfer impact assessment where the destination's laws may undermine the contract (post-Schrems-II practice).
- [ ] **PDPA s.28–29 satisfied for Thailand-outbound transfers:** destination has adequate protection per the Committee's rules; binding corporate rules (s.29) documented if your group relies on them.
- [ ] **Data localization checked per sector:** financial, health, government-adjacent workloads may have hosting restrictions — verify for your industry and markets before choosing a region.
- [ ] **Processor hosting map maintained:** which vendor stores/processes data in which country (including support access from other countries) — "it's a global SaaS" is not a transfer mechanism.
- [ ] **Government-access and onward-transfer exposure assessed:** where a destination's law permits authority access incompatible with the regime, supplementary measures (encryption with keys held elsewhere, minimization) or a different region is chosen — and the decision is written down.

## 8. Third Parties & SDKs
> **Note:** processors inherit your obligations but not your control — contract for the gap and review what actually ships (SDK traffic beats privacy-policy prose).

- [ ] **Data processing agreement with every processor:** purpose limitation, security measures, sub-processing terms, breach notification, deletion on termination (GDPR Art. 28-style; PDPA s.40 processor duties); no DPA, no data.
- [ ] **Sub-processor list maintained and changes notified:** current list published or tracked; contract requires notice and objection rights on change.
- [ ] **Vendor/SDK privacy review before integration:** what data the SDK sends, to whom, on what trigger; mobile SDKs silently exfiltrating data is a real, recurring failure mode → [[mobile]]; review on every SDK version bump.
- [ ] **Ad-tech and trackers scrutinized per purpose:** pixels, session replay, A/B tools, chat widgets each need a lawful basis and a place in the inventory (§1); disable anything without one.
- [ ] **Vendor breach obligations tested:** contractual notification deadline (shorter than yours if possible), evidence/audit rights, and named contacts — confirmed with the vendor, not assumed from their trust page.
- [ ] **Offboarding deletes data:** termination SLA for processor deletion/anonymization with written confirmation kept as evidence.
- [ ] **No purchased or scraped personal data without a provenance and lawful-basis check:** if the seller can't show how the data was collected and consented, it's a liability, not a lead list.

## 9. Sensitive & Children's Data
> **Note:** the default answer here is "don't collect it"; explicit consent is the minimum, not the justification.

- [ ] **Sensitive personal data requires explicit consent:** health, biometric, genetic, racial/ethnic, political, religious, sexual-life data (PDPA s.26 explicit-consent rule; GDPR Art. 9 special categories similar in effect — verify specifics per regime); plus a written necessity justification before collecting it at all.
- [ ] **Extra security proportional to sensitivity:** stronger access control, encryption, masking in UIs, and audit depth for sensitive classes (§5 applies with the dial turned up).
- [ ] **Children's data: parental-consent thresholds respected:** PDPA s.20 — consent from the holder of parental responsibility for minors below ten, and Civil and Commercial Code capacity rules for older minors; GDPR Art. 8 — 16 for information-society services, member states may lower to no less than 13; verify the threshold for each market you serve.
- [ ] **Age-gating designed honestly:** neutral age screen or verified assurance proportionate to risk; never a nudge to claim an older age; children's flows collect the minimum and default to private.
- [ ] **No behavioral profiling or marketing targeting of children** without the stronger basis applicable under local law; when unsure, don't.

## 10. Marketing & Messaging

- [ ] **Consent before promotional messages:** opt-in per channel (email/SMS/LINE/push); the service-message exemption is narrow — don't stretch it → [[bot]].
- [ ] **Opt-out honored immediately:** unsubscribe works on day one and propagates to every sending tool within the SLA; no dark-pattern "manage preferences" maze.
- [ ] **Platform block/unfollow respected:** a user who blocked or unfollowed the bot/account stays suppressed — platform-level opt-out is data-subject intent → [[bot]].
- [ ] **Do-not-contact list enforced across tools:** one suppression list, every sender consults it; marketing lists built before the rule get reconciled.
- [ ] **New marketing purpose = new consent:** list rental, partner promotion, and new channels are new purposes; never inherit consent by association.
- [ ] **Sending systems prove consent per message:** each outbound campaign can answer "who consented, when, to what" for every recipient — channel opt-in evidence travels with the list, not in someone's inbox.

## 11. Governance & Accountability
> **Note:** accountability means demonstrable evidence, not good intentions — every item here should produce a document someone can be shown.

- [ ] **DPO / privacy lead designated when triggers apply:** PDPA s.41 triggers include public authorities, large-scale monitoring, and sensitive-data processing (s.26); GDPR Art. 37 similar (large-scale systematic monitoring / special categories at scale) — verify the exact trigger wording per regime; register the DPO where the regulator keeps a register (PDPC has one).
- [ ] **Privacy-by-design review in the SDLC:** new feature touching personal data gets a privacy review in design; gate it like [[qa]] gates tests and [[web]] gates accessibility.
- [ ] **Impact assessment (DPIA/PIA) for high-risk processing:** new data category, new profiling, large-scale sensitive data, new market — assessed before build (GDPR Art. 35-style; per applicable law).
- [ ] **Staff training on PII handling:** onboarding + yearly refresh; support and data teams get the deeper version; attendance kept as evidence.
- [ ] **Audit evidence kept:** consent records, ROPA versions, DPAs, rights-request log, breach register, training records — organized so an audit request is a search, not a scramble.
- [ ] **Sub-processor change notifications processed:** someone owns the vendor's change feed and re-runs the §8 review on material changes.
- [ ] **Extraterritorial obligations covered:** if you target Thailand from abroad, designate a written representative in the Kingdom (PDPA s.37(5), for controllers under s.5); if you target the EU from abroad, GDPR Art. 3 applies to offering goods/services or monitoring behavior in the Union — an EU representative may be required (verify trigger wording).

## 12. Regional Quick Reference

Breadth-first comparison — verify hot facts before legal reliance:

- **Breach notification deadline:** GDPR — 72h to the supervisory authority from awareness (Art. 33), reasons required if late. PDPA — without delay, where feasible within 72h to the PDPC Office (s.37(4)); high-risk cases also notify data subjects without delay. US state laws — vary by state (commonly ~30–60 days to regulator/credit bureaus; verify per state).
- **Consent model:** GDPR — freely given, specific, informed, unambiguous (Art. 7); children 16 for information-society services, floor of 13 (Art. 8). PDPA — consent-based with withdrawal as easy as opt-in (s.19); minors per s.20 (parental responsibility below ten + CCC capacity rules). US state laws — largely opt-out (sale/sharing, targeted advertising, GPC-style signals) with opt-in for certain sensitive/minor data (verify per state).
- **Extraterritorial reach:** GDPR Art. 3 — non-EU controllers offering goods/services to, or monitoring behavior of, people in the Union. PDPA s.5 — non-Thai controllers/processors whose collection/use/disclosure relates to people in Thailand, with a local representative duty (s.37(5)). US state laws — typically entity/revenue/processing-volume thresholds (verify per state).
- **Penalties:** GDPR — up to EUR 20 000 000 or 4% of total worldwide annual turnover, whichever higher (Art. 83). PDPA — tiered administrative fines up to THB 1M / 3M / 5M by offence class (sensitive-data violations at the top tier), plus criminal and civil liability (ss.82–88). US state laws — civil penalties per statute, often per-violation (verify per state).

---

## Quick Sanity Check Before Release

- [ ] Every field collected in the new flow appears in the ROPA with a purpose and lawful basis
- [ ] Privacy notice at the collection point matches what the code actually does (read both, side by side)
- [ ] Consent captures are recorded (who/when/version) and withdrawal works end-to-end within SLA
- [ ] One rights request (access or erasure) processed end-to-end before launch, including the warehouse → [[data]]
- [ ] PII grep over fresh logs/debug output of the new feature comes back empty
- [ ] Retention job covers any new data class and was seen deleting test records
- [ ] Any new vendor/SDK has a DPA, a sub-processor note, and a place in the data-flow map → [[mobile]]
- [ ] If the flow touches sensitive or children's data: explicit-consent path and age gate verified against the target market's threshold

---

## Sources

- PDPC Thailand (pdpc.or.th) — Personal Data Protection Act, B.E. 2562 (2019), official English translation (Gazette No. 136, 27 May 2019): ss.5, 19–21, 23–24, 26, 28–30, 37–41, 82–88 verified 2026-11-15; filing mechanics and Committee notifications change — re-check pdpc.or.th before filing.
- PDPC Notifications (pdpc.or.th → Laws & Regulations → Notifications of the PDPC) — subordinate rules on breach-notification procedure, DPO registration, and cross-border criteria; these carry the operational detail behind the Act's sections.
- GDPR full text — gdpr-info.eu / eur-lex.europa.eu: Arts. 3, 8, 12, 33, 83 verified 2026-11-15; other article references are well-known anchors, confirm text at eur-lex before legal reliance.
- ICO-style guidance (ico.org.uk) for practice-level patterns (notices, DPIAs, breach response); US state law details vary — check the relevant state attorney general's guidance.
- Cross-border practice notes from major regulators' SCC/transfer-impact-assessment guidance (EU: EDPB recommendations; read them as interpretation, not statute).
- Security sibling: [[security]].
