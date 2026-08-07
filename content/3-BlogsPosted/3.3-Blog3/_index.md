---
title: "Blog 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# AUTOMATING FINANCIAL DATA NORMALIZATION AT SCALE WITH AWS GLUE JOBS

One of the most essential components in building our team's data architecture was setting up an automated data integration (ETL) pipeline. Raw financial reports collected from heterogeneous providers inherently exhibit indicator naming conflicts, missing fields, and large unparsed record volumes. Our team selected AWS Glue Jobs — a powerful serverless data integration service by AWS. Here are the core key takeaways regarding financial data normalization with AWS Glue Jobs synthesized by our team:

* **Automated Schema Discovery via AWS Glue Crawlers**: Our team deployed Glue Crawlers to scan raw JSON/CSV files landing in our `S3 Raw Bucket`, automatically inferring schema structures for the 3 core financial statements and cataloging metadata into the AWS Glue Data Catalog.
* **Distributed Data Transformation via PySpark ETL Jobs**: Our team engineered PySpark scripts on AWS Glue Jobs to standardize hundreds of multi-source indicator variations into a canonical schema (`total_assets`, `net_revenue`, `ebit`, `ocf`), while automatically filtering out specialized financial sectors (Banks, Securities, Insurance).
* **Completeness Filtering and Outlier Winsorization**: Our team enforced a strict 5-year continuous dataset threshold and integrated Winsorization algorithms (1%-99%) to cap statistical outliers without distorting underlying data distributions.
* **DPU Cost Optimization via Job Bookmarks**: Our team leveraged Job Bookmarks to maintain state tracking across execution runs, ensuring recurring ETL jobs process only incremental financial reports, preventing duplicate reprocessing and significantly lowering DPU expenses.
* **Snappy-Compressed Parquet Conversion and Partitioning**: Cleaned outputs were converted by our team into Snappy-compressed Apache Parquet files partitioned by `year` and `quarter` inside our `S3 Curated Bucket`, slashing S3 storage footprints by 80% while boosting Amazon Athena SQL query speeds.

Through this article, our team hopes readers gain a clear understanding of the fundamental principles behind operating AWS Glue Jobs to build an automated, cost-optimized, and scalable financial data normalization pipeline on the cloud.

![AWS Glue Jobs ETL Architecture and S3 Data Lake Flow Diagram](/images/Blog_3.jpg)

---

### Reference Sources:
* [AWS Glue Product Homepage](https://aws.amazon.com/glue/)
* [AWS Glue Technical Documentation](https://docs.aws.amazon.com/glue/)
* [AWS Glue ETL Best Practices Guide](https://docs.aws.amazon.com/prescriptive-guidance/latest/aws-glue-etl/welcome.html)