
---

## Detailed Architecture & Data Flow

```markdown
# Snowflake Netflix-Scale Analytics Engineering Project — Architecture

## Layered Architecture

### Bronze Layer (Raw)
- Raw playback events
- Immutable, append-only
- Partitioned by event_date + region
- No business logic

### Silver Layer (Cleaned)
- Deduplicated and cleaned
- Incremental merge using event_ts watermark
- Handles late-arriving data
- Partition-aware

### Gold Layer (Business)
- Fact tables: playback, daily aggregates
- Dimensions: dim_user (SCD2), dim_title, dim_device
- Experiment mart for A/B testing
- Optional ML feature tables

---

## Incremental Strategy
- Merge-based incremental models
- Watermark-based partition pruning
- Late-arrival window simulation (24–48 hrs)

---

## SCD2 Strategy
- Timestamp-based snapshots for dim_user
- Backfill support without full refresh
- Historical tracking of subscription and region changes

---

## Performance Optimizations
- Micro-partition pruning
- Clustering keys on high-cardinality columns
- Batch load of raw events using `COPY INTO`
- Incremental run to minimize warehouse usage

---

## Observability
- dbt tests (not_null, unique, accepted_values)
- run_results.json parsing for failure alerts
- Slim CI simulation
- Query profile monitoring

---

## Data Volume Simulation
| Phase | Rows | Purpose |
|-------|------|---------|
| Phase 1 | 5M | Functional validation |
| Phase 2 | 20–50M | Performance testing |
| Phase 3 | 100M+ | Scale simulation
