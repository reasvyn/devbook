# Data Engineer

## Description

What a data engineer should know — building and maintaining data pipelines, data warehousing, ETL/ELT, data quality, and enabling analytics and ML at scale.

## Prerequisites

- [Mid Backend Developer](../mid/backend.md) or equivalent — SQL, programming, distributed systems basics

## Learning Path

### Data Pipelines

- `[must-know]` ETL vs ELT — when to transform before or after loading
- `[must-know]` Orchestration — Airflow, Dagster, Prefect
- `[must-know]` Batch processing — Spark, dbt, SQL transformations
- `[good-to-know]` Streaming — Kafka, Flink, Spark Streaming
- `[good-to-know]` Change Data Capture (CDC) — Debezium, AWS DMS
- `[nice-to-have]` Real-time pipelines — low-latency, exactly-once semantics

### Data Storage

- `[must-know]` Data warehouses — BigQuery, Snowflake, Redshift
- `[must-know]` Data lakes — S3/ADLS/GCS, Delta Lake, Iceberg, Hudi
- `[must-know]` Lakehouse architecture — combining lake and warehouse
- `[good-to-know]` Columnar storage — Parquet, ORC, compression
- `[good-to-know]` Partitioning and clustering strategies
- `[nice-to-have]` Data mesh — domain-oriented data ownership

### SQL & Data Modeling

- `[must-know]` Advanced SQL — window functions, CTEs, complex joins, query optimization
- `[must-know]` Dimensional modeling — fact tables, dimension tables, star schema
- `[must-know]` Slowly changing dimensions — SCD types 1, 2, 3
- `[good-to-know]` Data vault modeling
- `[good-to-know]` Query performance — execution plans, partitioning, clustering

### Data Quality & Governance

- `[must-know]` Data quality checks — freshness, volume, schema, NULL checks
- `[must-know]` Data catalog — data discovery, lineage, documentation
- `[must-know]` Data contracts — agreeing on schema and semantics between producers and consumers
- `[good-to-know]` Data masking and anonymization — GDPR, CCPA compliance
- `[good-to-know]` Data retention and lifecycle management

### Infrastructure & Operations

- `[must-know]` Cloud data infrastructure — AWS/GCP/Azure data services
- `[must-know]` Infrastructure as Code for data — Terraform, Pulumi
- `[must-know]` Monitoring data pipelines — latency, error rates, data freshness
- `[good-to-know]` Containerization — Docker, Kubernetes for data workloads
- `[good-to-know]` Cost optimization — query cost, storage tiering, compute auto-scaling

### Supporting ML

- `[good-to-know]` Feature engineering pipelines — transforming raw data into features
- `[good-to-know]` Feature stores — Feast, Tecton
- `[good-to-know]` Training data pipelines — versioning, serving training data
- `[nice-to-have]` Model serving infrastructure — batch inference, real-time endpoints

### Collaboration

- `[must-know]` Working with data scientists — understanding their needs, providing clean data
- `[must-know]` Working with analytics engineers — defining metrics, building marts
- `[good-to-know]` Documentation — data dictionaries, pipeline READMEs, runbooks
- `[good-to-know]` On-call for data pipelines — incident response for data issues

## Next Steps

- [Senior Data Scientist](../senior/data-scientist.md) — if shifting toward modeling and analysis
- [SRE](sre.md) — if shifting toward reliability and platform engineering
