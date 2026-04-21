# Service: AWS Lambda

**Role:** Backend logic layer, event-driven functions, schedulers. Sits between Salesforce, Adyen, Snowflake, and the email service — orchestrating triggers and background tasks.

---

## What it owns

- Event-triggered functions responding to Salesforce stage changes
- Billing schedulers (suspend, resume, modify Adyen mandates)
- Data sync jobs between services
- Notification dispatch triggers (feeds into email service)
- Scheduled background tasks (reminders, follow-ups, ETL prep)

---

## Conventions

- Each function has a single responsibility — do not extend an existing function's scope; create a new one.
- All configuration (API keys, thresholds, service URLs) via environment variables — never hardcoded.
- Functions are written in Node.js or Python — match the language of the service area being touched.
- All functions connected to Adyen or financial data must log every operation with timestamps for audit purposes.

---

## Gotchas

- Cold start latency affects infrequently-called functions — consider provisioned concurrency for user-facing flows.
- Default Lambda timeout is 3 seconds — long-running jobs (ETL, bulk Salesforce updates) need explicit timeout configuration.
- Functions with Adyen write access carry PCI compliance scope — any change to these functions requires a security review.
- Event ordering is not guaranteed for high-volume Salesforce triggers — design functions to be idempotent.

---

## What not to touch without human sign-off

- Billing scheduler functions (Adyen suspend/resume logic)
- Any function with write access to Adyen or Flywire
- Functions processing student PII
- Environment variable configuration in production
- IAM roles and execution permissions

---

## Human gates

| Gate | Reason | Approver |
|---|---|---|
| Billing scheduler changes | Directly affects live payment mandates | Finance + engineer |
| PII-processing function changes | Data handling compliance | DPO / legal |
| New external API integrations | Security scope review required | Engineering lead |
| Production environment variable changes | Immediate effect, no deploy needed | Engineering lead |
