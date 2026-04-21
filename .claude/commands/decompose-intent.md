# decompose-intent

Turn a stakeholder request into a structured execution plan for human review before any implementation begins.

## Input

$ARGUMENTS — the stakeholder's request, verbatim. May include additional context or constraints provided by a human alongside the raw ask.

## Steps

### 1. Read the service landscape

Read Context/hult-service-landscape.md in full. This contains five scenario pipelines showing how Hult's services connect. Do not skip this step — it is how you determine which services are likely affected.

### 2. Identify relevant pipelines

Match the stakeholder request against the five scenarios. A request may span more than one pipeline — flag this if so. Name the scenario(s) you are drawing from and briefly explain why.

### 3. Fetch only the relevant service files

From the matched pipelines, identify the specific services that are likely affected. Read only those files from Context/services/. Do not load service files for services that are not relevant to this request. List which files you are reading and why before you read them.

### 4. Check confidence before proceeding

Before producing the full execution plan, assess whether you have sufficient context to propose it responsibly.

For each affected service, rate your confidence on three dimensions:
- **Scope clarity** — do you know what the change requires across this service?
- **Constraint coverage** — do the service files give you enough about conventions, gotchas, and human gates?
- **Downstream risk** — are you confident you have identified all services that could be affected?

If any dimension is rated **low** for any affected service, or if the request is ambiguous in a way that would lead to two materially different plans, **stop here and ask the human before producing the plan.**

Internally note each gap, then output only the following to the human:

---

**Before I produce the plan, I need to clarify a few things:**

[Numbered questions only. Do not show your internal gap analysis or reasoning. Ask only what is necessary to remove the ambiguity — do not ask for information you can infer from the service files.]

---

**If no gaps:** State "No blocking uncertainties — proceeding to execution plan." and continue to step 5.

---

Do not produce the execution plan until either: (a) no gaps exist, (b) the human has answered the questions above, or (c) the human says "skip" or "proceed anyway" — in which case note the unresolved uncertainties briefly and continue to the execution plan using your best assumptions.

### 5. Produce the execution plan

Output the following sections. Be specific and honest — if something is uncertain, flag it rather than guessing.

---

**EXECUTION PLAN**

**Request received:** [Repeat the stakeholder request verbatim]

**Relevant pipeline(s):** [Name the scenario pipeline(s) matched and why]

**Affected services:** [Table: service name | what changes | confidence (high/medium/low)]

**Proposed approach per service:** [For each affected service: what the agent would do, in plain language. Reference the service file for conventions and constraints.]

**Roadblocks identified:** [Anything that could prevent clean execution: missing information, third-party constraints, irreversible operations, unclear requirements. Be direct — do not omit blockers to make the plan look cleaner.]

**Human approval gates:** [Table: gate | reason | who should approve. These must be acknowledged by the human before the relevant service work begins. Do not proceed past a gate without confirmation.]

**Verification steps:** [Per service: how the agent confirms the change worked correctly. Reference test types: user-flow test, logic test, API contract check, post-deploy health check.]

**Rollback plan:** [Per service: how the change is reversed if a problem is detected. Flag explicitly if any operation is irreversible.]

**What I will NOT proceed with until you confirm:** [Explicit list of actions held pending human sign-off. This should include all items from the human approval gates above, plus any operation flagged as irreversible in the rollback plan.]

---

### 6. Wait for human sign-off

Do not proceed to implementation. Present the execution plan and ask:

"Please review the plan above. Confirm to proceed, or tell me what to adjust. Flag any approval gates you have reviewed and approved."

Only begin implementation after the human explicitly confirms.
