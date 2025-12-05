📊 Grafana Setup (Using Athena Queries Manually)

This project visualizes the datasets stored in Amazon S3, crawled by AWS Glue, queried using AWS Athena, and finally visualized in Grafana using the Athena DataSource Plugin.

This folder contains documentation for connecting Grafana to Athena and the SQL queries used to build each dashboard panel.
(No JSON dashboards are used—instead, dashboards were created manually inside Grafana.)

✅ 1. Install Athena Data Source Plugin in Grafana

Open Grafana → Connections → Plugins

Search: AWS Athena

Install plugin: grafana-athena-datasource

Restart Grafana if required.

✅ 2. Configure Athena Connection in Grafana

Go to:

Connections → Add New → Athena

Use the following:
| Field                    | Value                                                |
| ------------------------ | ---------------------------------------------------- |
| AWS Region               | `ap-south-1` (if your data is there)                 |
| Access                   | Access Key / IAM Role                                |
| S3 Query Result Location | `s3://s3-athena-analytics-abhishek31/query-results/` |
| Workgroup                | `primary`                                            |
| Database                 | `s3_log_db`                                          |

Save & Test → should show Connection OK.

✅ 3. Tables Queried in Grafana

Grafana panels use data from Athena tables:
| Table Name                        | Description                    |
| --------------------------------- | ------------------------------ |
| `s3_birth_death_natural_increase` | Natural increase yearly stats  |
| `s3_death_by_sex_age`             | Deaths grouped by age & gender |
| `s3_electronic_card_transactions` | Economic/transaction dataset   |

🎨 4. Grafana Panels & Queries

Below are example SQL queries you used in Grafana to generate charts.
🔹 Panel 1 — Total by Age Group(bar format)

Dataset: s3_death_by_sex_age
SELECT
  age AS metric,
  SUM(try_cast(count AS BIGINT)) AS value
FROM s3_log_db.s3_death_by_sex_age
WHERE period = 2010
  AND try_cast(count AS BIGINT) IS NOT NULL
GROUP BY age
ORDER BY
  CASE
    -- Preserve human-friendly order if your age labels are strings
    WHEN age = 'Infant' THEN 0
    WHEN age LIKE '%1–4%' THEN 1
    WHEN age LIKE '%5–9%' THEN 2
    WHEN age LIKE '%10–14%' THEN 3
    -- ... add all your bins in order ...
    ELSE 999
  END;



Panel 2 — Time Series (Deaths by Age per Year)
SELECT
  try(date_parse(CAST(period AS VARCHAR), '%Y')) AS time,
  age AS metric,
  SUM(try_cast(count AS BIGINT)) AS value
FROM s3_log_db.s3_death_by_sex_age
WHERE period IS NOT NULL
  AND try(date_parse(CAST(period AS VARCHAR), '%Y')) IS NOT NULL
  AND try_cast(count AS BIGINT) IS NOT NULL
GROUP BY period, age
ORDER BY time ASC;

panel 3 - Annual death counts by gender(Time Series)
SELECT
  -- Interpret 4-digit year as Jan-01 timestamp
  TRY(DATE_PARSE(CAST(period AS VARCHAR), '%Y')) AS time,
  sex AS metric,
  SUM(TRY_CAST(count AS BIGINT)) AS value
FROM s3_log_db.s3_death_by_sex_age
WHERE
  period IS NOT NULL
  AND REGEXP_LIKE(CAST(period AS VARCHAR), '^[0-9]{4}$')                -- keep only YYYY
  AND TRY(DATE_PARSE(CAST(period AS VARCHAR), '%Y')) IS NOT NULL        -- safe parse
  AND TRY_CAST(count AS BIGINT) IS NOT NULL
GROUP BY period, sex
ORDER BY time ASC
LIMIT 890;

so on using mannulay you can create dashboard.
📂 Dashboard Storage

Since dashboard was created manually:
👉 No JSON export is used.
👉 You can optionally export later using:

Dashboard → Settings → JSON Model → Export

Save it as:
grafana/dashboard-export.json

<img width="1919" height="827" alt="image" src="https://github.com/user-attachments/assets/79ded2e4-9ecf-4ea8-a516-8874c291a475" />


