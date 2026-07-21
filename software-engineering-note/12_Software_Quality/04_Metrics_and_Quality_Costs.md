---
tags:
  - quality
  - metrics
  - function-points
  - cost-of-quality
  - software-quality
source: "Galin SQA Ch 21–22"
---

# Metrics and Quality Costs

> **Source:** Galin, *Software Quality Assurance* — Chapter 21 (Software Quality Metrics) and Chapter 22 (Costs of Software Quality)
> **Purpose:** Quantify software quality through metrics for process and product, and evaluate the economic impact of quality (and its absence) through cost-of-quality models.

## 1. Software Quality Metrics (Ch 21)

> *"You can't control what you can't measure."* — Tom DeMarco (1982)

### 1.1 Objectives of Quality Measurement

Per **Frame 21.1**, software quality metrics serve two main objectives:

1. **Facilitate management control, planning, and intervention** — by measuring deviations of actual performance from planned performance in:
   - Functional (quality) performance
   - Timetable and budget performance

2. **Identify situations for process improvement** (preventive/corrective actions) — based on:
   - Accumulated metrics across teams and units
   - Organizational quality achievements
   - Industry benchmarks

### 1.2 Requirements for Successful Metrics (Frame 21.2)

| Category | Requirement | Explanation |
|----------|-------------|-------------|
| **General** | Relevant | Measures an attribute of substantial importance |
| | Valid | Measures the required attribute |
| | Reliable | Produces similar results under similar conditions |
| | Comprehensive | Applicable to a large variety of situations |
| | Mutually exclusive | Does not measure attributes already measured by others |
| **Operative** | Easy and simple | Data collection with minimal resources |
| | No independent data collection | Integrated with existing systems (payroll, cost accounting) |
| | Immune to biased intervention | Resistant to manipulation by interested parties |

### 1.3 Classification of Metrics

A **two-level classification** system:

**Level 1 — Lifecycle phase:**
- **Process metrics** — software development
- **Product metrics** — software maintenance (operational phase)

**Level 2 — Subject measured:**
- Quality
- Timetable
- Effectiveness (error removal, maintenance)
- Productivity

Two common **size measures** underpin many metrics:

| Measure | Description |
|---------|-------------|
| **KLOC** | Thousands of lines of code; language/tool dependent |
| **Function Points (FP)** | Based on functionality specified; language independent (see §1.7) |

---

### 1.4 Process Metrics (Development Phase)

#### 1.4.1 Software Process Quality Metrics

**Error Density Metrics** (Table 21.1):

| Code | Name | Formula |
|------|------|---------|
| CED | Code Error Density | NCE / KLOC |
| DED | Development Error Density | NDE / KLOC |
| WCED | Weighted Code Error Density | WCE / KLOC |
| WDED | Weighted Development Error Density | WDE / KLOC |
| WCEF | Weighted Code Errors per Function Point | WCE / NFP |
| WDEF | Weighted Development Errors per Function Point | WDE / NFP |

Where:
- **NCE** = number of code errors detected by inspections and testing
- **NDE** = total development errors (design + code)
- **WCE / WDE** = weighted versions using severity weights (e.g., Low=1, Medium=3, High=9)
- **NFP** = number of function points

> **Key insight:** Weighted metrics can yield different conclusions than unweighted ones. A project may pass CED thresholds but fail WCED thresholds, revealing hidden severity issues.

**Error Severity Metrics** (Table 21.2):

| Code | Name | Formula |
|------|------|---------|
| ASCE | Average Severity of Code Errors | WCE / NCE |
| ASDE | Average Severity of Development Errors | WDE / NDE |

These detect situations where errors become *more severe* even as *total counts* decrease.

#### 1.4.2 Timetable Metrics (Table 21.3)

| Code | Name | Formula |
|------|------|---------|
| TTO | Timetable Observance | MSOT / MS |
| ADMC | Average Delay of Milestone Completion | TCDAM / MS |

Where MSOT = milestones completed on time, MS = total milestones, TCDAM = total completion delays for all milestones.

#### 1.4.3 Error Removal Effectiveness Metrics (Table 21.4)

| Code | Name | Formula |
|------|------|---------|
| DERE | Development Errors Removal Effectiveness | NDE / (NDE + NYF) |
| DWERE | Development Weighted Errors Removal Effectiveness | WDE / (WDE + WYF) |

Where **NYF** = software failures detected during a year of maintenance service, **WYF** = weighted version.

#### 1.4.4 Productivity Metrics (Table 21.5)

| Code | Name | Formula |
|------|------|---------|
| DevP | Development Productivity | DevH / KLOC |
| FDevP | Function Point Development Productivity | DevH / NFP |
| CRe | Code Reuse | ReKLOC / KLOC |
| DocRe | Documentation Reuse | ReDoc / NDoc |

---

### 1.5 Product Metrics (Maintenance/Operational Phase)

Product metrics cover two service types:
- **Help Desk (HD)** — user support, instruction, usability issues
- **Corrective Maintenance** — fixing software failures

#### 1.5.1 HD Quality Metrics

**HD Calls Density** (Table 21.6):

| Code | Name | Formula |
|------|------|---------|
| HDD | HD Calls Density | NHYC / KLMC |
| WHDD | Weighted HD Calls Density | WHYC / KLMC |
| WHDF | Weighted HD Calls per Function Point | WHYC / NMFP |

**Severity of HD Calls:**
```
ASHC = WHYC / NHYC    (Average Severity of HD Calls)
```

**HD Service Success:**
```
HDS = NHYOT / NHYC    (proportion completed on time)
```

#### 1.5.2 HD Productivity & Effectiveness

| Code | Name | Formula |
|------|------|---------|
| HDP | HD Productivity | HDYH / KLMC |
| FHDP | Function Point HD Productivity | HDYH / NMFP |
| HDE | HD Effectiveness | HDYH / NHYC |

#### 1.5.3 Corrective Maintenance Quality Metrics

**Failure Density** (Table 21.8):

| Code | Name | Formula |
|------|------|---------|
| SSFD | Software System Failure Density | NYF / KLMC |
| WSSFD | Weighted Software System Failure Density | WYF / KLMC |
| WSSFF | Weighted Failures per Function Point | WYF / NMFP |

**Failure Severity:**
```
ASSSF = WYF / NYF
```

**Maintenance Service Failure:**
```
MRepF = RepYF / NYF    (repeated repair failure rate)
```

**Availability Metrics** (Table 21.9):

| Code | Name | Formula |
|------|------|---------|
| FA | Full Availability | (NYSerH − NYFH) / NYSerH |
| VitA | Vital Availability | (NYSerH − NYVitFH) / NYSerH |
| TUA | Total Unavailability | NYTFH / NYSerH |

Where NYSerH = hours in service per year, NYFH = hours with any function failed, NYVitFH = hours with vital function failed, NYTFH = hours of total failure.

**Relationship:** `1 − TUA ≥ VitA ≥ FA`

#### 1.5.4 Corrective Maintenance Productivity & Effectiveness (Table 21.10)

| Code | Name | Formula |
|------|------|---------|
| CMaiP | Corrective Maintenance Productivity | CMaiYH / KLMC |
| FCMP | Function Point Corrective Maintenance Productivity | CMaiYH / NMFP |
| CMaiE | Corrective Maintenance Effectiveness | CMaiYH / NYF |

---

### 1.6 Implementation of Software Quality Metrics

#### 1.6.1 Defining New Metrics (4-stage process)

1. **Define the attribute** to be measured (quality, productivity, etc.)
2. **Define the metric** and validate against requirements (Frame 21.2)
3. **Determine comparative target values** (indicators) based on standards, prior performance
4. **Define application process** — reporting method, data collection method

> ⚠️ **Implementation tip:** Many organizations skip stage 3 (setting target values), reflecting a lack of serious commitment to using metrics for managerial control.

#### 1.6.2 Managerial Aspects

- Assign responsibility for reporting and data collection
- Train teams on new metrics
- Follow-up: support, control completeness/accuracy
- Update definitions based on past performance

#### 1.6.3 Statistical Analysis

| Type | Purpose | Tools |
|------|---------|-------|
| **Descriptive statistics** | Identify trends; mean, median, mode, histograms, control charts | Standard statistical packages |
| **Analytical statistics** | Assess statistical significance of observed changes | Regression, ANOVA, t-test, χ² test |

> Fundamental difference from manufacturing SPC: software development activities are **not repetitive** in the SPC sense — each project is unique.

#### 1.6.4 Actions in Response

- **Direct actions** — initiated by project/team management (reorganization, method changes, metric revision)
- **Indirect actions** — initiated by the Corrective Action Board (CAB), based on accumulated cross-project data

---

### 1.7 Limitations of Software Metrics (§21.6)

**Universal obstacles:**
- Budget constraints for metrics system development
- Employee opposition to evaluation
- Uncertainty from biased reporting

**Software-specific barriers** — factors that distort metric parameters:

| Parameter | Distorting Factors |
|-----------|-------------------|
| **KLOC** | Programming style ("wasteful" coding can double KLOC); volume of documentation comments; software complexity; % reused code |
| **NCE / NDE** | Complexity; % reused code; professionalism of review/testing teams; reporting style (concise vs. comprehensive) |
| **NYF / NHYC** | Initial software quality; programming style; complexity; % reused code; number of installations and user population size |

**Bottom line:** Many commonly used metrics suffer from **low validity** and **limited comprehensiveness**. The function point method is a successful attempt to replace the problematic KLOC measure.

---

### 1.8 Appendix 21A: The Function Point Method

#### Purpose
Provides **pre-project estimates** of project size in terms of development resources — something KLOC cannot do (code line count only available after programming).

#### Three Stages

**Stage 1: Compute Crude Function Points (CFP)**
Count five component types and classify each as Simple, Average, or Complex:

| Component | Simple | Average | Complex |
|-----------|--------|---------|---------|
| User inputs | ×3 | ×4 | ×6 |
| User outputs | ×4 | ×5 | ×7 |
| User online queries | ×3 | ×4 | ×6 |
| Logical files | ×7 | ×10 | ×15 |
| External interfaces | ×5 | ×7 | ×10 |

**Stage 2: Compute Relative Complexity Adjustment Factor (RCAF)**
Grade 14 complexity subjects from 0–5 (e.g., backup/recovery requirements, data communication, distributed processing, performance requirements, etc.). RCAF ranges 0–70.

**Stage 3: Compute Function Points**
```
FP = CFP × (0.65 + 0.01 × RCAF)
```

#### Advantages vs. Disadvantages

| Advantages | Disadvantages |
|------------|---------------|
| Pre-project estimates possible | Results depend on counting manual used (IFPUG 3/4, Mark II) |
| Based on requirements, not code — language independent | Requires detailed requirements specifications |
| Relatively high reliability | Needs experienced FP team and substantial resources |
| | Many subjective evaluations |
| | Best validated for data processing systems; limited in other domains |

#### Language Conversion (avg LOC per FP)

| Language/Tool | Avg LOC per FP |
|---------------|----------------|
| C | 128 |
| C++ | 64 |
| Visual Basic | 32 |
| Power Builder | 16 |
| SQL | 12 |

---

## 2. Costs of Software Quality (Ch 22)

### 2.1 Objectives (Frame 22.1)

Cost of software quality metrics enable **economic control** over SQA activities:

1. **Control** organization-initiated costs to prevent and detect software errors
2. **Evaluate** economic damages of software failures as basis for revising SQA budget
3. **Evaluate** plans to increase/decrease SQA activities or invest in infrastructure, based on past economic performance

Key comparative ratios:
- % of CoSQ out of total development costs
- % of software failure costs out of total development costs
- % of CoSQ out of total maintenance costs
- % of CoSQ out of total sales of software products and maintenance

---

### 2.2 The Classic Model (Feigenbaum, 1950s)

```
                        ┌── Prevention Costs
        ┌── Control ────┤
        │               └── Appraisal Costs
CoSQ ───┤
        │               ┌── Internal Failure Costs
        └── Failure ────┤
                        └── External Failure Costs
```

#### 2.2.1 Prevention Costs
Investments in quality infrastructure and activities **not tied to a specific project**:
- Developing/updating SQA infrastructure (procedures, templates, checklists, SCM, metrics)
- Training employees in SQA procedures
- Employee certification
- SQA consultations
- Internal quality reviews, external audits, management quality reviews

#### 2.2.2 Appraisal Costs
Detection of errors in **specific projects or systems**:
- **Reviews:** Formal design reviews, peer reviews (inspections, walkthroughs), expert reviews
- **Testing:** Unit, integration, system, acceptance tests
- **External participants:** Quality assurance of subcontractors, COTS suppliers, customer contributions

#### 2.2.3 Internal Failure Costs
Correcting errors detected **before installation** at customer sites:
- Redesign/reprogramming after review/test findings
- Repeated design reviews and regression testing

> **Note:** Regular reviews/tests are *appraisal* costs. **Repeated** reviews/tests due to poor quality are *internal failure* costs.

#### 2.2.4 External Failure Costs
Correcting failures detected **after installation**. Two types:

| Type | Examples |
|------|----------|
| **Overt** | Warranty corrections, bug fixes at customer sites, damages paid, purchase reimbursements, insurance |
| **Hidden** (far larger) | Lost sales to dissatisfied customers, damaged reputation, increased sales promotion costs, reduced tender competitiveness |

---

### 2.3 The Extended Model (§22.3)

The classic model **excludes managerial costs** that are substantial in software:

```
                        ┌── Prevention Costs
        ┌── Control ────┼── Appraisal Costs
        │               └── Managerial Preparation & Control Costs  ← NEW
CoSQ ───┤
        │               ┌── Internal Failure Costs
        └── Failure ────┼── External Failure Costs
                        └── Managerial Failure Costs                ← NEW
```

#### 2.3.1 Managerial Preparation & Control Costs
- Contract reviews (proposal draft and contract draft reviews)
- Preparing and reviewing project plans, including quality plans
- Periodic updating of project and quality plans
- Regular progress control of internal development
- Regular progress control of external participants' contributions

#### 2.3.2 Managerial Failure Costs
- Unplanned resource costs from underestimation in proposals
- Damages for late completion due to unrealistic scheduling
- Damages for late completion due to failure to recruit sufficient staff
- **Domino effect:** damages to *other* projects caused by delays cascading from the original project

---

### 2.4 Application of a CoSQ System (§22.4)

#### 2.4.1 Define the Model
1. Choose classic or extended model
2. List relevant cost items, each mapped to a cost subclass
3. Agree on allocation rules for shared costs across departments/projects

#### 2.4.2 Define Data Collection Method
- **Prefer the existing MIS** (Management Information System) over an independent system:
  - Cost savings
  - Avoids data inconsistencies between systems
- Adapt HR costing, ledger categories, and accounting systems

#### 2.4.3 Implementation
- Assign reporting/collection responsibility
- Train teams
- Follow-up: support, review of classification and completeness
- Update definitions based on feedback

#### 2.4.4 The Cost of Software Quality Balance

```
     Cost
      ↑
      │   ╲ Total CoSQ
      │    ╲        ↗ Minimum total CoSQ
      │     ╲    ╱
      │      ╲ ╱
      │      ╱ ╲
      │    ╱     ╲ Total Failure Costs
      │  ╱        ╲
      │╱    Total Control Costs
      └────────────────────→ Quality Level
                  ↑
           Optimal Quality Level
```

**Core concept:** Increasing control costs reduces failure costs, and vice versa. There exists an **optimal quality level** where **total CoSQ is minimized**. Management targets this minimum when budgeting.

**Typical actions and expected results** (Table 22.2):

| Action | Expected Result |
|--------|----------------|
| Improve help function | ↓ External failure costs |
| Increase contract review resources | ↓ Managerial failure costs |
| Reduce ineffective training | ↓ Prevention costs (no failure increase) |
| Train inspection teams | ↓ Internal + external failure costs |
| Intensify progress control | ↓ Managerial failure costs |
| Certify subcontractor list | ↓ Failure costs (especially external) |
| Automate testing | ↓ Internal + external failure costs |

---

### 2.5 Problems in Applying CoSQ Metrics (§22.5)

#### Standard Problems (all industries)
- Inaccurate/incomplete identification and classification
- Negligent reporting
- Biased reporting (especially "censored" internal/external costs)
- "Camouflaged" compensation (discounted future services, free services) not recorded as external failure costs

#### Software-Specific Problems

**Collecting managerial preparation & control costs:**
- Activities are fragmented into short, disconnected segments → inaccurate time reporting
- Senior staff often exempt from time reporting

**Collecting managerial failure costs:**
- Hard to assign responsibility for schedule failures (customer? development team? management?)
- Compensation often occurs long after project completion — too late for lessons learned
- Three potential classifications for delay causes:

| Cause of Delay | Cost Class |
|----------------|-----------|
| Customer-driven spec changes during development | Customer responsibility |
| Customer-delayed hardware installation / staff recruitment | Customer responsibility |
| Poor development team performance → extensive rework | External failure costs |
| Unrealistic proposal schedules/budgets | **Managerial failure costs** |
| Late/inadequate staff recruitment | **Managerial failure costs** |

---

## Summary Table: Metrics vs. CoSQ

| Aspect | Quality Metrics (Ch 21) | Cost of Quality (Ch 22) |
|--------|------------------------|------------------------|
| **Unit** | Counts, ratios, densities | Financial values ($) |
| **Scope** | Process + Product quality | Economic impact of quality activities |
| **Models** | Error density, severity, productivity, effectiveness | Classic (4 subclasses) + Extended (+2 managerial) |
| **Primary use** | Detect deviations, compare performance | Budget control, investment evaluation |
| **Key challenge** | Parameter validity (KLOC, NCE bias) | Accurate cost identification and classification |

## Related Notes

- [[Software Quality Overview|Software Quality Overview]] — Overview and SWEBOK context
- [[../11_Software_Testing/Software Testing Overview|Software Testing]] — Testing is a key appraisal cost and defect detection activity
- [[../15_Software_Engineering_Economics/Software Engineering Economics Overview|Software Engineering Economics]] — Economic models for software cost estimation
