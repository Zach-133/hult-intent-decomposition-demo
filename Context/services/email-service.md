# Service: Email Service

**Role:** Transactional email for all student, applicant, and advisor communications. Triggered by Salesforce stage changes and Lambda events. Covers the full student lifecycle from first enquiry through to programme completion.

---

## What it owns

- Enquiry and lead nurture emails (triggered by marketing pipeline)
- Enrolment stage-change notifications to applicants and advisors
- Payment confirmation and billing event emails
- Student support communications
- Advisor task and alert notifications

---

## Conventions

- All email templates are versioned — do not edit a live template in place; create a new version and switch the trigger to it.
- Every HTML email must have a plain text fallback.
- Unsubscribe and opt-out handling must be preserved in any template change — do not modify the footer unsubscribe block.
- Template changes take effect immediately for the next triggered send — there is no staging buffer for email.

---

## Gotchas

- Template changes are live immediately — a typo or broken link in a template will go out to real students on the next trigger.
- Unsubscribe logic is legally required under GDPR — any change to templates must preserve the unsubscribe mechanism.
- Sender domain configuration (SPF, DKIM, DMARC) affects deliverability — do not modify DNS records for the sending domain without specialist review.
- Emails sent to real students during testing cannot be recalled — use test accounts or sandbox sending modes only.

---

## What not to touch without human sign-off

- Unsubscribe and opt-out logic in any template
- Sender domain configuration
- Templates used in active enrolment or payment flows
- Any bulk send or broadcast to existing student lists

---

## Human gates

| Gate | Reason | Approver |
|---|---|---|
| Student-facing copy changes | Brand, accuracy, and legal compliance | Marketing |
| Compliance-related notifications | Legal language must be reviewed | Legal |
| Unsubscribe mechanism changes | GDPR requirement | DPO / legal |
| New trigger configuration | Ensures correct audience receives correct email | Marketing + engineering |
