# Service: Datadog

**Role:** Monitoring, alerting, and observability across Hult's production services. Primary tool for detecting issues post-deploy and for ongoing system health visibility.

---

## What it owns

- Error rate and latency monitors across Lambda functions
- Synthetic monitors for critical user flows post-deploy
- Alert routing to on-call engineers
- Dashboards for enrolment pipeline health and operational metrics
- Log aggregation from Lambda and application services

---

## Conventions

- New services or Lambda functions should have baseline monitors configured before go-live — not after.
- Synthetic monitors must be updated when a user flow changes (new form fields, URL changes, stage transitions) — stale synthetics produce false positives or false negatives.
- Alert thresholds are reviewed quarterly — do not set thresholds without checking existing baselines first.
- On-call routing is configured per service area — a Lambda billing function alert routes differently to a marketing site alert.

---

## Gotchas

- Alert fatigue: thresholds set too low generate noise and cause on-call engineers to ignore genuine alerts.
- Synthetic monitors that test a UI flow will fail silently if the flow changes and the monitor is not updated — this looks like the flow is healthy when it is not.
- Log retention policies mean historical debugging data is only available for a limited window — escalate promptly if an incident needs investigation.

---

## What not to touch without human sign-off

- On-call alert routing configuration
- Threshold changes on monitors covering billing or enrolment pipeline health
- Disabling or pausing any active monitor without a documented reason and reactivation plan

---

## Human gates

| Gate | Reason | Approver |
|---|---|---|
| On-call routing changes | Affects who gets paged during an incident | Engineering lead / ops |
| Billing pipeline monitor changes | Risk of missing a payment processing failure | Engineering lead |
| Monitor disable or pause | Creates a blind spot in production | Engineering lead |
