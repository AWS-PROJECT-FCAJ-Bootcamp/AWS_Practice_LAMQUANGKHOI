---
title: "Worklog Week 7"
date: 2026-08-03
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

# WEEK 7 WORKLOG: AUTOMATED PIPELINE COMPLETION, AWS STEP FUNCTIONS, AMPLIFY HOSTING, AND FINAL REPORT SYNTHESIS

### Objectives for Week 7:
* Hold team meetings to divide micro-tasks, tighten deployment deadlines, and closely monitor live deployment costs via AWS Cost Explorer.
* Grant fine-grained IAM permissions to team members and learn key insights from Swimburne Team's presentation regarding user targeting, security, and platform cost efficiency.
* Configure AWS Systems Manager (SSM) Parameter Store, standardize raw S3 data fields focusing on Market Price data, and clean data prior to Glue Job execution.
* Design and orchestrate the data pipeline using **AWS Step Functions (State Machine)** combined with AWS Lambda, AWS Glue Jobs, and Glue Crawlers.
* Automate data ingestion triggers via **AWS EventBridge** (daily scheduled execution post-market close or on-demand).
* Coordinate the deployment of the Web Dashboard to **AWS Amplify Hosting** with a custom domain: `https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login/`.
* Complete all individual documentation (Blog Reports, Worklog, Events Participated, Self-Evaluation, and FCAJ Feedback).

---

### Tasks Executed This Week:

| Day | Task Description | Start Date | Completion Date | Reference Materials |
| :---: | :--- | :---: | :---: | :---: |
| Mon | - Held team coordination meeting to assign micro-tasks and accelerate deployment deadlines to AWS.<br>- Accepted live deployment costs and actively tracked/analyzed expenses via **AWS Cost Explorer / Cost Analyze**. | 08/03/2026 | 08/03/2026 | <https://aws.amazon.com/aws-cost-management/aws-cost-explorer/> |
| Tue | - Configured IAM user accounts with appropriate policies for all team members.<br>- Investigated SSI FastConnectAPI restrictions (access granted exclusively via physical SSI headquarters connection).<br>- Attended **Swimburne Team**'s project showcase: learned that identifying target users early streamlines architecture design and scaling, while balancing platform costs and security (Web offers faster deployment, Mobile requires heavy UI/UX focus).<br>- Assessed team MVP progress and scalability potential. | 08/04/2026 | 08/04/2026 | |
| Wed | - Successfully executed data ingestion and tested data hosting on AWS prior to pushing GitHub CI/CD pipeline.<br>- Configured **AWS SSM Parameter Store** to securely manage environment parameters.<br>- Standardized S3 Raw data schemas: prioritized Market Price data while streamlining financial report fields to meet strict deadlines. | 08/05/2026 | 08/05/2026 | <https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html> |
| Thu | - Accelerated the deployment of AWS Pipeline Services.<br>- Implemented and managed data orchestration via **AWS Step Functions**: `Start` ➔ `Lambda: Ingest_Financial_Data` ➔ `Glue: StartJobRun` ➔ `Glue: StartCrawler` ➔ `Wait for Crawler` ➔ `Check Crawler Status` ➔ `Choice: Is Crawler Ready?` ➔ `Succeeded / Failed`.<br>- Formatted ingested financial data into standardized S3 Lake paths and prepared Frontend/Backend AWS deployment steps. | 08/06/2026 | 08/06/2026 | <https://aws.amazon.com/step-functions/> |
| Fri | - Fully automated the ingestion pipeline using **AWS EventBridge** (scheduled daily execution post-market close or on-demand).<br>- Coordinated web application hosting on **AWS Amplify** with a custom domain (`https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login/`).<br>- Synthesized all personal documentation (Workshop overview, Events, Self-Evaluation, FCAJ Feedback) and submitted the final deliverables. | 08/07/2026 | 08/07/2026 | `https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login/` |

---

### Achievements for Week 7:

* **End-to-End Automated Data Pipeline**: Built a seamless financial data ingestion and ETL pipeline using AWS Step Functions, Lambda, Glue, and EventBridge.
* **Production Web Dashboard Deployment**: Successfully hosted the web dashboard on AWS Amplify with an active custom domain.
* **Secure Environment & Governance**: Implemented proper IAM least-privilege permissions and secured sensitive parameters using AWS SSM Parameter Store.
* **Architecture & Cost Optimization**: Applied insights from Swimburne Team's presentation, analyzed real costs using AWS Cost Explorer, and optimized data scope.
* **Comprehensive Documentation**: Fully completed 7 weeks of worklogs, workshop reports, event recaps, self-evaluation, and constructive feedback for FCAJ.
