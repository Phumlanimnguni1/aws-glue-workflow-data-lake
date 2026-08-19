# MnguniCorp CloudTrail Data Lake Automation

Overview
This project simulates a real-world enterprise data platform implementation for  MnguniCorp, a cloud marketing organization. The objective is to design, build, and automate a complete serverless data lake pipeline consisting of:

- An **Ingestion Layer** for storing continuous AWS CloudTrail logs in Amazon S3.

- A **Cataloging Layer** utilizing AWS Lake Formation blueprints and the AWS Glue Data Catalog.

- An **Orchestration Layer** utilizing AWS Glue workflows and triggers to automate data discovery.

- A **Consumption Layer** designed to query transformed log data using Amazon Athena for operational analytics.

The dataset consists of AWS CloudTrail event logs tracking infrastructure and API activity. The implementation follows the architectural guidelines provided in the AWS Data Engineering skillbuilder.

---
## Architecture

<img width="1576" height="668" alt="dataLakeArchitecture" src="https://github.com/user-attachments/assets/7076bb84-b125-43b0-b5e3-d88857fc90e0" />


- Compute & ETL: AWS Glue (Crawlers and Workflows)

- Storage: Amazon S3 (Data Lake Logs Bucket)

- Orchestration: AWS Lake Formation Blueprints

- Analytics: Amazon Athena

---
## Data Lake Layers

**Storage Layer (Amazon S3)**

- CloudTrail Logs Bucket: Serves as the central repository for raw and transformed log files.

- Connected directly to Lake Formation as a registered and secure data lake location.

**Cataloging Layer (AWS Glue Data Catalog)**

- Creates the central cloudtraillogs-db database.

- AWS Glue Crawler automatically scans S3, infers schemas, and builds metadata tables.

**Orchestration Layer (AWS Glue Workflows)**

- Utilizes a Lake Formation blueprint to instantly provision the ingestion workflow.

- A custom pre_crawl_trigger automates the execution of the crawler when new data arrives.

**Consumption Layer (Amazon Athena)**

- Enables the operational analytics team to query the cataloged data using standard SQL.

- Accesses data directly from the S3 bucket through the Glue Data Catalog without moving the underlying files.

---
## Data Assets & Schemas

The data lake automatically discovers and categorizes the log data into specific tables within the catalog:

- Raw Data Table (_lab_cloudtrail)

- Contains the unformatted, pre-transformed CloudTrail log data.

- Transformed Data Table (lab_cloudtrail)

- Contains the optimized, transformed log data (Parquet format) ready for fast querying.

- Fields include event time, user identity, source IP, and error codes.

---
## Pipeline Execution Flow

- All data discovery and cataloging is automated. The execution flow follows a strict progression:

- Blueprint Deployment: AWS Lake Formation blueprint generates the foundational AWS Glue workflow.

- Trigger Activation: The custom pre_crawl_trigger starts the crawler upon fulfilling specified event conditions.

- Schema Inference: The Glue crawler processes new S3 data and updates the cloudtraillogs-db database.

- Analytics Querying: Operational analysts run SQL queries via Amazon Athena against the updated tables.

---
## Security & Roles

- AWS Lake Formation: Centralizes access control, registering the S3 bucket as a secure data lake location.

- Workflow Execution Roles: Provides AWS Glue the necessary permissions to crawl S3 and update the catalog securely.

- Analyst Access: Grants the operational analytics team secure, query-level access to the data via Amazon Athena.

---
## Business Outcomes & Analytical Outputs

- Automated Data Ingestion: Eliminates manual ETL overhead by dynamically cataloging new CloudTrail logs using Lake Formation Blueprints.

- Operational Monitoring: Empowers the organization to efficiently track cloud infrastructure events and user activity.

- Error Resolution & Auditing: Allows analysts to instantly filter and identify system failures using targeted SQL queries (e.g., isolating specific error codes).
