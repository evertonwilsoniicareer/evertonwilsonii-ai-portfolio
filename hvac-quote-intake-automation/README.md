# Automated HVAC Quote-Request Intake, Routing, Approval & Audit Logging

**Everton Wilson II · LaunchCode / Hire Human — Agentic Specialist Capstone · Final evidence package v5.0, July 31, 2026**

> Final measured pilot: **3/3 correct routes · 16 successful action steps · handling time −36%** including human review.

## The problem

At an HVAC distributor, quote requests arrive as unstructured emails. A sales rep spends about **12 minutes** on each one: reading it, working out which specs are missing, chasing the customer, and deciding whether it can be quoted, needs clarification, or needs a specialist. The information that matters (system type, tonnage, phase, voltage, refrigerant, orientation, quantity) is buried in prose, and the decision about who handles it is made from memory.

## What I built

A **19-step Zapier workflow** ("HVAC Quote Request Processing — Zap with AI") that turns each email into structured, routed, logged work — and never sends anything without a human.

```
Gmail (labeled email)
   │
   ▼
AI by Zapier — Extract & Classify  ──►  fixed JSON: 7 specs + confidence + missing_fields + risk_flags + route
   │
   ▼
Paths (deterministic)
   ├── QUOTE_READY ──► Sheets equipment lookup ──► AI quote-ready draft ──► HUMAN IN THE LOOP approval
   │                    ──► Slack notify ──► Gmail draft ──► Sheets log ──► Zapier Tables audit record   (8 steps)
   ├── NEEDS_INFO  ──► AI missing-information draft ──► Gmail draft ──► Sheets log                        (4 steps)
   └── MANUAL_REVIEW ► AI specialist summary + customer hold draft ──► Gmail draft ──► Sheets log         (4 steps)
```

**Stack:** Zapier (Paths, Human in the Loop, Tables) · AI by Zapier · Gmail · Google Sheets · Slack

## Routing rules

| Route | When |
|---|---|
| `QUOTE_READY` | All seven required fields present, confidence ≥ 90, single-phase, R454B, no conflicts or risk flags |
| `NEEDS_INFO` | One or more required fields missing, request unclear, or confidence < 90 |
| `MANUAL_REVIEW` | Three-phase, R410A, conflicting specs, unsupported configuration, or any technical / pricing / safety / compliance / data-quality risk |

"When uncertain, choose the safer route."

## Governance controls

| Control | Implementation |
|---|---|
| No automatic final commitment | Customer-facing messages are created as **Gmail drafts**, never sent |
| Human approval | `QUOTE_READY` pauses at a **Human in the Loop** step before any downstream communication |
| Safer routing | Missing data → `NEEDS_INFO`; 3PH, R410A, conflicts, unusual conditions → `MANUAL_REVIEW` |
| No guessing | Prompts must return **null** when a specification is not explicitly stated — never 0, a default, or an inference from "complete system" |
| No invention | No model numbers, prices, availability, lead times, warranties, or code statements may be generated |
| Auditability | Route, route reason, confidence, risk flags, timestamps, and outcomes logged to Google Sheets and Zapier Tables on every run |
| Specialist escalation | `MANUAL_REVIEW` remains stopped pending a qualified human decision |

## Measured results — three-run pilot

| Test | Expected | Observed | Correct | Automated wall-clock | Human review | Total handling | Successful actions |
|---|---|---|---|---|---|---|---|
| T01 | QUOTE_READY | QUOTE_READY | Yes | 2.8 min | 6.5 min | 9.3 min | 8 |
| T02 | NEEDS_INFO | NEEDS_INFO | Yes | 0.5 min | 7.0 min | 7.5 min | 4 |
| T03 | MANUAL_REVIEW | MANUAL_REVIEW | Yes | 0.33 min | 6.0 min | 6.33 min | 4 |

- Pilot routing accuracy: **3 ÷ 3 = 100%**. Manual baseline on five representative emails: **5 ÷ 5 = 100%**.
- Average handling time **7.71 min** vs. **12.0 min** manual baseline → **4.29 min saved per request (36%)**.
- At 75 requests/week (3,900/year): **~278.8 labor hours** and **~$7,808** gross annual labor value at $28/hr, before production Zapier pricing. *Projected annual extension based on the measured three-run pilot.*

## The prompt-improvement cycle (the part I'd show first)

Same test email (T02): "Complete AC system using R454B, quantity 1" — tonnage, phase, voltage, and orientation omitted.

| | v1.0 (original) | v2.0 (final) |
|---|---|---|
| Route | NEEDS_INFO | NEEDS_INFO |
| Missing fields | tonnage, phase, voltage, orientation | phase, tonnage, voltage, orientation |
| **Confidence** | **100** | **42** |
| Output contract | Loose field list | Fixed JSON schema with stable, loggable keys |
| Safety rules | No guessing + basic escalation | Explicit no-send, no-invention, safer-route, and risk-flag rules |

The v1.0 prompt reached the right route but reported **100% confidence on an email missing four required fields** — an overconfident model that would have been trusted downstream. v2.0 calibrates confidence, adds `risk_flags` and `route_reason`, and makes the human-approval boundary explicit. Both prompts are in [`prompts/`](./prompts/).

## What's in this folder

```
prompts/
  step-02-extract-and-classify-v1.0-ORIGINAL.txt   ← the "before" prompt
  step-02-extract-and-classify-v2.0.txt            ← final operating prompt, July 30, 2026
  step-06-quote-ready-response-v2.0.txt
  step-13-missing-information-response-v2.0.txt
  step-17-manual-review-response-v2.0.txt
test-emails/
  five-email-manual-baseline.txt                   ← the baseline sample and manual classifications
evidence/zapier-screenshots/                       ← Zap canvas, run history, step data-out for T01/T02/T03
docs/
  Evidence_Package_FINAL.pdf                       ← full 13-page submission
```

No API keys, OAuth tokens, passwords, or webhook URLs appear in any file or image.

## What I'd do next

1. Run a 30-request production pilot to replace the three-run figures with a real distribution of route mix and review times.
2. Add a fourth path for **quote revisions** (the second-most-common inbound email type).
3. Replace the Google Sheets equipment lookup with a direct ERP query (Epicor P21) so `QUOTE_READY` drafts carry live pricing under the same human-approval gate.
