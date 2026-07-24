---
title: "Week 5 Worklog"
date: 2026-07-20
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

* Complete the AWS Organization portal, Organizational Unit, and IAM role setup for shared team access.
* Reassess the Kiro subscription approach and select a viable team workflow.
* Refine the project direction by distinguishing database, data warehouse, data lake, and data lakehouse architectures.
* Consolidate the PoC codebase, standardize the development environment, and expand automated testing.
* Review the frontend and backend, identify security risks, and complete the AWS architecture draft for mentor review.

### Tasks carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Set up the team AWS Organization portal with experimental Organizational Units and roles based on the current architecture.<br>- Completed access for the leader administrator and a read-only workload role.<br>- Revisited the Kiro configuration after forming the required group structure.<br>- Reviewed a standard data lake architecture and its common interfaces. | 07/20/2026 | 07/20/2026 | <https://www.projectpro.io/article/how-to-build-a-data-lake/1071> |
| Tue | - Completed the main shared-resource IAM setup for the team AWS Organization.<br>- Contacted Support regarding the Kiro subscription issue and confirmed that the account was not eligible to create an organization subscription.<br>- Discontinued Kiro as the planned shared tool and began considering a more suitable workflow.<br>- Compared database, data warehouse, and data lake characteristics, including OLTP, OLAP, ETL, schema, storage, and analytics use cases.<br>- Planned to redraw the AWS architecture using the updated data lake research and financial-domain requirements. | 07/21/2026 | 07/21/2026 | <https://youtu.be/-bSkREem8dM?si=R5y_AqhNa39WvKPK> |
| Wed | - Analyzed the AWS sample data lake framework as a reference for presenting the target architecture.<br>- Consolidated the existing frontend and backend code into a runnable but incomplete PoC.<br>- Standardized the development environment through `uv.lock`, `.gitignore`, and dependency requirements.<br>- Confirmed that the current PoC could handle the initial target of approximately 100 stocks, while the demo was about 50% complete.<br>- Recorded remaining frontend defects, missing backend tests, and the need to modularize the interface.<br>- Evaluated additional financial data providers to improve source diversity and reliability. | 07/22/2026 | 07/22/2026 | <https://github.com/aws-solutions-library-samples/data-lakes-on-aws><br><https://dichvu.vietstock.vn/du-lieu-tai-chinh/datafeed---du-lieu-tai-chinh-tich-hop-chuyen-nghiep><br><https://youtu.be/CxQJUbdoxt4?si=esU0_gkQo9Tw4RE3><br><https://www.dnse.com.vn/><br><https://www.ssi.com.vn/khach-hang-ca-nhan/fast-connect-api><br><https://fiingroup.vn/vi/giai-phap-du-lieu-chung-khoan-api-datafeed-microsite.html> |
| Thu | - Continued correcting the latest PoC and prepared a testing approach for the main data pipeline.<br>- Defined testing requirements by frontend and backend module, including functional flows, use cases, latency, and CI execution.<br>- Implemented initial tests for the main pipeline from the data source into the system.<br>- Recorded that frontend issues and parts of the backend still required broader test coverage.<br>- Prepared a revised task list for presenting the project to the mentors. | 07/23/2026 | 07/23/2026 | <https://github.com/aws-solutions-library-samples/data-lakes-on-aws> |
| Fri | - Reviewed the updated interface and code changes contributed by a team member.<br>- Reallocated work by role and recorded backend concerns, especially data-injection and search-injection risks.<br>- Completed approximately 80% of the AWS architecture diagram.<br>- Identified the need to expand API-based data sources and incoming data types.<br>- Refined the target direction toward a data lakehouse architecture and reorganized the AWS documentation headings for team collaboration.<br>- Set the architecture submission deadline for July 25. | 07/24/2026 | 07/24/2026 | <https://github.com/aws-solutions-library-samples/data-lakes-on-aws> |

### Week 5 Achievements:

* Completed the main AWS Organization portal, Organizational Unit, and IAM configuration for shared team access.
* Confirmed that the current account could not create a Kiro organization subscription and removed Kiro from the planned toolchain.
* Clarified the differences among database, data warehouse, and data lake models and used them to refine the project's architecture direction.
* Consolidated the PoC codebase and standardized the development environment with `uv.lock`, `.gitignore`, and dependency definitions.
* Reached a runnable PoC that supported the initial scale of approximately 100 stocks, while clearly recording its incomplete areas.
* Implemented initial tests for the main data pipeline and defined the need for broader frontend and backend coverage.
* Identified additional financial data providers and the trade-off between broader coverage, reliability, and service cost.
* Reviewed the user interface and backend, recorded injection-related risks, and redistributed work by role.
* Completed approximately 80% of the AWS architecture diagram and refined the target toward a data lakehouse model.
