# Service: React Marketing Site

**Role:** Public-facing marketing website. Primary channel for prospective student discovery and lead capture. Built in React, hosted on AWS.

---

## What it owns

- All public marketing pages and programme information
- Enquiry and application entry forms (feeds into Salesforce)
- SEO metadata and structured content
- Campaign landing pages

---

## Conventions

- Content-only changes (copy, images) should go through the CMS where available — avoid code changes for copy updates.
- Form field changes require a Salesforce mapping review before deployment — the form and the CRM field must match.
- All deployments go through a staging environment before production.
- URL structure changes require a redirect plan — broken URLs affect SEO and any live marketing campaigns referencing them.

---

## Gotchas

- Form submissions feed directly into Salesforce lead records — a new form field with no corresponding Salesforce field will silently drop data.
- Removing a form field that Salesforce treats as required will cause form submissions to fail.
- Page metadata changes (title, description) affect live Google indexing and any active paid campaigns using those pages.
- React build and deploy time means changes are not instant — plan for deployment lag in time-sensitive campaigns.

---

## What not to touch without human sign-off

- Form endpoint configuration or field mappings to Salesforce
- URL structure on any page with existing inbound links
- Any page actively used in a live paid campaign
- SEO metadata on high-traffic programme pages

---

## Human gates

| Gate | Reason | Approver |
|---|---|---|
| Form field changes | Salesforce mapping must be verified first | Marketing + Salesforce engineer |
| URL or page structure changes | SEO and campaign impact | Marketing |
| Live campaign page changes | Immediate revenue risk | Marketing lead |
| Copy on programme pages | Brand and accuracy review | Marketing |
