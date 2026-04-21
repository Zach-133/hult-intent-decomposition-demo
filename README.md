# README — Hult/Zachery's Interview Demo

**This project is for demonstration purposes only for Hult Business School's interview procedure. It is not a production system.**

---

## Purpose

This folder demonstrates one stage (deconposition) of a proposed agentic software delivery system, in which an AI agent takes a stakeholder's plain-language request and produces a structured execution plan for human review before any implementation begins. It does not cover other stages of the lifecycle such as implementation, verification, deployment, or the feedback loop.

The demonstration uses Claude Code's native slash command system. Running `/decompose-intent [stakeholder request]` triggers the decomposition skill, which reads the service landscape, identifies affected services, and outputs a proposed execution plan for human sign-off.

---

## Important caveats on the content

**All files inside the `Context/` folder are assumed.** They do not accurately represent how Hult's software architecture works, how its services are configured, or what conventions and constraints its engineering team follows.

The service files (Salesforce, AWS Lambda, Snowflake, Adyen, etc.) and the pipeline scenarios in `hult-service-landscape.md` are informed by publicly available information from Hult's job description and Zachery's Round 1 interview conversation with Alex. They are illustrative — written to demonstrate the *pattern* and *structure* of how context files would work, not to document how Hult's systems actually operate.

**In a real implementation**, each file in `Context/services/` would be substantially richer — populated by the engineering team with accurate service ownership, real API conventions, known edge cases, specific gotchas discovered in production, off-the-shelf tool configurations, compliance constraints, and any nuances that only become apparent through working with the systems directly. The pipeline scenarios in `hult-service-landscape.md` would reflect how the services actually connect, not how they are assumed to connect from the outside.

The value of this artefact is in the architecture and the pattern — not in the specific content of the context files.

---

## File structure

```
CLAUDE.md                          Orchestrator — routes requests, enforces hard rules
.claude/commands/
  decompose-intent.md              Native slash command — invoked as /decompose-intent
Context/
  hult-service-landscape.md        Five assumed scenario pipelines + service index
  services/
    salesforce.md                  Assumed CRM and enrolment flow context
    aws-lambda.md                  Assumed backend logic and scheduler context
    snowflake.md                   Assumed data warehouse and reporting context
    react-marketing-site.md        Assumed marketing site context
    adyen.md                       Assumed payments platform context
    datadog.md                     Assumed monitoring and alerting context
    email-service.md               Assumed transactional email context
    internal-tools.md              Assumed internal ops tooling context
```

---

## Design decisions

**Why a 3-tier architecture (CLAUDE.md → skill → context files) rather than one flat file?**
Loading everything into CLAUDE.md would bloat the context window before any work begins. The orchestrator stays lean and always-loaded. The skill is only invoked when a decomposition is needed. The context files are fetched selectively — only the services relevant to the specific request are loaded. This keeps token cost proportional to the complexity of the task, not the size of the entire service estate.

**Why is the service landscape a separate index rather than detail inline?**
`hult-service-landscape.md` is a lightweight index the skill always reads to identify which services are affected. Individual service files carry the detail. This means the skill reads one small file to make routing decisions, then fetches only the files it actually needs — rather than loading all service detail upfront on every run.

**Why individual service files rather than one combined context file?**
Each service file can be updated independently as understanding of that service grows. A single combined file would require editing a large document every time one service changes, and would force the agent to load context for all services even when only one is relevant.

**Why decomposition only — what is deliberately excluded?**
This demo covers one stage of the factory: turning a stakeholder request into a structured execution plan for human review. The other lifecycle stages — implementation, verification, deployment, monitoring, and the feedback loop — are not implemented here. Each would require its own skill and potentially its own context layer. Decomposition was chosen as the demonstration stage because it is the highest-leverage point in the lifecycle and the one most teams currently skip.

**What would a real implementation require?**
Each file in `Context/services/` would need to be populated by the engineering team with accurate service ownership, real API conventions, known edge cases, production gotchas, and compliance constraints. The pipeline scenarios in `hult-service-landscape.md` would reflect how services actually connect in practice. Additional skills would be built for the remaining lifecycle stages. MCP servers or API integrations would be connected so the agent can take real action after the execution plan is approved.

---

## How to run the demo

1. Clone this repo and open the folder in Claude Code (`claude` from the repo root, or open via the Claude Code desktop app)
2. Type: `/decompose-intent [stakeholder request]`
3. Claude Code will read the service landscape, identify affected services, fetch only the relevant service files, and output a structured execution plan
4. The execution plan is the demonstration — the agent will wait for human sign-off and will not proceed further in this demo context
