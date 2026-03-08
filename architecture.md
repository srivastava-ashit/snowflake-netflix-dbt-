# Data Platform Architecture

## Overview

This project simulates a large-scale streaming platform (similar to Netflix) and demonstrates how modern data engineering pipelines process high-volume user activity data.

The platform ingests user interaction events, processes them using distributed data processing frameworks, and produces analytics-ready datasets for business intelligence and downstream applications.

The architecture follows a **Lakehouse pattern** using Delta Lake combined with a **modern data warehouse approach using Snowflake and dbt**.

---

# High Level Architecture

The data pipeline consists of several stages beginning with data generation and ending with analytics-ready datasets.

Python scripts generate large volumes of streaming-like events. These events are processed using Apache Spark and stored in a lakehouse architecture. Curated datasets are then loaded into Snowflake where dbt manages analytical transformations.
Python Event Generator
│
▼
Raw Event Files (JSON / Parquet)
│
▼
Spark ETL Pipeline
│
▼
Delta Lake Storage
(Bronze → Silver Layers)
│
▼
Snowflake Data Warehouse
│
▼
dbt Transformations
│
▼
Analytics Layer


---

# Technology Stack

| Layer | Technology |
|------|------------|
| Data Generation | Python |
| Distributed Processing | Apache Spark (PySpark) |
| Storage | Delta Lake |
| Data Warehouse | Snowflake |
| Transformation | dbt |
| Orchestration | Apache Airflow |
| Version Control | Git + GitHub |

---

# Data Flow

## 1. Data Generation

A Python-based event generator simulates user activity on a streaming platform.

Example events include:

- play
- pause
- stop
- search
- recommendation_click

Each event includes metadata such as:

- user_id
- content_id
- event_time
- device_type
- location

These events are written to raw JSON or Parquet files.

---

# 2. Bronze Layer (Raw Data)

The Bronze layer stores raw ingested data exactly as received from the source systems.

Characteristics:

- Raw format
- Schema-on-read
- Minimal transformation
- Used for replay and auditing

Example table:

---
bronze.streaming_events

# 3. Silver Layer (Cleaned Data)

The Silver layer contains cleaned and standardized data after processing.

Transformations include:

- Schema enforcement
- Deduplication
- Timestamp normalization
- Data validation
- Handling late arriving records

Example table:
silver.cleaned_streaming_events

---

# 4. Data Warehouse Layer

Curated datasets from the Silver layer are loaded into Snowflake for analytical processing.

Snowflake provides:

- Elastic compute
- High concurrency
- SQL analytics
- Integration with dbt

Schemas used:
RAW
STAGING
ANALYTICS

---

# 5. Transformation Layer (dbt)

dbt manages SQL transformations inside Snowflake and implements the dimensional data model.

dbt provides:

- Version-controlled SQL models
- Data lineage
- Testing and documentation
- Incremental model support

Example dbt models:
stg_users
stg_content
stg_streaming_events

# Final analytical models:
dim_users
dim_content
fact_streaming_events
daily_user_engagement


---

# Data Modeling Strategy

The project follows a **Kimball dimensional modeling approach**.

## Fact Table
fact_streaming_events

Contains event-level data such as:

- user_key
- content_key
- event_time
- event_type
- device_type

---

## Dimension Tables
dim_users
dim_content
dim_date


### Slowly Changing Dimensions

User profiles may change over time. Therefore:
dim_users


---

# Engineering Challenges Simulated

The pipeline is designed to handle realistic production challenges.

## Late Arriving Data

Events may arrive after their original event timestamp.

Example:

Event time: 10:00  
Arrival time: 10:10  

The pipeline must correctly process these records.

---

## Duplicate Events

Streaming platforms may generate duplicate events.

Deduplication logic is implemented during the Silver layer transformation.

---

## Large Scale Data Processing

The project simulates millions of events to demonstrate:

- distributed processing
- partitioned data storage
- scalable ETL pipelines

---

# Orchestration

Apache Airflow will orchestrate the data pipeline.

Example DAG workflow:

generate_event_data
↓
spark_bronze_ingestion
↓
spark_silver_transformation
↓
load_to_snowflake
↓
run_dbt_models
↓
run_dbt_tests


Airflow ensures reliable scheduling and monitoring of the pipeline.

---

# Observability and Data Quality

The platform incorporates data quality checks through:

- dbt tests
- data validation rules
- pipeline monitoring

Future enhancements may include:

- alerting systems
- data observability tools
- pipeline dashboards

---

# Summary

This project demonstrates a modern data engineering architecture using:

- Spark for distributed data processing
- Delta Lake for lakehouse storage
- Snowflake as the analytical warehouse
- dbt for transformation and modeling
- Airflow for orchestration

The architecture mirrors production-grade data platforms used by large-scale technology companies.
