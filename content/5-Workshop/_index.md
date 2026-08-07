---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# SECURE S3 DATA LAKE ACCESS USING AWS VPC ENDPOINTS & PRIVATELINK

### Workshop Overview
In our team's **Vietnamese Corporate Financial Distress Analytics Platform**, **Amazon S3 Data Lake** serves as the central storage repository containing raw financial report datasets (`S3 Raw Bucket`) and normalized indicator feature matrices (`S3 Curated Bucket`). To ensure high-level security for sensitive corporate financial data and prevent unauthorized data exposure over the public internet, our team implemented private connectivity using **AWS VPC Endpoints & AWS PrivateLink**.

This architecture enables compute workloads (EC2, AWS Lambda, AWS Glue Jobs) inside VPCs or on-premises data centers to access Amazon S3 directly via AWS internal backbone networks without ever traversing the public Internet.

In this practical workshop, our team synthesizes and guides the end-to-end implementation and testing of two core VPC Endpoint types:
* **Gateway VPC Endpoint**: Routes traffic directly from VPC Subnets to Amazon S3 via Route Tables. This delivers maximum data transfer throughput for financial analytics while incurring zero additional service costs.
* **Interface VPC Endpoint (AWS PrivateLink)**: Deploys Elastic Network Interfaces (ENIs) with private IP addresses inside your VPC. This allows hybrid on-premises systems or partner networks to securely query the S3 Data Lake using private DNS resolution.

---

### Table of Contents

1. **[5.1. Workshop Overview & Architecture](5.1-workshop-overview/)**
   * Overview of VPC Endpoint concepts, comparison between Gateway vs. Interface Endpoints, and lab topology.
2. **[5.2. Prerequisites & Environment Setup](5.2-prerequiste/)**
   * Provisioning underlying VPC infrastructure, subnets, test EC2 instances, and S3 Data Lake buckets.
3. **[5.3. Accessing Amazon S3 from VPC](5.3-s3-vpc/)**
   * Creating a Gateway VPC Endpoint, updating route tables, and verifying private S3 access from private subnets.
4. **[5.4. Accessing Amazon S3 from On-Premises Networks](5.4-s3-onprem/)**
   * Deploying an S3 Interface Endpoint, simulating on-premises DNS resolution, and testing PrivateLink connectivity.
5. **[5.5. Granular Access Control with Endpoint Policies](5.5-policy/)**
   * Attaching custom Endpoint Policies to restrict access strictly to authorized project S3 buckets.
6. **[5.6. Resource Cleanup](5.6-cleanup/)**
   * Step-by-step instructions to delete provisioned AWS resources after lab completion to prevent unexpected charges.