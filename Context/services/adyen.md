# Service: Adyen

**Role:** Primary payment platform for enrolment fees and recurring billing. Handles initial payment collection and ongoing billing mandates for enrolled students. Flywire handles international payment routing for certain markets.

---

## What it owns

- One-time payment collection at enrolment
- Recurring billing mandates for instalment plans
- Refund processing
- Webhook events dispatched to Lambda on payment status changes

---

## Conventions

- All payment operations are triggered via Lambda — never directly from the frontend or Salesforce.
- Test all payment flows against Adyen sandbox before any production change.
- Every payment operation must be logged by the Lambda function with a timestamp and the Adyen payment reference.
- Flywire handles specific international markets — check which markets are routed via Flywire before assuming Adyen covers all.

---

## Gotchas

- Pausing a billing mandate is not the same as cancelling it. Pause preserves the mandate for resumption. Cancellation requires re-authorisation from the student to restart billing. These are different API operations with different consequences.
- Mandate modifications (amount, frequency, schedule) may require re-authorisation depending on the terms agreed at sign-up.
- Adyen webhook retries — if Lambda fails to acknowledge a webhook, Adyen retries. Duplicate payment events must be handled idempotently.
- Refunds on instalment plans require confirmation of which instalments have been collected before processing.
- PCI compliance scope applies to any Lambda function with Adyen API write access.

---

## What not to touch without human sign-off

- Any live billing mandate operation (pause, cancel, modify)
- Webhook endpoint configuration
- Refund logic
- API key rotation or credential changes
- Any change affecting students currently in an active payment plan

---

## Human gates

| Gate | Reason | Approver |
|---|---|---|
| Mandate pause or cancel operations | Irreversible or requires re-auth from student | Finance |
| Billing schedule changes | Direct financial impact on enrolled students | Finance |
| Refund logic changes | Revenue and reconciliation risk | Finance |
| Webhook configuration | Payment event pipeline depends on this | Engineering lead |
| Any production payment operation | PCI compliance and audit trail | Finance + engineering |
