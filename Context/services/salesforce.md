# Service: Salesforce

**Role:** CRM, enrolment flow management, advisor tooling, student records. The central source of truth for all prospect, applicant, and student data.

---

## What it owns

- Lead and contact records for all prospects and students
- Enrolment application stages (30-step pipeline)
- Advisor task queues and communication logs
- Student support cases
- Custom objects for programme enrolment, deferrals, and withdrawals

---

## Conventions

- Stage transitions are managed via Salesforce Flow — do not write stage changes directly via API unless Flow cannot support the logic.
- All new custom fields require a naming convention review before deployment (prefix with programme or team namespace).
- Sandbox testing is required before any Flow or schema change reaches production.
- Reports and dashboards filtering on enrolment stages must be audited whenever stage picklist values change.

---

## Gotchas

- Adding a new stage picklist value does not automatically include it in existing reports — ops team must review all active reports.
- Removing or renaming a stage value breaks any Flow or automation referencing that value by name.
- Salesforce ETL into Snowflake runs nightly — new fields or stages added to Salesforce will not appear in Snowflake until the ETL mapping is updated and redeployed.
- Formula fields and rollup summaries have governor limit implications at scale — flag to the Salesforce engineer before adding.

---

## What not to touch without human sign-off

- Production Flow configurations — always test in sandbox first
- Stage picklist values on the enrolment object
- Any field storing personal data (PII) — requires DPO awareness
- Report or dashboard filters used in live advisor or ops workflows
- Permission sets and profiles — access changes have immediate effect

---

## Human gates

| Gate | Reason | Approver |
|---|---|---|
| Schema changes (new fields, objects) | Affects ETL, reports, downstream tools | Salesforce engineer + ops |
| Stage picklist changes | Breaks existing reports and automations silently | Ops / analytics |
| PII field changes | GDPR data retention and processing basis | DPO / legal |
| Flow changes in production | Live impact on enrolment pipeline immediately | Salesforce engineer |
