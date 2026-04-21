# Service: Internal Tools

**Role:** Advisor-facing dashboards and operational process tooling. Used daily by admissions advisors and ops staff to manage student pipelines, communications, and administrative workflows.

---

## What it owns

- Admissions advisor dashboards (pipeline views, task queues)
- Operational reporting for management and ops teams
- Process automation for administrative workflows
- Custom tooling built on top of Salesforce and Lambda data

---

## Conventions

- Changes to advisor-facing tools must be communicated to the ops team before rollout — advisors use these tools in live calls with prospective students.
- New features should be tested with a small group of advisors before full rollout.
- Internal tools often query Snowflake directly — schema changes in Snowflake must be coordinated with internal tool updates in the same release window.
- Do not change column names or filter logic in advisor dashboards without verifying the underlying Snowflake query still works.

---

## Gotchas

- Some internal tools are used during live enrolment calls — an outage or broken UI during business hours has direct impact on enrolment conversion.
- Internal tools are tightly coupled to the Salesforce data model — Salesforce field or object changes often require corresponding internal tool updates.
- Access controls on internal tools are tied to Salesforce permission sets — tool access changes may need to go via Salesforce profile configuration, not the tool itself.

---

## What not to touch without human sign-off

- Advisor pipeline views used in live enrolment calls
- Snowflake query logic underlying any dashboard metric
- Access control or permission configuration
- Any rollout during peak enrolment periods without ops notice

---

## Human gates

| Gate | Reason | Approver |
|---|---|---|
| Changes to live advisor tooling | Used in real student interactions | Ops lead |
| Dashboard metric or filter changes | May alter how advisors manage their pipeline | Ops + analytics |
| Access control changes | Immediate effect on what advisors can see and do | Ops lead |
| Rollout timing during peak periods | Minimise disruption to live enrolment activity | Ops lead |
