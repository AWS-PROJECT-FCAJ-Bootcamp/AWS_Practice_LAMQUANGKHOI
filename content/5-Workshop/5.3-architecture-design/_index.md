---
title: "Infrastructure Design & Architecture of AWS Data Pipeline"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# INFRASTRUCTURE DESIGN & ARCHITECTURE OF AWS FINANCIAL DATA PIPELINE

### Technical Architecture Overview
This section provides a step-by-step guide to building the technical infrastructure for the **Automated Vietnamese Securities Financial Data Ingestion and Analytics Platform on AWS Serverless**. The architecture is partitioned into decoupled service layers, ensuring automated execution, seamless horizontal scalability, and strict adherence to cloud security best practices.

---

### Section Navigation

* **[5.3.1. S3 Initialization and Configuration]({{< ref "5.3.1-s3-setup" >}})**: Provisioning S3 Raw Bucket for raw JSON payloads and S3 Curated Bucket for Snappy Parquet datasets.
* **[5.3.2. IAM Initialization and Configuration]({{< ref "5.3.2-iam-setup" >}})**: Setting up IAM Roles, Trust Relationships, and execution permissions for serverless services.
* **[5.3.3. Glue Initialization and Configuration]({{< ref "5.3.3-glue-setup" >}})**: Configuring AWS Glue PySpark ETL Jobs, computing technical indicators (MA20/RSI14), running Crawlers, and querying via Athena.
* **[5.3.4. Lambda Initialization and Configuration]({{< ref "5.3.4-lambda-setup" >}})**: Creating AWS Lambda Collector functions for market data crawling and Lambda Email functions for automated reporting.
* **[5.3.5. IAM Policy Initialization and Configuration]({{< ref "5.3.5-iam-policy" >}})**: Complete collection of sanitized JSON IAM Policies adhering to the Principle of Least Privilege.
