---
document_type: Enterprise Limitation
version: "1.0"
status: Active
author: "[Business Analyst]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [enterprise-limitation, organizational, babok]
standard_ref:
  - BABOK v3 — Solution Evaluation
---

# Enterprise Limitation

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Documents enterprise-level limitations that affect the solution — organizational, environmental, or regulatory constraints beyond the solution itself.

## 2. Enterprise Limitations

| # | Limitation | Category | Impact on Solution | Mitigation |
|---|-----------|---------|-------------------|-----------|
| 1 | [Legacy ERP integration constraints] | [Technical] | [Sync limited to batch, not real-time] | [Nightly sync + manual refresh] |
| 2 | [Staff resistance to new system] | [Organizational] | [Slow adoption, workarounds] | [Training + change management] |
| 3 | [Regulatory compliance requirements] | [Regulatory] | [Additional audit logging, data retention] | [Built into Phase 1] |
| 4 | [Budget constraints for Phase 2] | [Financial] | [Deferred features] | [Prioritize highest-value features] |
| 5 | [Limited IT support capacity] | [Organizational] | [Slower issue resolution] | [Vendor support contract] |

## 3. Organizational Limitations

| Limitation | Impact | Mitigation | Owner |
|-----------|--------|-----------|-------|
| [Staff capacity for training] | [Slow adoption] | [Phased training, e-learning] | [Change Manager] |
| [Change resistance] | [Workarounds, low usage] | [Change management program] | [Change Manager] |
| [IT support capacity] | [Slow issue resolution] | [Vendor L2/L3 support] | [IT Director] |

## 4. Technical Limitations

| Limitation | Impact | Mitigation | Owner |
|-----------|--------|-----------|-------|
| [Legacy ERP] | [Batch sync only] | [Nightly sync + manual refresh] | [Tech Lead] |
| [Network bandwidth] | [Slow file uploads] | [Compression, CDN] | [DevOps] |
| [Browser compatibility] | [IE11 not supported] | [Documented, modern browsers only] | [Tech Lead] |

## 5. Regulatory Limitations

| Regulation | Requirement | Impact | Compliance |
|-----------|------------|--------|-----------|
| [Data Protection] | [Data retention 7 years] | [Storage cost] | ✅ Built-in |
| [Audit Trail] | [All actions logged] | [Performance overhead] | ✅ Built-in |
| [Accessibility] | [WCAG 2.1 AA] | [Development effort] | ✅ Compliant |

## 6. Environmental Limitations

| Factor | Limitation | Impact | Mitigation |
|--------|-----------|--------|-----------|
| [Market conditions] | [Competitive pressure] | [Feature parity needed] | [Continuous improvement] |
| [Technology trends] | [Rapid tech evolution] | [Technology debt] | [Regular updates] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Solution-Limitation]] | Solution-specific limitations |
| [[Recommended-Actions]] | Actions to address limitations |
| [[Gap-Analysis]] | Gaps identified |

---

> **Template Standard:** Based on BABOK v3
> **Usage:** Enterprise limitations are *context*, not excuses. Document them so stakeholders understand constraints.
