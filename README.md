# ETLWatchtower

> **Temporary project README — architecture and scope notes**
>
> This document records the approved purpose and current architectural direction. Detailed implementation contracts remain to be finalized.

## Overview

ETLWatchtower is a reusable ETL observability and monitoring platform. It is intended to provide a centralized view of the health and execution of data pipelines across multiple independent projects.

Rather than replacing a pipeline's ingestion or transformation logic, ETLWatchtower receives standardized execution information and makes pipeline activity, failures, quality signals, and performance easier to inspect.

## Established purpose and capabilities

The platform is intended to monitor:

- Pipeline execution status and run history.
- Start time, end time, and execution duration.
- Structured logs and error/exception details.
- Records extracted, processed, and loaded.
- Validation outcomes and data-quality metrics.
- Failures and overall pipeline health.

It must be reusable across projects, not hardcoded to ShopSphere or RepoTally.

## Architectural direction

The following direction has been established:

- ETLWatchtower is an **independent monitoring platform**.
- Pipeline projects retain ownership of their own ingestion and processing logic.
- Integrations use a standardized reporting contract, API, or SDK. The exact mechanism is still to be selected.
- Monitoring data remains logically separate from each pipeline's business data.
- The platform should support multiple projects and pipelines through a consistent interface.

### Initial run metadata contract

The initial integration concept includes these fields:

| Field | Purpose |
|---|---|
| Project ID | Identifies the source project |
| Pipeline ID | Identifies the pipeline within that project |
| Run ID | Uniquely identifies one execution |
| Start / end timestamps | Records execution timing |
| Execution status | Reports the run outcome |
| Extracted / processed / loaded counts | Reports processing volume |
| Validation results | Reports checks and quality outcomes |
| Error details | Captures relevant failure information |

This is an initial contract outline, not a finalized API schema. Field types, required/optional rules, event semantics, authentication, retries, and versioning still need design approval.

### Intended integration flow

```text
Independent ETL pipeline
          |
          | standardized run information
          v
   ETLWatchtower interface
          |
          v
  Monitoring data store
          |
          v
 Run history / logs / health
          |
          v
 Monitoring dashboard / reports
```

## Development and integration sequence

1. Build ETLWatchtower as an independently functioning platform.
2. Validate it using controlled test pipelines and/or the completed ShopSphere pipeline.
3. Integrate RepoTally so its executions report to ETLWatchtower from the first meaningful pipeline runs.

RepoTally remains responsible for its 20-repository selection and GitHub ingestion. ETLWatchtower only observes and reports on those executions.

## Database direction

The portfolio direction is to use one PostgreSQL database, `data_engineering_portfolio`, with separate schemas and database roles for individual projects. Monitoring records should be isolated from business datasets; no forced shared business data model is intended.

The exact ETLWatchtower schema and deployment topology remain to be designed.

## Non-goals

- Replacing pipeline-specific ingestion or transformation code.
- Becoming a general-purpose workflow orchestrator.
- Being coupled to one project or one ETL implementation.
- Owning RepoTally's repository selection or GitHub API ingestion.

## Current status

**Purpose and high-level architecture approved; integration mechanism, detailed data model, API/SDK, and UI design remain open.**

---

*This README is a temporary project brief. Do not treat the initial metadata list or flow as a finalized implementation specification.*
