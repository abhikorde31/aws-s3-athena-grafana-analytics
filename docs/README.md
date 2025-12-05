AWS S3 → Glue Crawler → Athena → Grafana Analytics Project

This project demonstrates a complete analytics pipeline using AWS S3, AWS Glue Crawler, AWS Athena, and Grafana for visualization.

📌 Project Overview

This pipeline allows you to:

Upload CSV/JSON files into an S3 bucket

Use Glue Crawlers to automatically detect schema and create tables

Query datasets using Athena

Connect Grafana to Athena using a data source

Build visual dashboards inside Grafana


📁 Architecture Flow
S3 Data Lake → Glue Crawler → Glue Data Catalog → Athena → Grafana Dashboard

🚀 Features

Fully serverless (no EC2 required)

Schema auto-detection from CSV/JSON

Athena SQL queries for analytics

Grafana visualization dashboards

Folder-based project structure for GitHub

📂 Project Structure:
AWS-S3-Athena-Grafana-Analytics/
│
├── docs/
│   └── README.md  ← (this file)
│
├── s3/
│   ├── sample-data/
│   │   └── your-data.csv
│   
│
├── sql/
│   ├── create-table-example.sql
│   ├── athena-queries.sql
│   
│
└── grafana/
    ├── dashboard.json

🪣 Step 1 — Upload Data to S3

Create an S3 bucket

Create folder: analytics-data/

Upload CSV/JSON files there
Example file path:s3://s3-athena-analytics-abhishek31/row

🕸 Step 2 — Create AWS Glue Crawler

1. Go to AWS Glue → Crawlers
2. Create crawler:
    Source: S3 folder (analytics-data)
    Target: Glue Data Catalog
    IAM Role: AWSGlueServiceRole

3. Run crawler
4. It creates a table in:

Database: s3_log_db
Table: s3_athena_analytics_abhishek31, s3_birth_death_natural_increase, s3_death_by_sex_age.

📊 Step 3 — Query Data in Athena

Example query:SELECT *
FROM s3_athena_analytics_abhishek31.s3_athena_table
ORDER BY period ASC;


📡 Step 4 — Connect Athena to Grafana

1. Install Athena plugin in Grafana
2. Add new Data Source
  -AWS Region
  -Authentication (IAM Role or Access Key)
  -Query results bucket
3. Save & Test

📈 Step 5 — Build Grafana Dashboard
-Use Athena queries as panels
-Build visualizations like:
  -Line charts
  -Bar graphs
  -Time series
  -Tables

📦 Future Enhancements

Automate crawler using Lambda
Add CI/CD for Terraform deployment
Add multiple datasets inside the same Athena database
<img width="1400" height="580" alt="1_NMhW3kKAVsN-EaEF57RYww" src="https://github.com/user-attachments/assets/51c0c8bf-64d8-4ffd-bf7c-9d41f97106a9" />
<img width="1487" height="672" alt="image001-4" src="https://github.com/user-attachments/assets/a5ef992a-4637-425c-a7a5-c7ad6e27c69d" />
![AWS-Config-data-exploration-and-visualization-with-Amazon-Managed-Grafana](https://github.com/user-attachments/assets/f489e2cb-313a-4da4-bac7-f79f798906bb)



👨‍💻 Author
Abhishek Korde
DevOps | Cloud | Automation Engineer

