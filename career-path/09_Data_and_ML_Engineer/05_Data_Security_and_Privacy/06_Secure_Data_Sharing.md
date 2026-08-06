---
title: "Secure Data Sharing"
note_type: capability-topic
capability_area: data-security-and-privacy
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
  - "[[01_Data_Classification_and_Sensitivity]]"
tags:
  - career-path
  - data-engineering
  - data-sharing
  - differential-privacy
  - anonymization
---

# Secure Data Sharing

> Enabling data exchange across organizational or system boundaries while controlling re-identification risk and meeting legal obligations.

## Why This Is a Senior Skill

Mid-level engineers export data when asked. Senior engineers evaluate whether sharing is safe, design anonymization that preserves analytical utility, negotiate data sharing agreements, and choose between technical approaches based on the trust relationship and regulatory context.

The senior challenge is that data sharing is where security, privacy, legal, and business interests collide. Getting it wrong means regulatory fines, re-identification of supposedly anonymous data, or losing a partnership.

## Core Frameworks

### Sharing Approach Decision Matrix

| Approach | Trust Level | Re-identification Risk | Analytical Utility |
|----------|-------------|----------------------|-------------------|
| Raw data with DSA | High: contractual trust | High: no technical protection | Full |
| Anonymized data | Medium | Medium: depends on k-anonymity | Reduced: generalization loses detail |
| Pseudonymized data | Medium-high | Medium: re-identification possible with auxiliary data | High: structure preserved |
| Differential privacy | Low-medium | Very low: mathematical guarantee | Reduced: noise added to queries |
| Secure enclave | Low | Very low: data never leaves enclave | Full within enclave |
| Federated learning | Low | Very low: only model updates shared | Model quality may degrade |

### Anonymization Techniques

| Technique | How It Works | Utility Loss | Re-identification Protection |
|-----------|-------------|-------------|---------------------------|
| k-Anonymity | Each record matches at least k-1 others | Moderate: generalization | Low: homogeneous attack |
| l-Diversity | Sensitive attribute has l distinct values in each group | Higher than k-anonymity | Medium: limits attribute disclosure |
| t-Closeness | Distribution of sensitive attribute within epsilon of global | Highest among structural | Medium-high |
| Generalization | Replace specific values with ranges | Moderate | Medium |
| Suppression | Remove identifying records | Low: few records lost | High for suppressed records |
| Data swapping | Exchange values between records | Low: preserves marginals | Low: vulnerable to attack |

### Data Sharing Agreement Checklist

| Element | Why It Matters |
|---------|---------------|
| Purpose limitation | Defines what the recipient can do with the data |
| Retention period | When the recipient must delete or return the data |
| Security requirements | Minimum encryption, access control, and audit standards |
| Breach notification | Timeline and process for reporting data breaches |
| Audit rights | Your right to verify the recipient's compliance |
| Sub-processor restrictions | Whether the recipient can share with third parties |
| Data subject rights | How erasure and access requests flow between parties |
| Liability and indemnity | Who pays if something goes wrong |

## In Practice

**Start with the question, not the data.** Before sharing a dataset, ask what question the recipient needs to answer. Often the answer can be provided through an aggregated result or API endpoint without sharing raw data at all.

**Use differential privacy for public statistics.** When publishing aggregate statistics, differential privacy provides a mathematical guarantee that no individual can be identified. The privacy budget epsilon controls the trade-off between accuracy and privacy.

**Secure enclaves for high-value, low-trust sharing.** When a partner needs to run analytics on your data but you cannot trust them with raw access, a secure enclave lets them run code in a controlled environment. The data never leaves, and you audit every query.

**Test anonymization rigorously.** k-Anonymity with k=5 sounds safe until someone combines your dataset with a purchased voter registry. Always test anonymized output against known auxiliary datasets and measure re-identification rates.

**Negotiate the DSA before sharing a single row.** The data sharing agreement is not a formality. It defines what happens when things go wrong. Senior engineers involve legal early and ensure the agreement covers breach scenarios.

## Practical Exercise

Design a data sharing approach for a scenario: your company wants to share customer behavior data with a research partner:
1. Define the research question: what does the partner need to learn?
2. Choose an anonymization approach and justify it against the decision matrix
3. Apply the chosen technique to a sample dataset and measure utility loss
4. Test re-identification risk: can you identify individuals using public auxiliary data?
5. Draft the key clauses of a data sharing agreement for this scenario
6. Define the technical controls: how is the data transmitted, stored, and eventually deleted?

## Knowledge Connections

- [[05_Data_Security]]: DMBOK data sharing and security
- [[CyBOK v1 - Overview]]: cryptography and privacy for sharing
- [[01_Data_Classification_and_Sensitivity]]: classification determines sharing restrictions
- [[04_Privacy_Engineering]]: privacy obligations at sharing boundaries
- [[05_Audit_and_Lineage_for_Compliance]]: auditing shared data usage
- [[03_Data_Integration_and_Interoperability/00_overview|Data Integration]]: sharing as a form of integration

## Key Takeaways

- Data sharing is a trust, legal, and technical problem: solving only one dimension leaves the others exposed
- Start with the question, not the data: often an API or aggregate result replaces a raw data transfer
- Differential privacy provides mathematical guarantees but reduces analytical utility: choose epsilon carefully
- Anonymization must be tested against auxiliary data: k-anonymity alone is not sufficient against targeted attacks
- The data sharing agreement is a critical control: negotiate it before sharing any data
- Secure enclaves enable analytics without data transfer when trust is low and value is high
