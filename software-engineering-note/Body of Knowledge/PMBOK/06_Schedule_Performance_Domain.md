---
tags: [schedule, performance-domain, project-management, pmbok]
---

> *Source: PMBOK® Guide — Eighth Edition by PMI, §2.3 Schedule Performance Domain (pp. 47–58)*

## Purpose

The **Schedule Performance Domain** provides a plan that represents *how and when* the project will deliver the products, services, and results defined in the project scope. The schedule serves as a tool for communication, stakeholder expectation management, and performance reporting. It also facilitates proactive identification of potential delays and risks, enabling timely corrective actions to keep the project on track.

## Key Concepts

### Project Schedule
An output of a schedule model that presents **linked activities** with planned dates, durations, milestones, and resources. Includes:
- Start and finish dates
- **Duration** — total number of work periods (hours, days, weeks) required to complete an activity or **WBS** component
- **Milestones** — significant points or events in the project
- Resources (see also [[09_Resources_Performance_Domain]])

### Estimate
A quantitative assessment of a variable's likely amount or outcome:
- **Effort** — number of labor units required to complete a schedule activity or WBS component
- **Duration** — number of work periods needed to complete individual activities with the estimated resources

### Schedule Baseline
The **approved version** of a schedule model, changed only through formal change control procedures. Used as the basis for comparison to actual results. Formality ranges from rigid (predictive) to flexible or nonexistent (adaptive/agile).

### Schedule Flexibility
Essential for adapting to changes, managing risks, and optimizing resource allocations. Enhances team morale, supports incremental delivery, and helps ensure higher-quality outcomes.

### Schedule Forecasts
Estimates or predictions of future conditions and events based on current progress, performance trends, and available knowledge. Updated and reissued as work performance data is obtained during execution.

### Actual Duration
The time (in calendar units) between the actual start date of a schedule activity and either the **data date** (if in progress) or the **actual finish date** (if complete).

### Project Schedule Network Diagrams
A graphical representation of the **logical relationships** (dependencies) among project schedule activities. Sequencing is typically performed using project management software.

### Progressive Elaboration & Rolling Wave Planning
With shrinking timelines, it is often impossible to develop a detailed project schedule up front. Practitioners increasingly prepare detailed schedules progressively: outlining broad milestones and key activities first, then refining as more information becomes available. **Rolling wave planning** is the well-known technique that supports this approach.

## Processes

The Schedule Performance Domain includes three processes aligned with the Planning and Monitoring & Controlling focus areas:

### 1. Plan Schedule Management
**Purpose:** Establish policies, procedures, and documentation for designing, developing, managing, performing, and maintaining the schedule.

| Category | Items |
|---|---|
| **Key Inputs** | Project charter, project management plan (scope management plan), development approach, EEFs, OPAs |
| **Tools & Techniques** | Expert judgment, data analysis (alternative analysis), meetings |
| **Key Output** | **Schedule management plan** — covering schedule development approach, release/iteration length, level of accuracy, units of measurement, links to organizational procedures, schedule maintenance, control thresholds, rules of performance measurement, and reporting formats |

### 2. Develop Schedule
**Purpose:** Analyze sequences, durations, resource requirements, and schedule constraints to create a **schedule model** for project execution and monitoring & controlling. This is an *iterative* process.

| Category | Items |
|---|---|
| **Key Inputs** | Project charter, project management plan, development approach, project documents (activity list, activity attributes, duration estimates, milestone list, assumptions log, risk register, resource calendars, resource requirements, project schedule network diagrams, project team assignments, basis of estimates, lessons learned register), agreements, EEFs, OPAs |
| **Tools & Techniques** | Expert judgment, decomposition, rolling wave planning, precedence diagramming method (PDM), logical relationships, leads & lags, dependency determination & integration, estimation techniques, reserve analysis, data analysis (what-if, simulation, alternative analysis), voting, schedule network analysis, schedule compression, **critical path method (CPM)**, critical chain method, resource optimization (resource leveling), PMIS, agile release planning |
| **Key Outputs** | Schedule baseline, project schedule, schedule data, project calendars, change requests, plan/document updates |

#### Schedule Development Steps

The four iterative steps for developing the schedule are:

**Step 1: Define Activities**
Identify and document the specific actions to produce project deliverables. Decomposes work packages (see [[05_Scope_Performance_Domain]]) into schedule activities.
- **Artifacts:** Activity list, activity attributes, milestone list.

**Step 2: Determine Sequence**
Determine the logical sequence of work by defining dependencies and relationships among schedule elements. Every activity (except first and last) should connect to at least one predecessor and one successor.
- **Dependency types:** Finish-to-Start (FS), Start-to-Start (SS), Finish-to-Finish (FF), Start-to-Finish (SF)
- **Leads & Lags** may be used to support a realistic schedule
- Sequencing can occur at different levels: control account, work package, or activity

**Step 3: Estimate Effort and Duration**
Determine the number of work periods needed to complete each activity with estimated resources.

*Estimation techniques include:*
- Expert judgment, Delphi technique
- Analogous estimating, parametric estimating
- Program Evaluation and Review Technique (**PERT**)
- Bottom-up estimating
- Planning poker, story points, T-shirt sizing (adaptive/agile)

*Factors influencing duration estimates:*
- Constraints (fixed duration, fixed effort, fixed resources)
- Team experience and risk profile
- **Law of diminishing returns** — adding more of one factor eventually yields progressively smaller output increases
- **Number of resources** — doubling resources does not halve duration; may increase it due to knowledge transfer, learning curves, and coordination overhead
- Advances in technology
- **Cognitive bias** — planning fallacy, end-of-story illusion, Hofstadter's Law, team motivation
- Level of decomposition (control account, work package, or activity level)

**Step 4: Adjust**
Review the draft schedule and apply techniques to find alternative schedule options if unacceptable.
- Team members review and confirm their assignments against resource calendars
- Analyze for logical relationship conflicts
- Determine if **resource leveling** is required
- Resource estimates from the draft may feed back into further adjustment

#### Critical Path Method (CPM)

The **Critical Path Method** is a schedule network analysis technique used to determine the minimum project duration and the amount of scheduling flexibility (float) on each network path. The critical path is the **longest path** through the network diagram and represents the shortest possible project duration — any delay on a critical path activity directly delays the project finish date.

CPM is applied during *Develop Schedule* and used again during *Monitor and Control Schedule* to assess the impact of variances and what-if scenarios.

#### Critical Chain Method
An alternative to CPM that accounts for resource constraints and builds buffers (feeding buffers, project buffer) rather than floating individual activities. Used when resource availability is the primary scheduling constraint.

#### Schedule Compression
Techniques used during the *Adjust* step — and during monitoring & controlling — to shorten the schedule without reducing scope:
- **Crashing** — adding resources to critical path activities (increases cost)
- **Fast tracking** — performing activities in parallel that would normally be sequential (increases risk)

### 3. Monitor and Control Schedule
**Purpose:** Monitor project status to update the project schedule and manage changes to the agreed-upon schedule. Maintains a realistic schedule throughout the project.

| Category | Items |
|---|---|
| **Key Inputs** | Project management plan (schedule management plan, scope baseline, performance measurement baseline), product backlog, project documents (lessons learned register, project calendars, project schedule, resource calendars, risk register, schedule data, basis of estimates), work performance data, EEFs, OPAs |
| **Tools & Techniques** | Data analysis (earned value analysis, burnup/burndown charts, performance reviews, trend analysis, variance analysis, what-if scenario analysis), CPM, critical chain method, PMIS, resource optimization, leads & lags, schedule compression, branch and bound, velocity, daily coordination meetings, sprint reviews, backlog refinement |
| **Key Outputs** | Work performance information, schedule forecasts, change requests, plan/document updates |

#### Predictive Approach (Monitoring & Controlling)
- Changes to the schedule baseline require formal **integrated change control** (via **Assess and Implement Changes**)
- Determine status of the project schedule
- Influence factors creating schedule changes
- Reconsider necessary schedule reserves
- Determine if overall schedule has changed
- Manage actual changes as they occur

#### Adaptive/Agile Approach
- Compare total work delivered and accepted against estimates for the elapsed time cycle
- Conduct **retrospectives** to correct and improve processes
- Reprioritize the remaining work plan (backlog)
- Determine **velocity** (rate of deliverable production, validation, and acceptance per iteration)
- Determine if schedule has changed and manage changes

#### Contracted Work
Regular and milestone status updates from contractors, plus scheduled status reviews and walkthroughs, ensure contractor reports are accurate and the schedule remains under control.

## Tailoring Considerations

### Life Cycle and Development Approach
- **Predictive:** Schedule defined up front; detailed planning, baseline created, formal change control; tailoring sets comprehensive timeline with milestones, dependencies, and critical paths.
- **Adaptive:** Flexible, timeboxed schedules divided into sprints/iterations with short-term planning horizons; tailoring adapts to scope and priority changes.
- **Hybrid:** High-level predictive milestones with subordinate adaptive/predictive streams; combines Kanban, CPM, and iterative methods for flexibility and structure.

### Product and Deliverable Attributes
- **Compliance/criticality:** High-criticality projects need more detailed and frequent schedule updates.
- **Type and technology:** New or innovative technologies require more iterative scheduling due to uncertainty.

### Project Team Attributes
- **Size and distribution:** Large, dispersed teams need sophisticated scheduling tools and regular updates.
- **Experience:** Experienced teams may use less-detailed schedules; less-experienced teams require more detailed, step-by-step plans.

### Culture
- **Organizational culture:** Tolerance for uncertainty affects schedule rigidity (adaptive culture → fluid schedules; risk-averse culture → detailed, fixed schedules). Industry drivers: pharmaceuticals follow strict processes; technology companies are more risk-aggressive.
- **Buy-in and trust:** High trust enables flexible scheduling; low trust requires formal, detailed scheduling.

### Project Environment
- **Scale and environment:** Megaprojects vs. small projects; commercial, government, or nonprofit environments influence detail and update frequency.
- **Goals and timeframe:** Aggressive timelines or critical deadlines require more rigorous scheduling.

### Scheduling Approaches and Methods
- **Location-Based Scheduling (LBS):** Allocates quantities to locations considering production rates, crew sizing, logic, and splitting/buffering.
- **Lean scheduling:** Based on lean project delivery — pull-planning sessions, limiting queues, master scheduling → phase scheduling → look-ahead planning. Uses Last Planner System®.
- **Critical Path Method / Critical Chain Method / Gantt charts / Kanban** — selected based on industry and project context.
- **Schedule and deliverable alignment:** Ensure WBS elements have commensurate schedule detail levels.

## Interactions With Other Domains

The Schedule Performance Domain closely interacts with:

| Domain | Nature of Interaction |
|---|---|
| [[05_Scope_Performance_Domain|Scope]] | Scope, Schedule, and Finance form the "triple constraint" — changes in one impact the others. Scope defines the work to be scheduled. |
| [[07_Finance_Performance_Domain|Finance]] | Duration depends on cost/funding; schedule compression often increases costs. |
| [[09_Resources_Performance_Domain|Resources]] | Resource availability, skill levels, and calendars directly determine activity durations and scheduling feasibility. |
| [[04_Governance_Performance_Domain|Governance]] | All domains function under governance; change control procedures for the schedule baseline are governed here. |
| [[08_Stakeholders_Performance_Domain|Stakeholders]] | Stakeholder expectations shape the schedule; stakeholder involvement is critical during development. |
| [[10_Risk_Performance_Domain|Risk]] | Schedule reserves and buffers address known-unknowns; risk events can directly impact the schedule. |

The triple constraint relationship — Scope, Schedule, and Finance — means any change in one domain likely impacts the others. These three domains are directly linked to project outcomes, while Stakeholders, Resources, and Risk maintain balance in parallel.

## Check Results

| Outcome | Check |
|---|---|
| Scheduling approaches consistent with project deliverables | Align schedule with the development approach (predictive, adaptive, or hybrid). |
| Project life cycle phases connect delivery of business and stakeholder value | Represent project work from launch to close in phases with appropriate exit criteria. |
| Holistic approach to delivering project outcomes | Schedule demonstrates holistic planning with no gaps or misalignment. |
| Schedule documentation is complete | Documentation includes task dependencies, durations, resource allocations, and milestones per the selected development approach. |
| Scheduling tools and techniques are used | Verify team employed appropriate tools (e.g., PMIS) and techniques (e.g., CPM). |
| Stakeholders are involved in schedule development | Check participation levels from key stakeholders; low involvement risks an unrealistic schedule. |
| Sufficient buffers for known-unknowns | Verify adequate floats, contingency reserves, or alternate activity network strategies (e.g., secondary plans) are built into the schedule. |

## Related Chapters

- [[05_Scope_Performance_Domain]] — Work packages and decomposition that feed activity definition
- [[09_Resources_Performance_Domain]] — Resource estimation, calendars, and leveling
- [[07_Finance_Performance_Domain]] — Cost/schedule trade-offs, triple constraint
- [[04_Governance_Performance_Domain]] — Change control for schedule baseline
- [[08_Stakeholders_Performance_Domain]] — Stakeholder involvement in schedule development
- [[10_Risk_Performance_Domain]] — Schedule reserves, buffers, and risk-driven schedule impacts
- **PERT** — Program Evaluation and Review Technique for probabilistic duration estimating
- **Critical Path Method** — CPM for determining minimum project duration and float
- **Critical Chain Method** — Buffer-based scheduling accounting for resource constraints
- **WBS** — Work Breakdown Structure decomposition
