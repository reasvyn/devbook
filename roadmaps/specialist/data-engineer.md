# Data Engineer

## Description

What a data engineer should know — building and maintaining data pipelines, data warehousing, ETL/ELT, data quality, and enabling analytics and ML at scale.

## Prerequisites

- [Mid Backend Developer](../mid/backend-developer.md) or equivalent — SQL, programming, distributed systems basics

## Learning Path

### Data Pipelines

- `🔴 CRITICAL` ETL vs ELT — when to transform before or after loading
- `🔴 CRITICAL` Orchestration — Airflow, Dagster, Prefect
- `🔴 CRITICAL` Batch processing — Spark, dbt, SQL transformations
- `🟠 HIGH` Streaming — Kafka, Flink, Spark Streaming
- `🟠 HIGH` Change Data Capture (CDC) — Debezium, AWS DMS
- `🟡 MEDIUM` Real-time pipelines — low-latency, exactly-once semantics

### Data Storage

- `🔴 CRITICAL` Data warehouses — BigQuery, Snowflake, Redshift
- `🔴 CRITICAL` Data lakes — S3/ADLS/GCS, Delta Lake, Iceberg, Hudi
- `🔴 CRITICAL` Lakehouse architecture — combining lake and warehouse
- `🟠 HIGH` Columnar storage — Parquet, ORC, compression
- `🟠 HIGH` Partitioning and clustering strategies
- `🟡 MEDIUM` Data mesh — domain-oriented data ownership

### SQL & Data Modeling

- `🔴 CRITICAL` Advanced SQL — window functions, CTEs, complex joins, query optimization
- `🔴 CRITICAL` Dimensional modeling — fact tables, dimension tables, star schema
- `🔴 CRITICAL` Slowly changing dimensions — SCD types 1, 2, 3
- `🟠 HIGH` Data vault modeling
- `🟠 HIGH` Query performance — execution plans, partitioning, clustering

### Data Quality & Governance

- `🔴 CRITICAL` Data quality checks — freshness, volume, schema, NULL checks
- `🔴 CRITICAL` Data catalog — data discovery, lineage, documentation
- `🔴 CRITICAL` Data contracts — agreeing on schema and semantics between producers and consumers
- `🟠 HIGH` Data masking and anonymization — GDPR, CCPA compliance
- `🟠 HIGH` Data retention and lifecycle management

### Infrastructure & Operations

- `🔴 CRITICAL` Cloud data infrastructure — AWS/GCP/Azure data services
- `🔴 CRITICAL` Infrastructure as Code for data — Terraform, Pulumi
- `🔴 CRITICAL` Monitoring data pipelines — latency, error rates, data freshness
- `🟠 HIGH` Containerization — Docker, Kubernetes for data workloads
- `🟠 HIGH` Cost optimization — query cost, storage tiering, compute auto-scaling

### Supporting ML

- `🟠 HIGH` Feature engineering pipelines — transforming raw data into features
- `🟠 HIGH` Feature stores — Feast, Tecton
- `🟠 HIGH` Training data pipelines — versioning, serving training data
- `🟢 LOW` Model serving infrastructure — batch inference, real-time endpoints

### Collaboration

- `🔴 CRITICAL` Working with data scientists — understanding their needs, providing clean data
- `🔴 CRITICAL` Working with analytics engineers — defining metrics, building marts
- `🟠 HIGH` Documentation — data dictionaries, pipeline READMEs, runbooks
- `🟠 HIGH` On-call for data pipelines — incident response for data issues

## Next Steps

- [Senior Data Scientist](../senior/data-scientist.md) — if shifting toward modeling and analysis
- [SRE](site-reliability-engineer.md) — if shifting toward reliability and platform engineering
