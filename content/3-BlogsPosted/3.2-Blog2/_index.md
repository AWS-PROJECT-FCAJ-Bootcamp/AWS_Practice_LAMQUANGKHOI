---
title: "Blog 2"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# AUTOMATING ML LIFECYCLE WITH AMAZON SAGEMAKER AI IN FINANCIAL RISK PREDICTION

Following the deployment of our financial Data Lake, the next milestone in our research journey was exploring artificial intelligence solutions for early corporate financial distress prediction. Our team selected Amazon SageMaker AI — a fully managed AWS machine learning platform designed to automate the entire end-to-end ML lifecycle. Here are the core key takeaways our team synthesized regarding the implementation of SageMaker AI:

* **Centralized Feature Governance via SageMaker Feature Store**: Our team registered normalized financial ratio metrics (CR, ROA, ROE, DAR, WCTA, ...) and target distress labels from our S3 Data Lake into SageMaker Feature Store, enabling consistent feature reusability across modeling iterations.
* **Model Experimentation in SageMaker Studio**: Utilizing SageMaker Studio and Autopilot, our team evaluated popular classification algorithms including XGBoost, Random Forest, Logistic Regression, and LightGBM to identify optimal performance on Vietnamese financial datasets.
* **Time-Series Training and Recall Optimization**: Our team adopted a Time-Series Split strategy (2018–2022 Train, 2023–2025 Test) to prevent future data leakage. In financial distress prediction, our team prioritized optimizing the Recall metric for the distressed class (`distress = 1`) to eliminate catastrophic false negatives.
* **Cost-Optimized Serverless Endpoint Deployment**: Our team packaged approved models into SageMaker Serverless Endpoints, allowing prediction APIs to scale automatically during Dashboard request bursts and scale down to zero when idle, cutting inference infrastructure expenses by up to 70%.
* **Standardized MLOps Governance with Model Registry and Monitor**: Our team versioned approved models in SageMaker Model Registry and configured SageMaker Model Monitor to detect Data Drift upon new quarterly reporting releases, triggering automated retraining alerts.

Through this article, our team hopes to highlight the essential principles of leveraging Amazon SageMaker AI to construct an automated, standardized, and cost-optimized MLOps workflow for corporate financial risk analytics.

![Amazon SageMaker AI Training and Deployment Workflow Diagram](/images/Blog%202.drawio.png)

---

### Resources & Publication Proof Links:
* **Official Proof Post on AWS Study Group**: [Blog 2 Facebook Post Link](https://www.facebook.com/groups/660548818043427/?multi_permalinks=2239716780126615&hoisted_section_header_type=recently_seen&__cft__[0]=AZbmBB6YUbgWFZSvx8t5TOmBZJ1nMh71nPnTdPGW0IkydgXvVwfKeccapRPtiQDMOr9TW0B49RZWy6YwedBQwGAS6vuZ164Y2AUClXbuGF-gH184ym6-NTJh2sKKQ5-PUBzLFvcXsR3tOAr1IfdxM9Za&__tn__=%2CO%2CP-R)
* [Amazon SageMaker AI Official Homepage](https://aws.amazon.com/sagemaker/)
* [AWS SageMaker Technical Documentation](https://docs.aws.amazon.com/sagemaker/)
* [AWS Machine Learning Official Blog](https://aws.amazon.com/blogs/machine-learning/)