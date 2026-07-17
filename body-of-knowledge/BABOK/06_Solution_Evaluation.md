---
tags:
- solution-evaluation
- business-analysis
- babok
---

# 06 — Solution Evaluation

> *Source: BABOK® Guide v3 by IIBA, Chapter 7 — Solution Evaluation*

## Purpose

The **Solution Evaluation** knowledge area describes how business analysts assess the performance of solutions and deliver recommendations for increasing value. It focuses on evaluating how well a solution fulfills business needs, identifying factors (both internal and external to the solution) that limit value realization, and defining actions the enterprise can take to close the gap between potential and actual value.

Solution Evaluation generally focuses on a **component of an enterprise** rather than the entire enterprise. It may include any combination of performance assessments, tests, and experiments, and may combine both objective and subjective assessments of value.

### The Business Analysis Value Spectrum

As business analysis activities progress from **potential value** (strategy and design) to **actual value** (operating solution), the focus shifts across the following stages:

```
Need → Requirements → Design → Prototype/Pilot → Operating Solution
(Potential Value)                     (Actual Value)
```

Core activities align as: **Strategy Analysis** and **Requirements Analysis & Design Definition** deliver *potential* value; **Solution Evaluation** assesses *actual* value.

### The BACCM in Solution Evaluation

| Core Concept | During Solution Evaluation, business analysts... |
|---|---|
| **Change** | recommend a change to either a solution or the enterprise in order to realize the potential value of a solution. |
| **Need** | evaluate how a solution or solution component is fulfilling the need. |
| **Solution** | assess the performance of the solution, examine if it is delivering the potential value, and analyze why value may not be realized. |
| **Stakeholder** | elicit information from stakeholders about solution performance and value delivery. |
| **Value** | determine if the solution is delivering the potential value and examine why value may not be being realized. |
| **Context** | consider the context when determining solution performance measures and any limitations within the context that may prohibit value from being realized. |

---

## Measure Solution Performance

**Task 8.1**

### Purpose

To define performance measures and use the data collected to evaluate the effectiveness of a solution in relation to the value it brings.

### Description

Performance measures determine the value of a newly deployed or existing solution. The measures used depend on the solution itself, the context, and how the organization defines value. When solutions do not have built-in performance measures, the business analyst works with stakeholders to determine and collect the measures that will best reflect the performance of a solution.

Performance may be assessed through **key performance indicators (KPIs)** aligned with enterprise measures, goals and objectives for a project, process performance targets, or tests for a software application.

### Inputs

- **Business Objectives**: measurable results the enterprise wants to achieve; provides a benchmark against which solution performance can be assessed.
- **Implemented Solution (external)**: a solution (or component) that exists in some form — an operating solution, a prototype, or a pilot/beta solution.

### Elements

1. **Define Solution Performance Measures** — Determine if current measures exist or if methods for capturing them are in place. Ensure existing measures are accurate, relevant, and elicit additional measures from stakeholders. Measures may be:
   - **Quantitative**: numerical, countable, or finite — involving amounts, quantities, or rates.
   - **Qualitative**: subjective — attitudes, perceptions, and other subjective responses from customers, users, and others involved in the operation of a solution.

2. **Validate Performance Measures** — Validate the performance measures and any influencing criteria with stakeholders. Specific performance measures should align with any higher-level measures that exist within the context affecting the solution. Decisions about which measures to use often reside with the sponsor.

3. **Collect Performance Measures** — Employ basic statistical sampling concepts, considering:
   - **Volume or Sample Size**: appropriate for the initiative; too small skews results, too large may be impractical.
   - **Frequency and Timing**: may affect the outcome of measurements.
   - **Currency**: more recent measurements tend to be more representative than older data.

### Guidelines and Tools

- Change Strategy
- Future State Description
- Requirements (validated)
- Solution Scope

### Techniques

- Acceptance and Evaluation Criteria
- Benchmarking and Market Analysis
- Business Cases
- Data Mining
- Decision Analysis
- Focus Groups
- Metrics and Key Performance Indicators (KPIs)
- Non-Functional Requirements Analysis
- Observation
- Prototyping
- Survey or Questionnaire
- Use Cases and Scenarios
- Vendor Assessment

### Stakeholders

- **Customer**: consulted for feedback on solution performance.
- **Domain SME**: consulted to provide potential measurements.
- **End User**: contributes to actual value realized; provides reviews and feedback on workload and job satisfaction.
- **Project Manager**: manages schedule and tasks for solution measurement.
- **Sponsor**: approves measures used; may also provide performance expectations.
- **Regulator**: dictates or prescribes constraints and guidelines for performance measures.

### Outputs

- **Solution Performance Measures**: measures that provide information on how well the solution is performing or potentially could perform.

---

## Analyze Performance Measures

**Task 8.2**

### Purpose

To provide insights into the performance of a solution in relation to the value it brings.

### Description

The measures collected in *Measure Solution Performance* often require interpretation and synthesis to derive meaning and to be actionable. Performance measures themselves rarely trigger a decision about the value of a solution.

To meaningfully analyze performance measures, business analysts require a thorough understanding of the potential value that stakeholders hope to achieve with the solution. Variables considered include enterprise goals and objectives, KPIs, risk levels, risk tolerance, and other stated targets.

### Inputs

- **Potential Value**: describes the value that may be realized by implementing the proposed future state; used as a benchmark.
- **Solution Performance Measures**: measures providing information on how well the solution is performing.

### Elements

1. **Solution Performance versus Desired Value** — Examine measures to assess their ability to help stakeholders understand the solution's value. A high-performing solution might contribute lower value than expected, while a low-performing but potentially valuable solution can be enhanced. If measures are insufficient, collect more measurements or treat the lack of measures as a solution risk.

2. **Risks** — Performance measures may uncover new risks to solution performance and to the enterprise. These risks are identified and managed like any other risks.

3. **Trends** — Consider the time period when data was collected to guard against anomalies and skewed trends. A large enough sample size over a sufficient period provides an accurate depiction. Note pronounced and repeated trends (e.g., increase in errors at certain times, change in process speed when volume increases).

4. **Accuracy** — Test and analyze data collected by performance measures to ensure accuracy. To be considered accurate and reliable, results should be reproducible and repeatable.

5. **Performance Variances** — The difference between expected and actual performance represents a variance. Root cause analysis may be necessary to determine underlying causes of significant variances. Recommendations for improvement are made in *Recommend Actions to Increase Solution Value*.

### Guidelines and Tools

- Change Strategy
- Future State Description
- Risk Analysis Results
- Solution Scope

### Techniques

- Acceptance and Evaluation Criteria
- Benchmarking and Market Analysis
- Data Mining
- Interviews
- Metrics and Key Performance Indicators (KPIs)
- Observation
- Risk Analysis and Management
- Root Cause Analysis
- Survey or Questionnaire

### Stakeholders

- **Domain SME**: identifies risks and provides insights into data for analysis.
- **Project Manager**: responsible for overall risk management; may participate in risk analysis.
- **Sponsor**: identifies risks, provides insights into data and potential value; makes decisions about significance of expected versus actual performance.

### Outputs

- **Solution Performance Analysis**: results of the analysis of measurements collected and recommendations to solve performance gaps and leverage opportunities to improve value.

---

## Assess Solution Limitations

**Task 8.3**

### Purpose

To determine the factors **internal to the solution** that restrict the full realization of value.

### Description

Assessing solution limitations identifies the root causes for under-performing and ineffective solutions and solution components. This task is closely linked to *Assess Enterprise Limitations*; these tasks may be performed concurrently. If the solution has not met its potential value, business analysts determine which factors — both internal and external to the solution — are limiting value. This task focuses on factors **internal** to the solution.

This assessment may be performed at any point during the solution life cycle: on a solution component during development, on a completed solution prior to full implementation, or on an existing operational solution.

### Inputs

- **Implemented Solution (external)**: a solution that exists in some form in order to be evaluated.
- **Solution Performance Analysis**: results of the analysis of measurements collected and recommendations to solve performance gaps.

### Elements

1. **Identify Internal Solution Component Dependencies** — Solutions often have internal dependencies that limit overall performance to the performance of the least effective component. Identify solution components with dependencies on other components, then determine if those dependencies limit solution performance and value realization.

2. **Investigate Solution Problems** — When the solution consistently or repeatedly produces ineffective outputs, perform problem analysis to identify the source. Problems may be indicated by:
   - Inability to meet a stated goal, objective, or requirement.
   - Failure to realize a benefit projected during *Define Change Strategy* or *Recommend Actions to Increase Solution Value*.

3. **Impact Assessment** — Review identified problems to assess the effect on the organization's operations or the solution's ability to deliver potential value. Determine:
   - **Severity** of the problem.
   - **Probability** of re-occurrence.
   - **Impact** on business operations.
   - **Capacity** of the business to absorb the impact.

   Identify which problems must be resolved, which can be mitigated through other activities (additional quality control, new/adjusted business processes, additional support for exceptions), and which can be accepted. Also assess risks specific to the solution and its limitations.

### Guidelines and Tools

- Change Strategy
- Risk Analysis Results
- Solution Scope

### Techniques

- Acceptance and Evaluation Criteria
- Benchmarking and Market Analysis
- Business Rules Analysis
- Data Mining
- Decision Analysis
- Interviews
- Item Tracking
- Lessons Learned
- Risk Analysis and Management
- Root Cause Analysis
- Survey or Questionnaire

### Stakeholders

- **Customer**: affected by the solution; consulted for reviews and feedback.
- **Domain SME**: provides input on how the solution should perform; identifies potential limitations.
- **End User**: uses the solution; consulted for reviews and feedback on workload and job satisfaction.
- **Regulator**: consulted about planned and potential value; may constrain the solution or the degree to which value is realized.
- **Sponsor**: approves potential value, provides resources, and approves changes to potential value.
- **Tester**: identifies solution problems during construction and implementation.

### Outputs

- **Solution Limitation**: a description of the current limitations of the solution including constraints and defects.

---

## Assess Enterprise Limitations

**Task 8.4**

### Purpose

To determine how factors **external to the solution** are restricting value realization.

### Description

Solutions may operate across various organizations within an enterprise, and therefore have many interactions and interdependencies. Solutions may also depend on environmental factors that are external to the enterprise. Enterprise limitations may include factors such as culture, operations, technical components, stakeholder interests, or reporting structures.

Assessing enterprise limitations identifies root causes and describes how enterprise factors limit value realization. This assessment may be performed at any point during the solution life cycle.

### Inputs

- **Current State Description**: the current internal environment of the solution including environmental, cultural, and internal factors influencing solution limitations.
- **Implemented (or Constructed) Solution (external)**: a solution that exists in some form in order to be evaluated.
- **Solution Performance Analysis**: results of the analysis of measurements collected and recommendations.

### Elements

1. **Enterprise Culture Assessment** — Evaluate the deeply rooted beliefs, values, and norms shared by members of the enterprise. Business analysts:
   - Identify whether stakeholders understand why a solution exists.
   - Ascertain whether stakeholders view the solution as beneficial and are supportive of the change.
   - Determine what cultural changes are required to better realize value.
   - Evaluate the enterprise's ability and willingness to adapt to cultural changes.
   - Assess internal and external stakeholders to gauge understanding, acceptance, perception of value, and communication needs.

2. **Stakeholder Impact Analysis** — Provide insight into how the solution affects a particular stakeholder group by considering:
   - **Functions**: the processes in which the stakeholder uses the solution — inputs provided, how the solution is used to execute the process, and outputs received.
   - **Locations**: geographic locations of stakeholders; disparate locations may impact use and value realization.
   - **Concerns**: issues, risks, and overall concerns stakeholders have with the solution, including its use, perceptions of value, and impact on the stakeholder's ability to perform necessary functions.

3. **Organizational Structure Changes** — Assess how the organization's structure is impacted by a solution. The reporting structure may be too complex or too simple to allow a solution to perform effectively. Formal and informal relationships (alliances, friendships, matrix-reporting) among stakeholders can enable or block adoption. Consider both formal and informal relationships.

4. **Operational Assessment** — Determine if the enterprise is able to adapt to or effectively use a solution. Identify which processes and tools are adequately equipped to benefit from the solution, and if sufficient and appropriate assets are in place. Consider:
   - Policies and procedures
   - Capabilities and processes that enable other capabilities
   - Skill and training needs
   - Human resources practices
   - Risk tolerance and management approaches
   - Tools and technology that support a solution

### Guidelines and Tools

- Business Objectives
- Change Strategy
- Future State Description
- Risk Analysis Results
- Solution Scope

### Techniques

- Benchmarking and Market Analysis
- Brainstorming
- Data Mining
- Decision Analysis
- Document Analysis
- Interviews
- Item Tracking
- Lessons Learned
- Observation
- Organizational Modelling
- Process Analysis
- Process Modelling
- Risk Analysis and Management
- Roles and Permissions Matrix
- Root Cause Analysis
- Survey or Questionnaire
- SWOT Analysis
- Workshops

### Stakeholders

- **Customer**: people directly purchasing or consuming the solution; may interact with the organization in the use of the solution.
- **Domain SME**: provides input into how the organization interacts with the solution; identifies potential limitations.
- **End User**: people who use a solution or who are a component of the solution.
- **Regulator**: governmental or professional entities ensuring adherence to laws, regulations, or rules.
- **Sponsor**: authorizes and ensures funding for solution delivery; champions action to resolve problems identified in the organizational assessment.

### Outputs

- **Enterprise Limitation**: a description of the current limitations of the enterprise including how the solution performance is impacting the enterprise.

---

## Recommend Actions to Increase Solution Value

**Task 8.5**

### Purpose

To understand the factors that create differences between potential value and actual value, and to recommend a course of action to align them.

### Description

This task focuses on understanding the aggregate of the performed assessments and identifying alternatives and actions to improve solution performance and increase value realization. Recommendations generally identify how a solution should be replaced, retired, or enhanced. They may also consider long-term effects and contributions of the solution to stakeholders, and may include recommendations to adjust the organization to allow for maximum solution performance and value realization.

### Inputs

- **Enterprise Limitation**: a description of the current limitations of the enterprise.
- **Solution Limitation**: a description of the current limitations of the solution including constraints and defects.

### Elements

1. **Adjust Solution Performance Measures** — In some cases, performance is considered acceptable but may not support the fulfillment of business goals and objectives. Identify and define more appropriate measures.

2. **Recommendations** — Depending on the reason for lower-than-expected performance, recommendations may include:

   - **Do Nothing**: recommended when the value of a change is low relative to the effort required, when risks of change significantly outweigh the risks of remaining in the current state, or when change is impossible with available resources or within the allotted time frame.

   - **Organizational Change**: manage attitudes, perceptions, and participation in the change. Recommendations may include:
     - Automating or simplifying the work people perform.
     - Improving access to information (greater amounts and better quality).
     - Re-engineering work activities and business rules; changes in responsibilities; outsourcing.

   - **Reduce Complexity of Interfaces**: interfaces are needed whenever work is transferred between systems or people. Reducing complexity improves understanding.

   - **Eliminate Redundancy**: different stakeholder groups may have common needs that can be met with a single solution, reducing implementation cost.

   - **Avoid Waste**: completely remove activities that do not add value, and minimize those that do not directly contribute to the final product.

   - **Identify Additional Capabilities**: solution options may offer capabilities beyond those identified in requirements. These may not be of immediate value but have potential future value.

   - **Retire the Solution**: consider replacement or retirement when:
     - Technology has reached the end of its life.
     - Services are being insourced or outsourced.
     - The solution is not fulfilling the goals for which it was created.

   - Additional factors impacting retirement/replacement decisions:
     - **Ongoing Cost versus Initial Investment**: existing solutions often have increasing costs over time; alternatives may have higher upfront investment but lower maintenance costs.
     - **Opportunity Cost**: the potential value that could be realized by pursuing alternative courses of action.
     - **Necessity**: most solution components have a limited lifespan due to obsolescence, changing market conditions, and other causes.
     - **Sunk Cost**: money and effort already committed — psychologically difficult to set aside, but effectively irrelevant for future decision-making. Decisions should be based on future investment required and future benefits that can be gained.

### Guidelines and Tools

- Business Objectives
- Current State Description
- Solution Scope

### Techniques

- Data Mining
- Decision Analysis
- Financial Analysis
- Focus Groups
- Organizational Modelling
- Prioritization
- Process Analysis
- Risk Analysis and Management
- Survey or Questionnaire

### Stakeholders

- **Customer**: people directly purchasing or consuming the solution.
- **Domain SME**: provides input into how to change the solution and/or the organization to increase value.
- **End User**: people who use a solution or who are a component of the solution.
- **Regulator**: ensures adherence to laws, regulations, or rules.
- **Sponsor**: authorizes and ensures funding for implementation of any recommended actions.

### Outputs

- **Recommended Actions**: recommendation of what should be done to improve the value of the solution within the enterprise.

---

## Task I/O Summary

| Task | Key Inputs | Key Output |
|---|---|---|
| 8.1 Measure Solution Performance | Business Objectives, Implemented Solution | Solution Performance Measures |
| 8.2 Analyze Performance Measures | Potential Value, Solution Performance Measures | Solution Performance Analysis |
| 8.3 Assess Solution Limitations | Implemented Solution, Solution Performance Analysis | Solution Limitation |
| 8.4 Assess Enterprise Limitations | Current State Description, Implemented Solution, Solution Performance Analysis | Enterprise Limitation |
| 8.5 Recommend Actions to Increase Solution Value | Enterprise Limitation, Solution Limitation | Recommended Actions |

### Task Flow

```
8.1 Measure Solution Performance → 8.2 Analyze Performance Measures
                                     ↓                           ↓
                              8.3 Assess Solution       8.4 Assess Enterprise
                                  Limitations               Limitations
                                     ↓                           ↓
                              8.5 Recommend Actions to Increase Solution Value
```

---

## Related Chapters

- [[00_Introduction_to_BABOK]] — Business Analysis Core Concept Model (BACCM), key terms, requirements classification schema
- [[01_BA_Planning_and_Monitoring]] — Planning the business analysis approach, including governance and performance improvements
- [[02_Elicitation_and_Collaboration]] — Eliciting information from stakeholders about solution performance
- [[03_Requirements_Life_Cycle_Management]] — Managing requirements that inform performance measurement baselines
- [[04_Strategy_Analysis]] — Defining change strategy, future state, and potential value benchmarks used in evaluation
- [[05_Requirements_Analysis_and_Design]] — Specifying and modeling requirements that shape the designed solution
