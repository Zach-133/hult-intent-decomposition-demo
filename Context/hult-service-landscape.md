# Hult Service Landscape

This file describes how Hult's services connect across five common scenarios. Use this to identify which services are likely affected by a given stakeholder request before fetching individual service files.

Each pipeline shows the sequence of services involved and the nature of their relationship. Services listed first in a pipeline are often upstream dependencies — changes there can have downstream effects.

---

## Pipeline 1 — Marketing and Lead Generation

A prospective student discovers Hult, engages with marketing content, and submits an enquiry or application.

```
React marketing site → Salesforce (lead capture, enquiry routing) → Email service (lead nurture, response confirmations)
```

**Common change types:** website copy, landing pages, form fields, lead routing rules, email sequences for new enquiries.

**Downstream risk:** form field changes on the marketing site affect what data lands in Salesforce. Lead routing rule changes affect which advisor receives the enquiry. Always check both ends.

---

## Pipeline 2 — Student Application and Enrolment

A lead becomes an applicant and moves through the 30-step enrolment process through to enrolled student status.

```
Salesforce (enrolment stages, advisor tooling) → AWS Lambda (process orchestration, event triggers) → Adyen / Flywire (payment collection, billing mandates) → Email service (stage-change notifications, payment confirmations) → Snowflake (enrolment reporting, pipeline metrics)
```

**Common change types:** new enrolment stages, payment schedule changes, advisor workflow updates, notification triggers, reporting metrics.

**Downstream risk:** this is the highest-risk pipeline. Salesforce stage changes affect Lambda triggers, which affect Adyen billing events, which affect Snowflake reporting. A change to one node can silently break another. Treat this pipeline as a whole when scoping any request.

---

## Pipeline 3 — Enrolled Student Management

An enrolled student interacts with Hult systems throughout their programme — tracking progress, submitting work, receiving support.

```
Salesforce (student record, support cases) → AWS Lambda (background processing, scheduled tasks) → Email service (advisor communications, reminders, alerts) → Snowflake (academic tracking, completion data)
```

**Common change types:** student record fields, support workflows, communication templates, academic data schema, programme milestones.

**Downstream risk:** student record changes in Salesforce flow into Snowflake via nightly ETL. Changes to academic data schema require ETL updates to be deployed in the same release window.

---

## Pipeline 4 — Reporting and Analytics

Internal teams consume data to track enrolment performance, financial health, and operational metrics.

```
Snowflake (primary data warehouse, all reporting schemas) → Datadog (operational monitoring, system health dashboards) → Internal tools (advisor dashboards, ops reporting)
```

**Common change types:** new report fields, dashboard updates, metric definitions, alert thresholds, data pipeline jobs.

**Downstream risk:** Snowflake schema changes break downstream reports and internal tools that query those schemas. Always identify all consumers of a schema before changing it.

---

## Pipeline 5 — Operations and Internal Tooling

Admissions advisors and operations staff manage day-to-day workflows, communications, and administrative processes.

```
Internal tools (advisor-facing dashboards, process tooling) → Salesforce (data source for advisor workflows) → AWS Lambda (background tasks, integrations)
```

**Common change types:** advisor workflow updates, internal dashboard changes, process automation, administrative tooling.

**Downstream risk:** internal tools are often used during live advisor calls with prospective students. Changes to advisor-facing tooling should be communicated and tested with the ops team before rollout.

---

## Service index

| Service | Primary role | Relevant pipelines |
|---|---|---|
| React marketing site | Public website, lead capture forms | 1 |
| Salesforce | CRM, enrolment flow, advisor tooling, student records | 1, 2, 3, 5 |
| AWS Lambda | Backend logic, event triggers, schedulers | 2, 3, 5 |
| Adyen / Flywire | Payment collection, recurring billing mandates | 2 |
| Email service | Transactional notifications, advisor communications | 1, 2, 3 |
| Snowflake | Data warehouse, reporting, analytics | 2, 3, 4 |
| Datadog | Monitoring, alerting, system health | 4 |
| Internal tools | Advisor dashboards, ops process tooling | 4, 5 |
