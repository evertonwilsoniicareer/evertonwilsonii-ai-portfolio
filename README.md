# Everton Wilson II — AI Implementation Portfolio

**AI Implementation Engineer | Turning Business Problems into Practical AI Solutions**
Greater St. Louis, MO · [LinkedIn](https://www.linkedin.com/in/evertonwilsonii) · evertonwilsonii.career@gmail.com

I work at the intersection of business operations and technology: finding where AI and automation remove manual effort, designing governed human-in-the-loop solutions, building them, rolling them out, and measuring the result. Nearly ten years in operations, analysis, and delivery across distribution, financial services, SaaS, and telecom; SAFe 6 SSM / POPM / RTE; LaunchCode Agentic Specialist.

Both projects below follow the same discipline: **extract only what is stated, label facts versus inferences, route the uncertain cases to a human, never send without approval, and log everything.**

---

## Projects

### 1. [Automated HVAC Quote-Request Intake, Routing, Approval & Audit Logging](./hvac-quote-intake-automation/)
*LaunchCode Agentic Specialist Capstone · Zapier + AI by Zapier · July–August 2026*

A 19-step workflow that reads unstructured HVAC quote emails, extracts seven required specs into a fixed JSON contract, routes each request to **QUOTE_READY**, **NEEDS_INFO**, or **MANUAL_REVIEW**, drafts the customer response, pauses for human approval, and logs every run.

- **3 of 3** pilot routes correct · **16** successful action steps · manual baseline **5 of 5**
- Handling time **12.0 → 7.71 min** per request (**−36%**) including human review
- Projected **~279 labor hours / ~$7,800** gross annual labor value at 75 requests/week
- Documented prompt-improvement cycle: v1.0 scored **100** confidence on an email missing four fields; v2.0 scored **42** and listed them

Includes the four versioned operating prompts, the original v1.0 prompt for comparison, the five-email baseline, Zapier run screenshots, and the full evidence package.

### 2. [The Focus Agent — A Personal AI Work Secretary](./focus-agent/)
*Reference implementation · Claude Projects + Microsoft 365 (read-only) · 2026*

A personal secretary inside an AI assistant that keeps a dated record of a person's tasks, meeting outcomes, and commitments, reads their own calendar, mail, and team chat strictly read-only, and answers "what should I be doing right now?" with one sourced answer. Designed, built, and piloted on live data with a self-service setup of about 10 minutes per user, no IT help.

- Nine plain-language behaviors defined in one master instruction template
- Guardrails written into the rules: read-only always, nothing logged without a yes, every fact names its source, guesses labeled as inference
- First scan surfaced a colleague's request that had sat unanswered in chat for five days

Includes a fully generalized instruction template, setup guide, notebook template, and refinement-log template — a complete reference implementation any organization can adopt.

> Independently written reference implementation of a design I built and piloted in a workplace setting. No employer documents or data are included.

---

## Tools used across both projects
Claude (Projects, Docs, connectors) · AI by Zapier · Zapier (Paths, Human in the Loop, Tables) · Microsoft 365 (Outlook, Exchange, Teams, Forms) · Microsoft Copilot · ChatGPT · Perplexity · Gmail · Google Sheets · Slack · Airtable · JSON output contracts · prompt versioning · baselining and evaluation · AI-assisted Python scripting (OpenCode, Claude) · PowerShell · WSL · Docker Desktop
