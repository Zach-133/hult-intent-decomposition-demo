# Service: Snowflake

**Role:** Data warehouse, reporting pipeline, analytics. Downstream consumer of data from Salesforce, Lambda, and Adyen. Source of truth for all historical and operational reporting.

---

## What it owns

- Enrolment pipeline data (synced nightly from Salesforce)
- Financial records (payment events from Adyen via Lambda)
- Student academic tracking data
- Operational metrics consumed by internal tools and Datadog

---

## Conventions

- No direct writes to production schema — all changes via versioned migration scripts.
- Schema changes must be deployed before the nightly ETL runs, or the ETL will fail silently and produce gaps in reporting.
- New Salesforce fields or stages must have a corresponding ETL mapping update before they appear in Snowflake.
- All queries touching PII must be access-controlled — raw student data is not available to all internal tool users.

---

## Gotchas

- ETL runs nightly — schema changes not coordinated with the ETL team produce a 24-hour gap before data is available.
- Renaming a Snowflake column breaks any downstream report, dashboard, or internal tool querying that column by name.
- Paused, deferred, or withdrawn students must be explicitly handled in ETL logic — they are not the same as dropouts and must not appear as such in reporting.
- Historical data corrections require careful handling — a naive UPDATE can corrupt time-series reporting.

---

## What not to touch without human sign-off

- Production schema (table structure, column names, data types)
- ETL job configuration
- Access control policies on PII tables
- Historical data corrections or bulk updates

---

## Human gates

| Gate | Reason | Approver |
|---|---|---|
| Schema changes | Breaks downstream reports and tools | Data / analytics team |
| ETL mapping changes | Affects all reporting from next nightly run | Data engineer |
| PII access policy changes | Compliance and data governance | DPO / legal |
| Bulk data corrections | Risk of corrupting historical records | Data lead + engineering |
