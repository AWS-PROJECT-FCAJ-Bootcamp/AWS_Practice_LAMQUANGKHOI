---
title: "Week 6 Worklog"
date: 2026-07-27
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

* Pitch the team's financial application project to mentors and collect feedback to refine the target AWS architecture.
* Evaluate and test core AWS services, including AWS Amplify, S3, Glue Crawler, Glue Data Catalog, Athena, Lambda Worker, and EventBridge.
* Deploy hands-on setups for AWS Amplify hosting, WAF security rules, least-privilege IAM policies, and EventBridge automation.
* Design dual data ingestion flows: automated daily report generation via EventBridge and on-demand querying via Amazon Athena.
* Apply the 6 AWS Well-Architected pillars to optimize system security, cost, and operational reliability.
* Reorganize team task allocation (FE, BE, CI/CD, Data Processing, Monitoring) and update the layered architecture diagram.
* Maintain progress on academic journal research while proactively addressing monthly AWS credit reset constraints.

### Tasks carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Pitched the team's project idea to mentors to validate core assumptions and receive architectural guidance.<br>- Researched ~15 relevant AWS services (Amplify, S3, Glue Crawler, Glue Data Catalog, Athena, Lambda Worker, etc.) and Tiny Fish fetch/search capabilities.<br>- Completed ~50% of the AWS Amplify setup and prepared the configuration environment. | 07/27/2026 | 07/27/2026 | |
| Tue | - Completed the AWS Amplify hands-on lab and received guidance from mentor Minh on integrating Langfuse for dataset testing.<br>- Refined the closed-loop data pipeline based on architectural insights from mentor Minh's workshop.<br>- Evaluated core services (Amplify, S3, Glue Crawler, Glue Data Catalog, Athena, Lambda Worker) for proper functional alignment and cost efficiency.<br>- Drafted least-privilege IAM access structures and updated role assignments across team members (Vỹ - FE, Phong - BE, Khôi - CI/CD, Dương - Data, Tuấn Anh - Monitoring). | 07/28/2026 | 07/28/2026 | |
| Wed | - Configured AWS Amplify hosting, added WAF rules, and drafted granular IAM policy definitions for each role.<br>- Consulted mentor Quinler on ML prediction training pipelines, cost optimization, and adherence to the 6 AWS Well-Architected pillars.<br>- Conducted cost estimation on AWS Pricing Calculator and prepared IAM configurations for system migration. | 07/29/2026 | 07/29/2026 | <https://calculator.aws/> |
| Thu | - Studied Amazon Athena and AWS EventBridge to design dual data ingestion pipelines: (1) automated daily updates via EventBridge feeding user dashboards, and (2) on-demand user query retrieval via Athena.<br>- Completed the AWS EventBridge hands-on lab. | 07/30/2026 | 07/30/2026 | <https://youtu.be/77zSXuFs4GA?si=Us_XR5-ogDYW77hj> |
| Fri | - Submitted the architecture draft to the SGU mentor group for review and restructured diagram layout into distinct functional layers.<br>- Advanced academic research: investigated FinRL technical indicator preprocessing, reviewed RDX paper methods, and progressed with a new XAI approach. | 07/31/2026 | 07/31/2026 | |
| Sat | - Tested primary data pipeline services on AWS and tasked Tuấn Anh + Dương with redrawing the layered architecture diagram for group review.<br>- Consolidated report content for the academic journal research.<br>- Recorded a critical constraint regarding the monthly reset of AWS credits to $0 on August 1st and planned new account setups to sustain lab activities. | 08/01/2026 | 08/01/2026 | |

### Week 6 Achievements:

* Successfully pitched the financial application project to mentors (Khiêm, Lực, Minh, Quinler) and gathered feedback to refine the target architecture.
* Deployed AWS Amplify hosting, configured WAF security filters, and integrated Langfuse for dataset monitoring.
* Mastered the functional roles and synergy of key AWS components: Amplify, S3, Glue Crawler, Glue Data Catalog, Athena, Lambda Worker, and EventBridge.
* Formulated a robust dual data flow architecture combining automated EventBridge scheduling with on-demand Athena querying.
* Established least-privilege IAM policy structures and evaluated system design against the 6 AWS Well-Architected Framework pillars.
* Re-aligned team member responsibilities (FE, BE, CI/CD, Data Processing, Monitoring) and initiated layered architecture diagram updates.
* Maintained steady progress on parallel academic journal research focusing on FinRL technical indicators and XAI methodology.
* Promptly identified the monthly AWS credit reset issue on August 1st and planned replacement accounts to ensure continuous testing.
