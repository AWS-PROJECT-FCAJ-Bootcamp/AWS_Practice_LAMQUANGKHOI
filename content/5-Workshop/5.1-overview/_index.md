---
title: "Project Overview"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# AUTOMATED SECURITIES FINANCIAL DATA LAKE PLATFORM ON AWS

### Executive Overview
The **Automated Vietnamese Securities Financial Data Ingestion and Analytics Platform** is a serverless, end-to-end cloud-native system designed to solve the challenges of fragmented data collection, manual aggregation, and lack of standardized long-term financial storage. Built on AWS Serverless infrastructure and automated via Infrastructure as Code (Terraform), the platform ingests raw financial statement and market data, normalizes schemas, calculates technical indicators (MA20, RSI14, return_pct), partitions multi-tiered Data Lake storage (Raw, Curated, Feature Store), and delivers REST APIs, Cognito authentication, a Web Dashboard, and automated email alerts.

> [!NOTE]
> This overview section is modeled directly after **PRD Version 2.0** (Product Requirements Document), providing alignment on problem statement, business flow, challenges, objectives, actor interactions, and multi-phase launch planning.

---

### Section Navigation

* **[5.1.1. Background, Objectives and Scope]({{< ref "5.1.1-context-target-scope" >}})**: Problem alignment, As-Is vs To-Be business flows, high-level architecture approach, and target customer profiles.
* **[5.1.2. Challenges and Difficulties]({{< ref "5.1.2-challenges" >}})**: Data provider fragmentation, rate limits, enterprise security/IAM management, and cost optimization.
* **[5.1.3. Objectives]({{< ref "5.1.3-objectives" >}})**: Key performance metrics, data quality benchmarks, and Functional & Non-Functional Requirements (FR/NFR).
* **[5.1.4. Actors and Use Cases]({{< ref "5.1.4-actors-use-cases" >}})**: Actor vs Stakeholder differentiation, Use Case matrix, and C4 Model Level 1 (System Context Diagram).
* **[5.1.5. Proposed Execution Plan (Starting Week 4)]({{< ref "5.1.5-execution-plan" >}})**: 5-phase project implementation roadmap, CI/CD pipeline, and weekly deliverables starting from Week 4.
