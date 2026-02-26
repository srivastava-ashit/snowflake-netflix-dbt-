# Snowflake Netflix-Scale Analytics Engineering Project — Project Charter

## 🏷 Project Name
**Snowflake Netflix-Scale Analytics Engineering Project**

## 🎯 Objective
Simulate a Netflix-scale streaming analytics system with Snowflake + dbt, capable of:
- Handling tens of millions of playback events
- Incremental merge optimization
- Late-arriving event handling
- SCD2 dimension management
- Cost-aware warehouse design
- Production observability
- CI/CD simulation

---

## 🏗 Architecture Overview

### Layer 1 — Bronze (Raw)
- Append-only playback events
- Immutable
- No business logic
- Partitioned by event_date + region

### Layer 2 — Silver (Cleaned)
- Deduplicated
- Incremental merge
- Watermark-based partition pruning
- Handles late-arriving events

### Layer 3 — Gold (Business)
- Fact playback table
- SCD2 dim_user
- Daily aggregates
- Experiment analysis mart
- ML feature table (optional)

---

## 📊 Data Volume Plan

| Phase | Rows | Purpose |
|-------|------|---------|
| Phase 1 | 5M | Functional validation |
| Phase 2 | 20–50M | Performance testing |
| Phase 3 | 100M (optional) | Scale simulation |

---

## 🧠 Core Engineering Features

- Merge-based incremental models
- Watermark-based partition pruning
- Late-arrival window simulation (24–48 hr)
- SCD2 snapshot (timestamp strategy)
- Backfill strategy without full refresh
- Query profile performance analysis
- Warehouse credit analysis
- run_results.json failure alert simulation
- Slim CI simulation

---

## 📂 dbt Project Structure
