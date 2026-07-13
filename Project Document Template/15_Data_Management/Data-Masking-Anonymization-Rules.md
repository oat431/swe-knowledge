---
document_type: Data Masking / Anonymization Rules
version: "1.0"
status: Draft
author: "[Data Architect]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Confidential"
tags: [data-masking, anonymization, privacy, dmbok, gdpr]
standard_ref:
  - DMBOK v2 — Data Security
  - GDPR — Data Protection
---

# Data Masking / Anonymization Rules

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines rules for masking and anonymizing sensitive data — protecting privacy while enabling data use.

## 2. Masking Techniques

| Technique | Description | Use Case | Example |
|----------|-----------|---------|---------|
| [Full Masking] | [Replace entire value with ***] | [Display in UI] | [***-**-1234] |
| [Partial Masking] | [Show partial value] | [Display in UI] | [j***@***.com] |
| [Tokenization] | [Replace with random token] | [Storage] | [tok_abc123] |
| [Hashing] | [One-way hash] | [Comparison] | [sha256(email)] |
| [Generalization] | [Reduce precision] | [Analytics] | [Age 25 → 20-30] |
| [Shuffling] | [Random rearrangement] | [Testing] | [Shuffle PII columns] |
| [Synthetic Data] | [Generate fake data] | [Testing] | [Fake names, addresses] |

## 3. Masking Rules by Data Element

| Data Element | Display Mask | Export Mask | Analytics Mask | Test Mask |
|-------------|-------------|------------|---------------|----------|
| [Email] | [j***@***.com] | [Full mask] | [Hash] | [Synthetic] |
| [Phone] | [+66***XXXX] | [Full mask] | [Hash] | [Synthetic] |
| [Name] | [J*** D**] | [Full mask] | [Generalize] | [Synthetic] |
| [Address] | [Bangkok, ****] | [Full mask] | [City only] | [Synthetic] |
| [ID Number] | [***-*****-**-X] | [Full mask] | [Hash] | [Synthetic] |
| [Account Number] | [****1234] | [Full mask] | [Hash] | [Synthetic] |
| [Amount] | [No mask] | [No mask] | [Range] | [No mask] |
| [Date of Birth] | [**/**/1990] | [Full mask] | [Year only] | [Synthetic] |

## 4. Masking by Context

| Context | Masking Level | Examples |
|---------|-------------|---------|
| [UI Display] | [Partial mask] | [j***@***.com, ****1234] |
| [API Response] | [Partial mask] | [Based on caller role] |
| [Logs] | [Full mask] | [No PII in logs] |
| [Reports] | [Generalize] | [Age ranges, regions] |
| [Export] | [Full mask] | [Unless authorized] |
| [Analytics] | [Hash or generalize] | [No re-identification] |
| [Testing] | [Synthetic] | [Fake data] |
| [Development] | [Synthetic] | [No production data] |

## 5. Anonymization Techniques (GDPR)

| Technique | Irreversible | Use Case | GDPR Compliant |
|----------|-------------|---------|---------------|
| [k-anonymity] | ✅ | [Dataset publication] | ✅ |
| [l-diversity] | ✅ | [Sensitive attributes] | ✅ |
| [t-closeness] | ✅ | [Statistical analysis] | ✅ |
| [Differential privacy] | ✅ | [Aggregate analytics] | ✅ |
| [Pseudonymization] | ❌ (reversible) | [Internal analytics] | ✅ (with controls) |

## 6. Implementation

```typescript
// Email masking
function maskEmail(email: string): string {
  const [local, domain] = email.split('@');
  return `${local[0]}***@***.${domain.split('.').pop()}`;
}

// Phone masking
function maskPhone(phone: string): string {
  return phone.replace(/(\d{2})\d+(\d{4})/, '$1***$2');
}

// Name masking
function maskName(name: string): string {
  return name.split(' ').map(n => `${n[0]}${'*'.repeat(n.length - 1)}`).join(' ');
}
```

## 7. Verification

| Test | Frequency | Method | Success Criteria |
|------|----------|--------|-----------------|
| [Masking effectiveness] | [Per release] | [Manual review] | [No PII visible] |
| [Re-identification risk] | [Quarterly] | [Statistical analysis] | [Risk < 0.1%] |
| [Synthetic data quality] | [Per release] | [Distribution comparison] | [Similar distribution] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Data-Classification-Schema]] | Classification driving masking |
| [[Privacy-Impact-Assessment]] | Privacy requirements |
| [[Data-Encryption-Standards]] | Encryption standards |

---

> **Template Standard:** Based on DMBOK v2, GDPR
> **Usage:** Masking is *not* encryption. Encrypted data can be decrypted. Masked data cannot be unmasked (if done correctly).
