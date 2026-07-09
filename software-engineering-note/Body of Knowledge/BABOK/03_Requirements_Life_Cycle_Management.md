---
tags:
- requirements
- life-cycle
- business-analysis
- babok
---

# Requirements Life Cycle Management

> *Source: BABOK® Guide v3 by IIBA, Chapter 4 — Requirements Life Cycle Management*

## Purpose

Requirements Life Cycle Management ensures that requirements and designs are aligned, maintained, prioritized, assessed for change, and approved throughout the initiative. It bridges the gap between initial elicitation and ultimate solution delivery, governing how requirements evolve from inception through implementation. The five tasks form an ongoing cycle: trace relationships, maintain accuracy, prioritize value, assess proposed changes, and secure stakeholder approval.

The BACCM™ (Business Analysis Core Concept Model™) frames this knowledge area through six core concepts: **Change** (managing how proposed changes are evaluated), **Need** (tracing/prioritizing/maintaining requirements so needs are met), **Solution** (tracing requirements to solution components), **Stakeholder** (maintaining understanding, agreement, and approval), **Value** (reuse extends value beyond the current initiative), and **Context** (analyzing context to support tracing and prioritization).

## Trace Requirements

### Purpose
Ensure that requirements and designs at different levels are aligned to one another, and manage the effects of change to one level on related requirements.

### Description
Requirements traceability identifies and documents the lineage of each requirement — backward traceability (to higher-level needs), forward traceability (to design, build, test), and relationships to other requirements. Traceability helps ensure solution conformance and assists in scope, change, risk, time, cost, and communication management. It detects missing functionality and identifies implemented functionality unsupported by any requirement.

Traceability enables faster impact analysis, more reliable discovery of inconsistencies and gaps, deeper insights into scope and complexity of change, and reliable assessment of which requirements have been addressed. The business analyst balances the number of relationship types maintained against the benefit gained.

### Inputs
- **Requirements**: May be traced to other requirements (goals, objectives, business, stakeholder, solution, transition requirements), solution components, visuals, business rules, and other work products.
- **Designs**: May be traced to other requirements, solution components, and other work products.

### Elements
1. **Level of Formality**: The effort to trace requirements grows significantly with the number of requirements or the level of formality. Business analysts consider the value each link delivers and the nature of the specific relationships.
2. **Relationships**: Types include:
   - **Derive**: A requirement is derived from another (links different levels of abstraction, e.g., solution requirement derived from business requirement).
   - **Depends**: One requirement depends on another. Subtypes: *Necessity* (only makes sense to implement together) and *Effort* (easier to implement if a related requirement is implemented).
   - **Satisfy**: Relationship between an implementation element and the requirements it satisfies (e.g., functional requirement to solution component).
   - **Validate**: Relationship between a requirement and a test case or element that verifies fulfillment.
3. **Traceability Repository**: Documented and maintained per the business analysis approach. Requirements management tools provide significant benefits for large numbers of requirements.

### Guidelines and Tools
- **Domain Knowledge**: Expertise in the business domain to support traceability.
- **Information Management Approach**: Decisions from planning concerning the traceability approach.
- **Legal/Regulatory Information**: Legislative rules that may influence traceability rules.
- **Requirements Management Tools/Repository**: From simple text documents to dedicated tools.

### Techniques
- **Business Rules Analysis**: Trace business rules to requirements they support.
- **Functional Decomposition**: Break down solution scope; trace high-level concepts to low-level.
- **Process Modelling**: Visually show future state processes; trace requirements to processes.
- **Scope Modelling**: Visually depict scope; trace requirements to the scope areas they support.

### Stakeholders
- **Customers**: Affected by how/when requirements are implemented; may need to agree to traceability relationships.
- **Domain SME**: Recommendations on linking requirements to solution components or releases.
- **End User**: May require specific dependency relationships for implementation sequence.
- **Implementation SME**: Traceability ensures the solution meets business need; reveals implementation dependencies.
- **Operational Support**: Traceability documentation as a reference for help desk support.
- **Project Manager**: Traceability supports project change and scope management.
- **Sponsor**: Approves various relationships.
- **Suppliers**: Affected by how/when requirements are implemented.
- **Tester**: Understands how/where requirements are implemented for test planning; may trace test cases to requirements.

### Outputs
- **Requirements (traced)**: Clearly defined relationships to other requirements, solution components, or releases/phases/iterations.
- **Designs (traced)**: Clearly defined relationships such that coverage and change effects are identifiable.

## Maintain Requirements

### Purpose
Retain requirement accuracy and consistency throughout and beyond the change during the entire requirements life cycle, and support reuse of requirements in other solutions.

### Description
Requirements representing ongoing needs must be maintained to remain valid over time. To maximize the benefits of maintenance and reuse, requirements should be consistently represented, reviewed and approved using a standardized process with proper access rights, and easily accessible and understandable.

### Inputs
- **Requirements**: Goals, objectives, business, stakeholder, solution, and transition requirements.
- **Designs**: Maintained throughout their life cycle as needed.

### Elements
1. **Maintain Requirements**: Requirements are kept correct and current after approved changes. They must be clearly named, defined, and easily available to stakeholders. Relationships among requirements and associated business analysis information are also maintained to preserve context and original intent.
2. **Maintain Attributes**: Requirement attributes (source, priority, complexity) aid in managing each requirement. Some attributes change as more information is uncovered, even if the requirement itself does not change.
3. **Reusing Requirements**: Candidates for long-term use are identified, clearly named, defined, and stored for easy retrieval. Reuse can occur within the current initiative, across similar initiatives, within similar departments, or throughout the entire organization. Requirements at high levels of abstraction (without direct ties to specific tools or organizational structures) tend to be more reusable. Stakeholders validate proposed requirements for reuse before acceptance.

### Guidelines and Tools
- **Information Management Approach**: Indicates how requirements will be managed for reuse.

### Techniques
- **Business Rules Analysis**: Identify business rules that may be similar across the enterprise.
- **Data Flow Diagrams**: Identify information flow patterns for reuse.
- **Data Modelling**: Identify data structures for reuse.
- **Document Analysis**: Analyze existing documentation as the basis for maintaining and reusing requirements.
- **Functional Decomposition**: Identify requirements associated with reusable components.
- **Process Modelling**: Identify requirements associated with reusable processes.
- **Use Cases and Scenarios**: Identify solution components utilized by more than one solution.
- **User Stories**: Identify requirements associated with stories available for reuse.

### Stakeholders
- **Domain SME**: Regularly references maintained requirements to ensure they accurately reflect stated needs.
- **Implementation SME**: Uses maintained requirements for regression tests and impact analysis.
- **Operational Support**: References maintained requirements to confirm the current state.
- **Regulator**: References maintained requirements to confirm compliance to standards.
- **Tester**: Uses maintained requirements for test plan and test case creation.

### Outputs
- **Requirements (maintained)**: Defined once, available for long-term usage; may become organizational process assets or be used in future initiatives.
- **Designs (maintained)**: Reusable once defined, e.g., self-contained components for future use.

## Prioritize Requirements

### Purpose
Rank requirements in the order of relative importance.

### Description
Prioritization determines the relative importance of requirements to stakeholders. Priority can refer to relative value or to the sequence of implementation. It is an ongoing process — priorities change as the context changes. Inter-dependencies between requirements are identified and used as a basis for prioritization. Prioritization is critical to ensure maximum value is achieved.

### Inputs
- **Requirements**: Any requirements in text, matrices, or diagrams ready to prioritize.
- **Designs**: Any designs in text, prototypes, or diagrams ready to prioritize.

### Elements
1. **Basis for Prioritization**: Agreed upon by relevant stakeholders. Typical factors include:
   - **Benefit**: Advantage to stakeholders as measured against goals/objectives. Different stakeholder groups may perceive benefits differently, requiring conflict resolution and negotiation.
   - **Penalty**: Consequences of not implementing a requirement (e.g., regulatory demands, customer dissatisfaction).
   - **Cost**: Effort and resources needed. Customers may change priority after learning cost.
   - **Risk**: Chance the requirement cannot deliver potential value. High-risk requirements may be prioritized early to minimize resources spent before discovering infeasibility. Proof of concept may be developed.
   - **Dependencies**: Relationships where one requirement cannot be fulfilled unless another is fulfilled. Dependencies may be external (team decisions, funding, resource availability).
   - **Time Sensitivity**: The "best before" date after which value significantly decreases (time-to-market, seasonal functionality).
   - **Stability**: Likelihood the requirement will change. Unstable requirements may have lower priority to minimize rework.
   - **Regulatory or Policy Compliance**: Requirements that must be implemented to meet regulatory/policy demands, taking precedence over other interests.
2. **Challenges of Prioritization**: Stakeholders may value different things, causing conflict. Stakeholders may have difficulty characterizing any requirement as lower priority, impacting trade-off ability. Stakeholders may intentionally or unintentionally influence prioritization toward desired outcomes.
3. **Continual Prioritization**: Priorities shift as context evolves and more information becomes available. Initially at a higher abstraction level, then more granular as requirements are refined. Different bases may apply at different stages (e.g., benefit-based initially, then technical dependency-based by implementation team, then cost-adjusted by stakeholders).

### Guidelines and Tools
- **Business Constraints**: Regulatory statutes, contractual obligations, business policies defining priorities.
- **Change Strategy**: Information on costs, timelines, value realization for determining priority.
- **Domain Knowledge**: Expertise to support prioritization.
- **Governance Approach**: Outlines the approach for prioritizing requirements.
- **Requirements Architecture**: Understand relationships with other requirements and work products.
- **Requirements Management Tools/Repository**: Priority attribute helps sort and access requirements.
- **Solution Scope**: Considered to ensure scope is managed.

### Techniques
- **Backlog Management**: Compare requirements; backlog as location where prioritization is maintained.
- **Business Cases**: Assess requirements against business goals and objectives.
- **Decision Analysis**: Identify high-value requirements.
- **Estimation**: Produce estimates for the basis of prioritization.
- **Financial Analysis**: Assess financial value and how timing affects that value.
- **Interviews**: Understand individual or small-group stakeholders' priorities.
- **Item Tracking**: Track issues raised during prioritization.
- **Prioritization**: Facilitate the process of prioritization itself.
- **Risk Analysis and Management**: Understand risks for the basis of prioritization.
- **Workshops**: Understand stakeholder priorities in a facilitated group setting.

### Stakeholders
- **Customer**: Verifies prioritized requirements deliver value from a customer/end-user perspective; can negotiate changes.
- **End User**: Verifies prioritized requirements deliver value from an end-user perspective.
- **Implementation SME**: Provides input on technical dependencies; negotiates changes based on technical constraints.
- **Project Manager**: Uses prioritization as input into project plan and allocation of requirements to releases.
- **Regulator**: Verifies prioritization is consistent with legal and regulatory constraints.
- **Sponsor**: Verifies prioritized requirements deliver value from an organizational perspective.

### Outputs
- **Requirements (prioritized)**: Ranked requirements available for additional work, highest-valued addressed first.
- **Designs (prioritized)**: Ranked designs available for additional work, highest-valued addressed first.

## Assess Requirements Changes

### Purpose
Evaluate the implications of proposed changes to requirements and designs.

### Description
Performed as new needs or possible solutions are identified — which may or may not align with the change strategy or solution scope. Assessment determines whether a proposed change will increase solution value and what action should be taken. Business analysts assess the potential effect on solution value, whether changes introduce conflicts with other requirements or increase risk, and ensure each proposed change traces back to a need.

When assessing changes, business analysts consider if each proposed change: aligns with overall strategy, affects value delivered, impacts time/resources to deliver, and alters risks, opportunities, or constraints.

### Inputs
- **Proposed Change**: Can be identified at any time; triggers include business strategy changes, stakeholders, legal requirements, or regulatory changes.
- **Requirements**: May need assessment to identify impact of a proposed modification.
- **Designs**: May need assessment to identify impact of a proposed modification.

### Elements
1. **Assessment Formality**: Determined by information available, apparent importance of the change, and the governance process. Predictive approaches typically use more formal assessment (changes can disrupt completed work). Adaptive approaches may require less formality (iterative/incremental implementation minimizes impact).
2. **Impact Analysis**: Performed to assess the effect of a change. Traceability is a key tool. When a requirement changes, its relationships to other requirements or components are reviewed. Impact is assessed by considering:
   - **Benefit**: Gain from accepting the change.
   - **Cost**: Total cost including implementation, associated rework, and opportunity costs (features sacrificed/deferred).
   - **Impact**: Number of customers or business processes affected.
   - **Schedule**: Impact to existing delivery commitments.
   - **Urgency**: Level of importance, including factors like regulatory or safety issues.
3. **Impact Resolution**: Various stakeholders may be authorized to approve, deny, or defer the proposed change. All impacts and resolutions are documented and communicated to all stakeholders per the governance approach.

### Guidelines and Tools
- **Change Strategy**: Purpose and direction for changes; establishes context and critical components.
- **Domain Knowledge**: Expertise in the business domain for assessing proposed changes.
- **Governance Approach**: Guidance on change control, decision-making processes, and stakeholder roles.
- **Legal/Regulatory Information**: Legislative rules that may impact requirements changes.
- **Requirements Architecture**: Understanding requirement relationships to determine which will be impacted.
- **Solution Scope**: Must be considered to fully understand the impact of a proposed change.

### Techniques
- **Business Cases**: Justify a proposed change.
- **Business Rules Analysis**: Assess changes to business policies and rules.
- **Decision Analysis**: Facilitate the change assessment process.
- **Document Analysis**: Analyze existing documents to understand impact.
- **Estimation**: Determine the size of the change.
- **Financial Analysis**: Estimate financial consequences of a proposed change.
- **Interface Analysis**: Identify interfaces affected by the change.
- **Interviews**: Understand impact from individual or small-group stakeholders.
- **Item Tracking**: Track issues or conflicts discovered during impact analysis.
- **Risk Analysis and Management**: Determine the level of risk associated with the change.
- **Workshops**: Understand impact or resolve changes in a group setting.

### Stakeholders
- **Customer**: Provides feedback on the impact the change will have on value.
- **Domain SME**: Provides insight into how the change will impact the organization and value.
- **End User**: Offers information about the impact of the change on their activities.
- **Operational Support**: Provides information on ability to support the solution and need to understand the change.
- **Project Manager**: Reviews the assessment to determine if additional project work is required.
- **Regulator**: Changes are referenced by auditors to confirm compliance to standards.
- **Sponsor**: Accountable for solution scope; provides insight for assessing change.
- **Tester**: Consulted for establishing impact of the proposed changes.

### Outputs
- **Requirements Change Assessment**: The recommendation to approve, modify, or deny a proposed change to requirements.
- **Designs Change Assessment**: The recommendation to approve, modify, or deny a proposed change to design components.

## Approve Requirements

### Purpose
Obtain agreement on and approval of requirements and designs for business analysis work to continue and/or solution construction to proceed.

### Description
Business analysts ensure clear communication of requirements, designs, and other business analysis information to key stakeholders responsible for approval. Approval may be formal or informal. Predictive approaches typically perform approvals at the end of phases or during planned change control meetings. Adaptive approaches typically approve requirements only when construction and implementation can begin. Business analysts work with stakeholders to gain consensus, communicate outcomes, and track and manage approval.

### Inputs
- **Requirements (verified)**: Requirements verified to be of sufficient quality for further specification and development.
- **Designs**: Designs determined as ready for further specification and development.

### Elements
1. **Understand Stakeholder Roles**: The approval process is defined by Plan Business Analysis Governance. Business analysts must understand who holds decision-making responsibility, who possesses sign-off authority, and which influential stakeholders should be consulted or informed. Few stakeholders may have approval authority, but many can influence decisions.
2. **Conflict and Issue Management**: Consensus among stakeholders is usually sought prior to requesting approval. Stakeholder groups frequently have varying points of view and conflicting priorities. The business analyst facilitates communication between stakeholders in areas of conflict so each group appreciates the needs of others. Conflict resolution occurs often during review and sign-off.
3. **Gain Consensus**: Business analysts ensure stakeholders with approval authority understand and accept the requirements. Approval confirms stakeholders believe sufficient value will be created to justify investment. Business analysts present requirements using established methods, facilitate the approval process, and address questions or provide additional information. Complete agreement may not be necessary, but associated risks of disagreement must be identified and managed.
4. **Track and Communicate Approval**: Approval decisions are recorded, possibly in maintenance and tracking tools. Accurate records of current approval status must be maintained. Stakeholders must know what is currently approved and in line for implementation. An audit history of changes (what changed, who made the change, why, when) may provide additional value.

### Guidelines and Tools
- **Change Strategy**: Assists in managing stakeholder consensus regarding the needs of all stakeholders.
- **Governance Approach**: Identifies stakeholders with authority/responsibility to approve and explains when/how approvals align to organizational policies.
- **Legal/Regulatory Information**: Legislative rules that may impact the approval process.
- **Requirements Management Tools/Repository**: Tool to record requirements approvals.
- **Solution Scope**: Must be considered to accurately assess alignment and completeness.

### Techniques
- **Acceptance and Evaluation Criteria**: Define approval criteria.
- **Decision Analysis**: Resolve issues and gain agreement.
- **Item Tracking**: Track issues identified during the agreement process.
- **Reviews**: Evaluate requirements.
- **Workshops**: Facilitate obtaining approval.

### Stakeholders
- **Customer**: May actively review and approve requirements and designs to ensure needs are met.
- **Domain SME**: May be involved in review and approval as defined by stakeholder roles designation.
- **End User**: May be involved in review, validation, and prioritization as defined by stakeholder roles designation.
- **Operational Support**: Responsible for ensuring requirements/designs are supportable within technology standards; may have a review/approval role.
- **Project Manager**: Identifies and manages risks; may manage project plan activities for review and/or approval.
- **Regulator**: Provides opinions on the relationship between stated requirements and specific regulations.
- **Sponsor**: Responsible to review and approve the business case, solution/product scope, and all requirements and designs.
- **Tester**: Ensures quality assurance standards are feasible; verifies requirements have the testable characteristic.

### Outputs
- **Requirements (approved)**: Requirements agreed to by stakeholders, ready for use in subsequent business analysis efforts.
- **Designs (approved)**: Designs agreed to by stakeholders, ready for use in subsequent business analysis or solution development efforts.

## Related Chapters

- [[00_Introduction_to_BABOK]]: Provides the BACCM™ foundation that frames the six core concepts governing requirements life cycle management.
- [[01_BA_Planning_and_Monitoring]]: Defines the governance approach, information management approach, and change strategy that drive how requirements are traced, maintained, prioritized, assessed, and approved.
- [[02_Elicitation_and_Collaboration]]: Produces the requirements and designs that enter the life cycle management tasks; collaboration maintains stakeholder engagement through the life cycle.
- [[04_Strategy_Analysis]]: Defines the future state and change strategy; outputs from strategy analysis feed into prioritization, change assessment, and traceability.
- [[05_Requirements_Analysis_and_Design]]: Refines and structures requirements and designs that the life cycle management tasks maintain, trace, and govern.
- [[06_Solution_Evaluation]]: Assesses whether implemented requirements deliver value; feeds back into the life cycle for maintenance and potential change assessment.
