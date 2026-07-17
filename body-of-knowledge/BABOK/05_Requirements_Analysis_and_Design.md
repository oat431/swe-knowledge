---
tags:
- requirements
- design
- business-analysis
- babok
---

# Requirements Analysis and Design Definition

> **Source:** *BABOK® Guide v3 by IIBA, Chapter 6 — Requirements Analysis and Design Definition*
>
> **Purpose:** This knowledge area describes the tasks business analysts perform to structure and organize requirements discovered during elicitation, specify and model requirements and designs, validate and verify information, identify solution options that meet business needs, and estimate the potential value that could be realized for each solution option. It covers the full spectrum from initial elicitation results through to a recommended solution.

## Purpose

The Requirements Analysis and Design Definition knowledge area transforms elicitation results into well-structured, high-quality requirements and designs. It ensures requirements collectively support business objectives, are of sufficient quality to guide further work, align to business value, and that the best solution option is recommended. The knowledge area sits between Strategy Analysis (identifying the need) and Solution Evaluation (assessing the implemented solution), representing the bridge from potential value to actual value through defined requirements, designs, and solution recommendations.

### Core Concept Model (BACCM™)

| Core Concept | During Requirements Analysis and Design Definition, business analysts... |
|---|---|
| **Change** | transform elicitation results into requirements and designs in order to define the change. |
| **Need** | analyze the needs in order to recommend a solution that meets the needs. |
| **Solution** | define solution options and recommend the one most likely to address the need and has the most value. |
| **Stakeholder** | tailor the requirements and designs so they are understandable and usable by each stakeholder group. |
| **Value** | analyze and quantify the potential value of the solution options. |
| **Context** | model and describe the context in formats understandable and usable by all stakeholders. |

---

## Specify and Model Requirements

### Purpose

To analyze, synthesize, and refine elicitation results into requirements and designs.

### Description

This task describes the practices for analyzing elicitation results and creating representations of those results. When the focus is on understanding the need, outputs are called **requirements**. When the focus is on a solution, outputs are called **designs**. In many IT environments, all business deliverables are referred to as "requirements," and the word "design" is reserved for technical designs by developers and architects.

Specifying and modelling activities include capturing attributes/metadata about requirements and relate to all requirement types.

### Inputs

- **Elicitation Results (any state):** modelling can begin with any elicitation result and may lead to more elicitation. Elicitation and modelling may occur sequentially, iteratively, or concurrently.

### Elements

1. **Model Requirements** — A model is a descriptive and visual way to convey information to support analysis, communication, and understanding. Models confirm knowledge, identify information gaps, and identify duplicate information.

   Modelling formats:
   - **Matrices:** for requirements with complex but uniform structure (data dictionaries, traceability, gap analysis, prioritization, attributes).
   - **Diagrams:** visual representations for depicting complexity, defining boundaries, categorizing/hierarchies, and showing data components and relationships.

   Model categories:
   - **People and Roles:** Organizational Modelling, Roles and Permissions Matrix, Stakeholder List/Map/Personas.
   - **Rationale:** Decision Modelling, Scope Modelling, Business Model Canvas, Root Cause Analysis, Business Rules Analysis.
   - **Activity Flow:** Process Modelling, Use Cases and Scenarios, User Stories.
   - **Capability:** Business Capability Analysis, Functional Decomposition, Prototyping.
   - **Data and Information:** Data Dictionary, Data Flow Diagrams, Data Modelling, Glossary, State Modelling, Interface Analysis.

2. **Analyze Requirements** — Decompose business analysis information to examine what must change, what should stay the same, missing components, unnecessary components, and constraints/assumptions. The level of decomposition varies by stakeholder knowledge, misunderstanding risk, organizational standards, and regulatory obligations.

3. **Represent Requirements and Attributes** — Explicitly represent requirements with enough detail to exhibit quality characteristics. Specify attributes per the information management plan. Categorize requirements according to the classification schema to ensure completeness and appropriate traceability between types.

4. **Implement the Appropriate Levels of Abstraction** — Vary the level of abstraction based on requirement type and audience. Produce different viewpoints for different stakeholders while maintaining the meaning and intent of requirements across all representations.

### Guidelines and Tools

- **Modelling Notations/Standards:** enable precise specification appropriate for audience and purpose.
- **Modelling Tools:** software for drawing and storing matrices and diagrams.
- **Requirements Architecture:** interrelationships among requirements to ensure model completeness and consistency.
- **Requirements Life Cycle Management Tools:** software for recording, organizing, storing, and sharing requirements.
- **Solution Scope:** provides boundaries for requirements and design models.

### Techniques

Acceptance and Evaluation Criteria, Business Capability Analysis, Business Model Canvas, Business Rules Analysis, Concept Modelling, Data Dictionary, Data Flow Diagrams, Data Modelling, Decision Modelling, Functional Decomposition, Glossary, Interface Analysis, Non-Functional Requirements Analysis, Organizational Modelling, Process Modelling, Prototyping, Roles and Permissions Matrix, Root Cause Analysis, Scope Modelling, Sequence Diagrams, Stakeholder List/Map/Personas, State Modelling, Use Cases and Scenarios, User Stories.

### Stakeholders

- **Any stakeholder:** business analysts may perform this task themselves and package/communicate requirements for review, or invite stakeholders to participate.

### Outputs

- **Requirements (specified and modelled):** any combination of requirements and/or designs in text, matrices, and diagrams.

---

## Verify Requirements

### Purpose

To ensure that requirements and designs specifications and models meet quality standards and are usable for the purpose they serve.

### Description

Verification constitutes a check that requirements and designs have been defined correctly. It determines that requirements are ready for validation and provides information for further work. A high-quality specification is well-written and easily understood; a high-quality model follows notation standards and effectively represents reality. The most important characteristic is **fitness for use** — quality is ultimately determined by stakeholders.

### Inputs

- **Requirements (specified and modelled):** any requirement, design, or set may be verified for well-structured text and correct use of matrices and modelling notation.

### Elements

1. **Characteristics of Requirements and Designs Quality:**
   - **Atomic:** self-contained and independently understandable.
   - **Complete:** enough to guide further work at the appropriate level of detail.
   - **Consistent:** aligned with stakeholder needs and not conflicting with other requirements.
   - **Concise:** contains no extraneous or unnecessary content.
   - **Feasible:** reasonable and possible within agreed risk, schedule, and budget.
   - **Unambiguous:** clearly stated so it is clear whether a solution meets the need.
   - **Testable:** able to verify that the requirement has been fulfilled.
   - **Prioritized:** ranked, grouped, or negotiated in terms of importance and value.
   - **Understandable:** represented using common terminology of the audience.

2. **Verification Activities** — performed iteratively throughout analysis:
   - Checking compliance with organizational performance standards for business analysis.
   - Checking correct use of modelling notation, templates, or forms.
   - Checking for completeness within each model.
   - Comparing each model against other relevant models for missing or inconsistently referenced elements.
   - Ensuring terminology is understandable to stakeholders and consistent within the organization.
   - Adding examples where appropriate for clarification.

3. **Checklists** — used for quality control during verification. May include standard quality elements or be specifically developed to capture issues of concern. Ensures important items are included in final deliverables and required verification steps are followed.

### Guidelines and Tools

- **Requirements Life Cycle Management Tools:** some tools have functionality to check for issues related to characteristics such as atomic, unambiguous, and prioritized.

### Techniques

- **Acceptance and Evaluation Criteria:** ensure requirements are stated clearly enough to devise tests that prove requirements have been met.
- **Item Tracking:** ensure problems or issues identified during verification are managed and resolved.
- **Metrics and Key Performance Indicators (KPIs):** identify how to evaluate the quality of requirements.
- **Reviews:** inspect requirements documentation to identify requirements not of acceptable quality.

### Stakeholders

- **All stakeholders:** the business analyst, domain and implementation SMEs have primary responsibility. Other stakeholders may discover problematic requirements during communication.

### Outputs

- **Requirements (verified):** a set of requirements or designs of sufficient quality to be used as a basis for further work.

---

## Validate Requirements

### Purpose

To ensure that all requirements and designs align to the business requirements and support the delivery of needed value.

### Description

Validation is an ongoing process ensuring that stakeholder, solution, and transition requirements align to business requirements and that designs satisfy the requirements. Understanding the stakeholders' desired future state is valuable during validation. Stakeholders often have different, conflicting needs and expectations that may be exposed through the validation process. Validation activities cannot be completed before requirements are completely verified.

### Inputs

- **Requirements (specified and modelled):** any types can be validated. Validation may begin before requirements are completely verified but cannot be completed until verification is done.

### Elements

1. **Identify Assumptions** — When launching unprecedented products/services, assumptions about stakeholder response may be necessary. Assumptions about benefits resulting from requirement implementation are identified and defined so associated risks can be managed.

2. **Define Measurable Evaluation Criteria** — Define the evaluation criteria used to assess how successful the change has been after implementation. Baseline metrics may be established based on the current state; target metrics are developed to reflect achievement of business objectives.

3. **Evaluate Alignment with Solution Scope** — A requirement that does not deliver benefit to a stakeholder is a strong candidate for elimination. When requirements do not align, either the future state must be re-evaluated and the solution scope changed, or the requirement removed from scope. If a design cannot be validated to support a requirement, there may be a missing or misunderstood requirement, or the design must change.

### Guidelines and Tools

- **Business Objectives:** ensure requirements deliver desired business benefits.
- **Future State Description:** ensures requirements within solution scope help achieve the desired future state.
- **Potential Value:** used as a benchmark against which value delivered by requirements can be assessed.
- **Solution Scope:** ensures beneficial requirements are within scope.

### Techniques

- **Acceptance and Evaluation Criteria:** define quality metrics that must be met to achieve stakeholder acceptance.
- **Document Analysis:** identify previously documented business needs to validate requirements.
- **Financial Analysis:** define financial benefits associated with requirements.
- **Item Tracking:** ensure problems or issues identified during validation are managed and resolved.
- **Metrics and Key Performance Indicators (KPIs):** select appropriate performance measures.
- **Reviews:** confirm whether stakeholders agree their needs are met.
- **Risk Analysis and Management:** identify scenarios that would alter the benefit delivered by a requirement.

### Stakeholders

- **All stakeholders:** business analyst, customer, end users, and sponsors have primary responsibility. Virtually all project stakeholders are involved.

### Outputs

- **Requirements (validated):** requirements and designs demonstrated to deliver benefit to stakeholders and align with business goals and objectives. If a requirement cannot be validated, it either does not benefit the organization, does not fall within solution scope, or both.

---

## Define Requirements Architecture

### Purpose

To ensure that the requirements collectively support one another to fully achieve the objectives.

### Description

Requirements architecture is the structure of all requirements of a change. It fits individual models and specifications together to ensure all requirements form a single whole supporting overall business objectives and producing a useful outcome. Business analysts use requirements architecture to: understand which models are appropriate; organize requirements for different stakeholders; illustrate how requirements/models interact and relate; ensure requirements work together to achieve objectives; and make trade-off decisions considering overall objectives.

Requirements architecture shows how elements work in harmony — it is broader than traceability. Traceability proves each requirement links back to an objective but does not prove the solution is a cohesive whole that will work.

### Inputs

- **Information Management Approach:** defines how business analysis information will be stored and accessed.
- **Requirements (any state):** every requirement should be stated once and incorporated into the architecture for completeness evaluation.
- **Solution Scope:** must be considered to ensure architecture is aligned with solution boundaries.

### Elements

1. **Requirements Viewpoints and Views** — A **viewpoint** is a set of conventions defining how requirements will be represented, organized, and related. Viewpoints provide templates addressing concerns of particular stakeholder groups, including standards for model types, attributes, notations, and analytical approaches for relationships.

   Example viewpoints: business process models, data models and information, user interactions (use cases, user experience), audit and security, business models.

   No single viewpoint alone forms an entire architecture. Each is stronger for some aspects and weaker for others. The actual requirements and designs for a solution from a chosen viewpoint are referred to as a **view**. A collection of views makes up the requirements architecture. Viewpoints tell business analysts *what* information to provide; views describe the *actual* requirements and designs produced.

2. **Template Architectures** — An architectural framework is a collection of viewpoints standard across an industry, sector, or organization. Frameworks serve as predefined templates; when populated with domain-specific information, they form even more useful templates.

3. **Completeness** — The architecture helps ensure the entire set of requirements is complete, cohesive, and tells a full story. No requirements should be missing, inconsistent, or contradictory. Dependencies between requirements that could keep objectives from being achieved must be accounted for. Structuring requirements according to different viewpoints helps ensure completeness; iterations of elicitation, specification, and analysis help identify gaps.

4. **Relate and Verify Requirements Relationships** — Examine and analyze requirements to define relationships. Relationship quality criteria:
   - **Defined:** the relationship exists and its type is described.
   - **Necessary:** the relationship is necessary for holistic understanding.
   - **Correct:** the elements actually have the relationship described.
   - **Unambiguous:** no relationships link elements in two different and conflicting ways.
   - **Consistent:** relationships are described using the same standard descriptions as defined in viewpoints.

5. **Business Analysis Information Architecture** — The structure of business analysis information, defined in Plan Business Analysis Information Management. It describes how all business analysis information (requirements, designs, models, elicitation results) relates, helping ensure the full set of requirements is complete.

### Guidelines and Tools

- **Architecture Management Software:** helps manage volume, complexity, and versions of relationships.
- **Legal/Regulatory Information:** legislative rules or regulations that may impact the architecture or its outputs.
- **Methodologies and Frameworks:** predetermined sets of models and relationships for representing different viewpoints.

### Techniques

- **Data Modelling:** describe requirements structure as it relates to data.
- **Functional Decomposition:** break down organizational units, product scope, or elements into component parts.
- **Interviews:** define requirements structure collaboratively.
- **Organizational Modelling:** understand organizational units, stakeholders, and relationships to define relevant viewpoints.
- **Scope Modelling:** identify elements and boundaries of the requirements architecture.
- **Workshops:** define requirements structure collaboratively.

### Stakeholders

- **Domain SME, Implementation SME, Project Manager, Sponsor, Tester:** assist in defining and confirming the requirements architecture.
- **Any stakeholder:** may use the architecture to assess completeness of requirements.

### Outputs

- **Requirements Architecture:** the requirements and their interrelationships, plus any contextual information recorded.

---

## Define Design Options

### Purpose

To define the solution approach, identify opportunities to improve the business, allocate requirements across solution components, and represent design options that achieve the desired future state.

### Description

When designing a solution, one or more design options may be identified. Each design option represents a way to satisfy a set of requirements. Design options exist at a lower level than the change strategy and are tactical rather than strategic. As initiatives progress and requirements evolve, design options evolve as well. Business analysts must assess the effect of tactical trade-offs on value delivery.

### Inputs

- **Change Strategy:** describes the approach to transition to the future state; may impact design decisions regarding feasibility.
- **Requirements (validated, prioritized):** only validated requirements are considered. Priorities aid in suggesting reasonable design options — higher-priority requirements deserve more weight in choosing solution components.
- **Requirements Architecture:** the full set of requirements and their relationships is important for defining design options that address the holistic set.

### Elements

1. **Define Solution Approaches** — Describes whether solution components will be created or purchased, or some combination:
   - **Create:** components are assembled, constructed, or developed by experts. Includes modifying an existing solution.
   - **Purchase:** components are selected from third-party offerings (products or services).
   - **Combination of both:** design options may include a mix of creation and purchase. Proposed integration of components is also considered within the design option.

2. **Identify Improvement Opportunities** — Common examples include:
   - **Increase Efficiencies:** automate or simplify work by re-engineering or sharing processes, changing responsibilities, or outsourcing. Automation may increase consistency of behavior.
   - **Improve Access to Information:** provide greater amounts of information to staff, reducing the need for specialists.
   - **Identify Additional Capabilities:** highlight capabilities with potential future value that can be supported by the solution.

3. **Requirements Allocation** — The process of assigning requirements to solution components and releases to best achieve objectives. Allocation is supported by assessing trade-offs between alternatives to maximize benefits and minimize costs. Value may vary depending on how requirements are implemented and when the solution becomes available. Requirements may be allocated between organizational units, job functions, solution components, or releases. Allocation typically begins when a solution approach is determined and continues through design and implementation.

4. **Describe Design Options** — Design options are investigated and developed while considering the desired future state. A design option usually consists of many design components, each described by a design element. Design elements may describe: business policies and rules, business processes, people (job functions and responsibilities), operational business decisions, software applications and components, and organizational structures (including interactions with customers and suppliers).

### Guidelines and Tools

- **Existing Solutions:** existing third-party products or services considered as components of a design option.
- **Future State Description:** identifies the desired state; helps ensure design options are viable.
- **Requirements (traced):** define the design options that best fulfill known requirements.
- **Solution Scope:** defines boundaries when selecting viable design options.

### Techniques

- **Benchmarking and Market Analysis:** identify and analyze existing solutions and market trends.
- **Brainstorming:** identify improvement opportunities and design options.
- **Document Analysis:** provide information needed to describe design options and elements.
- **Interviews:** identify improvement opportunities and design options.
- **Lessons Learned:** identify improvement opportunities.
- **Mind Mapping:** identify and explore possible design options.
- **Root Cause Analysis:** understand underlying causes of problems to propose solutions.
- **Survey or Questionnaire:** identify improvement opportunities and design options.
- **Vendor Assessment:** couple assessment of third-party solutions with vendor assessment.
- **Workshops:** identify improvement opportunities and design options.

### Stakeholders

- **Domain SME:** provides business expertise to evaluate solution alternatives and potential benefits.
- **Implementation SME:** provides input about constraints and costs of design options.
- **Operational Support:** evaluates difficulty and costs of integrating proposed solutions with existing processes and systems.
- **Project Manager:** plans and manages the solution definition process, including scope and risks.
- **Supplier:** provides information on functionality associated with a particular design option.

### Outputs

- **Design Options:** describe various ways to satisfy one or more needs in a context. May include solution approach, potential improvement opportunities, and the components defining the option.

---

## Analyze Potential Value and Recommend Solution

### Purpose

To estimate the potential value for each design option and to establish which one is most appropriate to meet the enterprise's requirements.

### Description

This task describes how to estimate and model the potential value delivered by requirements, designs, or design options. Potential value is analyzed many times over the course of a change — it may be planned or triggered by modifications to context or scope. Analysis includes consideration of uncertainty in estimates. Value can be described in terms of finance, reputation, or marketplace impact. Any change may include a mix of increases and decreases in value.

Design options are evaluated by comparing potential value. Each option has advantages and disadvantages. Depending on the reasons for change, there may be no best option, a clear best choice, or the best recommendation may be to begin work against multiple design options (e.g., proofs of concept). In other instances, all proposed designs may be rejected, or the best recommendation may be to **do nothing**.

### Inputs

- **Potential Value:** can be used as a benchmark against which value delivered by a design is evaluated.
- **Design Options:** need to be evaluated and compared to recommend one option for the solution.

### Elements

1. **Expected Benefits** — The positive value a solution is intended to deliver. Value can include benefits, reduced risk, compliance with business policies/regulations, improved user experience, or any other positive outcome. Benefits are determined by analyzing what stakeholders desire and what is possible to attain. Expected benefits can be calculated at the requirement level by considering how much of an overall business objective the set of requirements contributes to if fulfilled. The total expected benefit is the net benefit of all requirements a design option addresses. Benefits are often realized over time.

2. **Expected Costs** — Any potential negative value associated with a solution, including: timeline, effort, operating costs, purchase/implementation costs, maintenance costs, physical resources, information resources, and human resources. Expected costs for a design option consider the cumulative costs of design components.

   **Opportunity cost** is also considered: the value of the best alternative not selected. The opportunity cost of any design option equals the value of the next-best forgone alternative.

3. **Determine Value** — The potential value is based on benefits delivered minus associated costs. Value can be positive (benefits exceed costs) or negative (costs exceed benefits). Value to the enterprise is almost always more heavily weighted than value for individual stakeholder groups.

   Potential value is **uncertain value**. There are always events or conditions that could increase or decrease actual value. Many changes are proposed with intangible/uncertain benefits but tangible/absolute costs, making comparison difficult. Business analysts define complete estimates by considering tangible and intangible costs alongside tangible and intangible benefits, accounting for the degree of uncertainty at the time estimates are made.

4. **Assess Design Options and Recommend Solution** — Each design option is assessed based on expected potential value. Re-evaluation of design element allocation may become necessary as costs are better understood. Factors to consider:
   - **Available Resources:** limitations on what can be implemented based on allocated resources; a business case may justify additional investment.
   - **Constraints on the Solution:** regulatory requirements or business decisions may require certain requirements to be handled manually or automatically, or prioritized above others.
   - **Dependencies between Requirements:** some capabilities may provide limited value themselves but are needed to support other high-value requirements.
   - **Other considerations:** vendor relationships, dependencies on other initiatives, corporate culture, cash flow for investment.

   Business analysts recommend the option(s) deemed most valuable. It is possible that none of the design options are worthwhile and the best recommendation is to do nothing.

### Guidelines and Tools

- **Business Objectives:** used to calculate expected benefit.
- **Current State Description:** provides context and helps identify/quantify value to be delivered.
- **Future State Description:** ensures design options are appropriate for the desired future state.
- **Risk Analysis Results:** potential value includes assessment of risk associated with design options.
- **Solution Scope:** defines scope boundaries for relevant evaluation.

### Techniques

- **Acceptance and Evaluation Criteria:** express requirements as acceptance criteria to assess proposed solutions.
- **Backlog Management:** sequence potential value.
- **Brainstorming:** identify potential benefits collaboratively.
- **Business Cases:** assess recommendations against business goals and objectives.
- **Business Model Canvas:** understand strategy and initiatives.
- **Decision Analysis:** support assessment and ranking of design options.
- **Estimation:** forecast costs and efforts to estimate value.
- **Financial Analysis:** evaluate financial return of different options and choose the best ROI.
- **Focus Groups:** get stakeholder input on which design options best meet requirements.
- **Interviews:** get individual stakeholder input on design options and value expectations.
- **Metrics and Key Performance Indicators (KPIs):** create and evaluate measurements used in defining value.
- **Risk Analysis and Management:** identify and manage risks affecting potential value.
- **Survey or Questionnaire:** identify stakeholders' value expectations.
- **SWOT Analysis:** identify areas of strength and weakness impacting solution value.
- **Workshops:** get stakeholder input on design options and value expectations.

### Stakeholders

- **Customer:** represents market segments affected; involved in analyzing benefits and costs.
- **Domain SME:** assists in analyzing potential value and benefits, particularly for hard-to-identify requirements.
- **End User:** provides insight into potential value of the change.
- **Implementation SME:** provides expertise in implementing design options to identify potential costs and risks.
- **Project Manager:** manages the selection process and is aware of potential impacts and risks.
- **Regulator:** may be involved in risk evaluation or place constraints on potential benefits.
- **Sponsor:** approves expenditure, approves the final recommendation, and must be kept informed of changes in potential value, risk, and opportunity cost.

### Outputs

- **Solution Recommendation:** identifies the suggested, most appropriate solution based on evaluation of all defined design options. The recommended solution should maximize the value provided to the enterprise.

---

## Related Chapters

- [[04_Strategy_Analysis]]: Strategy Analysis identifies the business need and defines the change strategy that Requirements Analysis and Design Definition acts upon.
- [[06_Solution_Evaluation]]: Solution Evaluation assesses the performance and value of the implemented solution — the actual value realization of what this knowledge area defines as potential value.
- [[02_Elicitation_and_Collaboration]]: Elicitation results are the primary input that Requirements Analysis and Design Definition transforms into specified requirements and designs.
- [[01_BA_Planning_and_Monitoring]]: Planning activities (information management approach, governance) define the framework within which analysis and design tasks are performed.
- [[03_Requirements_Life_Cycle_Management]]: Requirements Life Cycle Management tasks (traceability, prioritization, approval) are closely interleaved with analysis and design activities.
