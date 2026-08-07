---
title : "Introduction"
date : 2024-01-01 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

# INTRODUCTION TO AWS VPC ENDPOINTS & ARCHITECTURE

#### Overview of VPC Endpoints in Our Financial System
* **VPC Endpoints** are fully managed virtual devices provided by AWS infrastructure. They are horizontally scaled, redundant, and highly available VPC components that enable compute resources inside our team's system to communicate privately with AWS services such as **Amazon S3** without exposing traffic to the public Internet.
* **Compute workloads inside VPC** (EC2, Lambda, Glue ETL) securely access our Amazon S3 Data Lake using a **Gateway VPC Endpoint**.
* **On-premises workloads or Hybrid Cloud environments** connect privately to the S3 Data Lake using an **Interface VPC Endpoint (AWS PrivateLink)**.

#### Practical Lab Topology
In this workshop, our team establishes and operates a simulated environment featuring two primary VPCs:
* **"VPC Cloud"**: Represents our cloud infrastructure hosting the Amazon S3 Data Lake (`S3 Raw` & `S3 Curated Bucket`), Gateway Endpoints, and EC2 test instances running financial ratio calculation engines.
* **"VPC On-Prem"**: Simulates an enterprise corporate data center. An EC2 instance running strongSwan VPN software is deployed inside "VPC On-prem" and automatically configured to establish a **Site-to-Site VPN** tunnel with AWS Transit Gateway to connect privately into the VPC Cloud.

![VPC Endpoint Architecture Diagram](/images/5-Workshop/5.1-Workshop-overview/diagram1.png)