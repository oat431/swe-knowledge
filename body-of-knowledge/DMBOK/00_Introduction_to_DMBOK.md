---
title: "Introduction to Data Management"
tags:
  - introduction
  - data-management
  - dmbok
source: "DMBoK v2 by DAMA"
chapter: 1
created: 2026-07-09
---

# Introduction to Data Management

## Data Management Definition

**Data Management** is the development, execution, and supervision of plans, policies, programs, and practices that deliver, control, protect, and enhance the value of data and information assets throughout their lifecycles.

A **Data Management Professional** is any person who works in any facet of data management — from technical management of data throughout its lifecycle to ensuring that data is properly utilized and leveraged — to meet strategic organizational goals. Roles range from highly technical (database administrators, network administrators, programmers) to strategic business (Data Stewards, Data Strategists, Chief Data Officers).

Data management activities are wide-ranging and require both technical and non-technical ("business") skills. Responsibility for managing data must be shared between business and IT roles, and people in both areas must collaborate to ensure high quality data that meets strategic needs.

## Business Drivers

Information and knowledge hold the key to competitive advantage. Organizations with reliable, high quality data about their customers, products, services, and operations can make better decisions than those without data or with unreliable data. The primary driver for data management is to enable organizations to get value from their data assets, just as effective management of financial and physical assets enables organizations to get value from those assets.

## Goals of Data Management

Within an organization, data management goals include:

1. Understanding and supporting the information needs of the enterprise and its stakeholders — including customers, employees, and business partners
2. Capturing, storing, protecting, and ensuring the integrity of data assets
3. Ensuring the quality of data and information
4. Ensuring the privacy and confidentiality of stakeholder data
5. Preventing unauthorized or inappropriate access, manipulation, or use of data and information
6. Ensuring data can be used effectively to add value to the enterprise

## Essential Concepts

### Data

Data represents facts about the world. In the context of information technology, data is information stored in digital form — though data management principles also apply to data on paper and in databases. Data is both an interpretation of the objects it represents and an object that must itself be interpreted. Context — provided by **Metadata** — is required for data to be meaningful.

Organizations often have multiple ways of representing the same concept, which is why **Data Architecture**, **Data Modeling and Design**, **Data Governance**, and **Data Quality** management are needed.

### Data and Information

Data has been called the "raw material of information" and information has been called "data in context." While the traditional DIKW pyramid (Data → Information → Knowledge → Wisdom) is conceptually useful, it has limitations:

- Data doesn't simply exist — it must be **created**, and it takes knowledge to do so
- Data and information are intertwined and dependent on each other; data is a form of information and vice versa
- Both data and information need to be managed together with uses and customer requirements in mind

### Data as an Organizational Asset

An **asset** is an economic resource that can be owned or controlled and that holds or produces value. Data is widely recognized as an enterprise asset. Organizations rely on data assets to make more effective decisions, create new products and services, improve operational efficiency, cut costs, and control risks.

Being **data-driven** means recognizing that data must be managed efficiently and with professional discipline, through a partnership of business leadership and technical expertise.

## Data Management Principles

Data management shares characteristics with other forms of asset management. It involves knowing what data an organization has, what might be accomplished with it, and how best to use data assets to reach organizational goals. The following principles guide data management practice:

### 1. Data is an asset with unique properties
Data differs from other assets in important ways: it is not consumed when used, it is easy to copy and transport, it can be used by multiple people simultaneously, and it is durable (does not wear out). These properties influence how it must be managed.

### 2. The value of data can and should be expressed in economic terms
Calling data an asset implies it has value. Organizations should develop consistent ways to quantify that value, and measure both the costs of low quality data and the benefits of high quality data.

### 3. Managing data means managing the quality of data
Ensuring data is fit for purpose is a primary goal of data management. Organizations must understand stakeholders' quality requirements and measure data against them. See **Data Quality**.

### 4. It takes Metadata to manage data
The data used to manage and understand data is called **Metadata**. Because data cannot be held or touched, understanding what it is and how to use it requires definitions and knowledge in the form of Metadata.

### 5. It takes planning to manage data
Data is created in many places and moved between places for use. Coordinating work and keeping end results aligned requires planning from an architectural and process perspective. See **Data Architecture**.

### 6. Data management is cross-functional; it requires a range of skills and expertise
A single team cannot manage all of an organization's data. Data management requires both technical and non-technical skills and the ability to collaborate across domains.

### 7. Data management requires an enterprise perspective
Data management has local applications but must be applied across the enterprise to be as effective as possible. This is why data management and **Data Governance** are intertwined.

### 8. Data management must account for a range of perspectives
Data is fluid. Data management must constantly evolve to keep up with the ways data is created, used, and the data consumers who use it.

### 9. Data management is lifecycle management
Data has a lifecycle — from creation/obtainment through storage, maintenance, use, enhancement, and disposal. Managing data requires managing its lifecycle. Key lifecycle activities include:
- **Plan & Design**: Architecture, modeling, design
- **Enable & Maintain**: Storage, operations, integration, security
- **Use & Enhance**: Warehousing, business intelligence, analytics, data science

### 10. Different types of data have different lifecycle characteristics
Different types of data — transactional, **reference data**, **master data**, **Metadata** — have different management requirements. Data management practices must be flexible enough to accommodate these differences.

### 11. Managing data includes managing the risks associated with data
Data represents risk as well as value. Data can be lost, stolen, or misused. Organizations must consider the ethical implications of data use and manage data-related risks throughout the lifecycle. See **Data Security** and **Data Handling Ethics**.

### 12. Data management requirements must drive Information Technology decisions
Data and data management are deeply intertwined with IT, but managing technology is not the same as managing data. Technology should serve — not drive — an organization's strategic data needs.

### 13. Effective data management requires leadership commitment
Data management involves complex processes requiring coordination, collaboration, and commitment. This requires not only management skills but also the vision and purpose that come from committed leadership — often embodied in the role of **Chief Data Officer**.

## Data Management Challenges

The unique properties of data present distinct challenges:

| Challenge | Description |
|-----------|-------------|
| **Data differs from other assets** | Data is intangible, non-consumable, easily copied, and hard to value monetarily |
| **Data valuation** | The value of data is contextual and temporal; no standardized valuation methods exist |
| **Data quality** | Poor quality data costs organizations 10–30% of revenue; quality must be built into processes |
| **Planning** | Deriving value requires systems thinking across business processes, technology, architecture, and strategy |
| **Metadata management** | Metadata itself is data that must be managed; organizations that don't manage data well typically don't manage metadata at all |
| **Cross-functional nature** | Data management requires design, technical, analytical, linguistic, and strategic skills |
| **Enterprise perspective** | Data is a "horizontal" that moves across verticals (sales, marketing, operations); siloed data creates integration challenges |
| **Multiple perspectives** | Data comes from internal and external sources with different legal, compliance, and representational requirements |
| **Data lifecycle complexity** | Lifecycle and lineage intersect; creation and usage are the most critical lifecycle points |
| **Different data types** | Transactional, master, reference, and metadata each have distinct management requirements |
| **Data and risk** | Low quality, misunderstood, or misused data creates risk; regulatory environment increasingly expects data on the risk register |
| **Technology vs. data** | Tension exists between building new technology and having reliable data; data requirements should drive technology decisions |
| **Leadership** | Few organizations are truly data-driven; committed leadership and cultural change are essential |

## Data Management Strategy

A **data strategy** should include business plans to use information for competitive advantage and to support enterprise goals. It must come from an understanding of the data needs inherent in the business strategy: what data the organization needs, how it will get it, how it will manage it, and how it will utilize it.

Components of a data management strategy include:

- A compelling vision for data management
- A summary business case with selected examples
- Guiding principles, values, and management perspectives
- Mission and long-term directional goals
- Proposed measures of success
- Short-term (12–24 months) SMART objectives
- Descriptions of data management roles, organizations, responsibilities, and decision rights
- A prioritized program of work with scope boundaries
- A draft implementation roadmap with projects and action items

Key deliverables:
- **Data Management Charter**: Vision, business case, goals, guiding principles, measures of success, critical success factors, recognized risks, operating model
- **Data Management Scope Statement**: Goals and objectives for a planning horizon (typically 3 years), roles, organizations, and leaders accountable
- **Data Management Implementation Roadmap**: Specific programs, projects, task assignments, and delivery milestones

## The DAMA-DMBOK Framework

The DAMA-DMBOK Framework provides a comprehensive view of data management through three primary visuals:

### The DAMA Wheel
The **Data Governance** Knowledge Area sits at the center of data management activities, providing consistency and balance between all other functions. Surrounding it are ten additional Knowledge Areas, all necessary parts of a mature data management function.

### The Environmental Factors Hexagon
Shows the relationship between **people**, **process**, and **technology**, with goals and principles at the center providing guidance for how people should execute activities and effectively use tools.

### The Knowledge Area Context Diagram
Each Knowledge Area is described using a SIPOC-style diagram (Suppliers, Inputs, Processes, Outputs, Consumers) with activities classified into four phases: **Plan (P)**, **Develop (D)**, **Operate (O)**, and **Control (C)**.

## The Eleven Knowledge Areas

The DMBOK is structured around eleven interconnected Knowledge Areas. Each is covered in its own chapter (Chapters 3–13):

### 1. **Data Governance** (Chapter 3)
Provides direction and oversight for data management by establishing a system of decision rights over data that accounts for the needs of the enterprise. Sits at the center of the DAMA Wheel.

### 2. **Data Architecture** (Chapter 4)
Defines the blueprint for managing data assets by aligning with organizational strategy to establish strategic data requirements and designs to meet these requirements.

### 3. **Data Modeling and Design** (Chapter 5)
The process of discovering, analyzing, representing, and communicating data requirements in a precise form called the data model.

### 4. **Data Storage and Operations** (Chapter 6)
Includes the design, implementation, and support of stored data to maximize its value. Operations provide support throughout the data lifecycle from planning to disposal.

### 5. **Data Security** (Chapter 7)
Ensures that data privacy and confidentiality are maintained, that data is not breached, and that data is accessed appropriately.

### 6. **Data Integration and Interoperability** (Chapter 8)
Includes processes related to the movement and consolidation of data within and between data stores, applications, and organizations.

### 7. **Document and Content Management** (Chapter 9)
Includes planning, implementation, and control activities used to manage the lifecycle of data and information found in a range of unstructured media, especially documents needed to support legal and regulatory compliance requirements.

### 8. **Reference and Master Data** (Chapter 10)
Includes ongoing reconciliation and maintenance of core critical shared data to enable consistent use across systems of the most accurate, timely, and relevant version of truth about essential business entities.

### 9. **Data Warehousing and Business Intelligence** (Chapter 11)
Includes the planning, implementation, and control processes to manage decision support data and to enable knowledge workers to get value from data via analysis and reporting.

### 10. **Metadata** (Chapter 12)
Includes planning, implementation, and control activities to enable access to high quality, integrated Metadata — including definitions, models, data flows, and other information critical to understanding data and the systems through which it is created, maintained, and accessed.

### 11. **Data Quality** (Chapter 13)
Includes the planning and implementation of quality management techniques to measure, assess, and improve the fitness of data for use within an organization.

## Supporting Chapters

Beyond the eleven Knowledge Areas, the DMBOK also covers:

- ****Data Handling Ethics**** (Chapter 2) — The central role that data ethics plays in making informed, socially responsible decisions about data and its uses.
- ****Big Data and Data Science**** (Chapter 14) — Technologies and business processes emerging as our ability to collect and analyze large, diverse data sets increases.
- ****Data Management Maturity Assessment**** (Chapter 15) — Approaches to evaluating and improving an organization's data management capabilities.
- ****Data Management Organization and Role Expectations**** (Chapter 16) — Best practices for organizing data management teams and enabling successful practices.
- ****Data Management and Organizational Change Management**** (Chapter 17) — How to plan for and successfully move through cultural changes necessary to embed effective data management practices.

## DMBOK Evolution: Alternative Frameworks

### The DMBOK Pyramid (Aiken)
Describes how organizations evolve through four phases toward mature data management:
1. **Phase 1**: Purchase applications with database capabilities (starting point for **modeling**, **storage**, **Data Security**)
2. **Phase 2**: Address data quality challenges; build reliable **Metadata** and consistent **Data Architecture**
3. **Phase 3**: Implement **Data Governance** as structural support; execute strategic initiatives (**content management**, **master data**, **BI**)
4. **Phase 4**: Leverage well-managed data for advanced analytics

### Functional Area Dependencies (Geuens)
Recognizes that **Business Intelligence and Analytics** depend on all other functions, which in turn rest on a foundation of **Data Governance** (including **Metadata**, **Data Security**, **Data Architecture**, and **Reference Data**).

### DAMA Data Management Function Framework
Organizes data management into three layers:
- **Oversight**: **Data Governance** — strategy, principles, policy, stewardship, culture change
- **Lifecycle Management**: Plan & Design → Enable & Maintain → Use & Enhance
- **Foundational Activities**: Data Risk Management (**Security**, Privacy, Compliance), **Metadata** Management, **Data Quality** Management

## DAMA and the DMBOK

DAMA International (The Data Management Association) produces the DMBOK to:
- Provide a functional framework for implementing enterprise data management practices
- Establish a common vocabulary for data management concepts
- Serve as the fundamental reference guide for the CDMP (Certified Data Management Professional) and other certification exams

## Works Cited

- Aiken, Peter and Billings, Juanita. *Monetizing Data Management*. Technics Publishing, 2014.
- English, Larry. *Improving Data Warehouse and Business Information Quality*. John Wiley and Sons, 1999.
- Redman, Thomas. *Data Driven: Profiting from Your Most Important Business Asset*. Harvard Business Review Press, 2008.
- Redman, Thomas. *Data Quality for the Information Age*. 1996.
- Sebastian-Coleman, Laura. *Measuring Data Quality for Ongoing Improvement*. Morgan Kaufmann, 2013.
- Henderson, J.C. and Venkatraman, H. "Leveraging Information Technology for Transforming Organizations." *IBM System Journal*, 1999.
- Abcouwer, A.W., Maes, R., Truijens, J. "Contouren van een generiek Model voor Informatiemanagement." Primavera Working Paper, 1997.
- *The Leader's Data Manifesto*, 2017.
