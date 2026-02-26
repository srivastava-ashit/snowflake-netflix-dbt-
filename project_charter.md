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

models/
staging/
silver/
marts/
dimensions/
snapshots/
macros/
tests/
analysis/


---

## 🗓 Phase Roadmap

### Week 1 — Phase 1: Setup
- Snowflake account setup
- Warehouse sizing
- Schema design
- Synthetic data generator
- 5M row load into Bronze
- dbt project bootstrap

### Week 2 — Phase 2: Core Modeling
- Staging models
- Silver incremental model
- Deduplication logic
- Watermark implementation

### Week 3 — Phase 3: SCD2 Snapshot
- dim_user snapshot
- Late update handling
- Backfill tests
- Historical query validation

### Week 4 — Phase 4: Scale Simulation
- Increase to 50M rows
- Performance tuning
- Clustering analysis
- Credit usage monitoring

### Week 5–6 — Phase 5: Production Hardening (Optional)
- CI/CD simulation
- Slim CI
- Alerting integration
- DAG optimization
- Interview scenario drills

---

## 🔁 Continuation Protocol

To resume work:
> "Continue Snowflake Netflix-Scale Project — currently at Phase X."

---

## 📌 Current Status
**Phase 0 — Planning complete**
