
## Data Flow

1. User activity generates events.
2. Events are streamed to Kafka topics.
3. Spark Structured Streaming consumes Kafka messages.
4. Raw events are stored in Bronze layer.
5. Cleaned events stored in Silver tables.
6. Aggregated analytics models created in Gold layer.
7. dbt manages transformations inside Snowflake.

## Medallion Architecture

### Bronze
Raw streaming ingestion.

Example tables

bronze.events_raw

### Silver
Cleaned and validated data.

Example tables

silver.events_clean

### Gold
Business-level datasets.

Example tables

gold.daily_user_engagement

gold.content_popularity

## Data Modeling

### Fact Table

fact_streaming_events

### Dimension Tables

dim_users (SCD Type 2)

dim_content

dim_date

## Engineering Challenges

### Late Arriving Events
Events may arrive out of order.

### Duplicate Events
Streaming systems sometimes deliver duplicate messages.

### Data Skew
Popular content can cause uneven data distribution.

### Idempotent Processing
Pipeline must safely reprocess data.
