# Focus Agent — A Personal AI Work Secretary (Reference Implementation)

**Designed and built by Everton Wilson II · 2026**

> *It keeps the record. The human keeps the wheel.*

The Focus Agent is a personal secretary that lives inside an AI assistant. You talk to it in plain words — typed or spoken — and it keeps a dated, written record of your tasks, meeting outcomes, and commitments. It reads your own calendar, mail, and team chat **strictly read-only** and answers the one question everyone asks between meetings: **"What should I be doing right now?"**

It never sets your priorities, reorders your list on its own, or pushes an agenda. You drive; it records, retrieves, and answers when asked.

This folder is a **generalized reference implementation** written for any organization. It contains no employer materials.

---

## The problem it solves

One person's work lives in five places:

| Where the work lives | What goes wrong |
|---|---|
| Meetings on the calendar | The calendar knows the schedule, not what each meeting produced |
| Requests buried in mail and chat | Commitments arrive mid-thread and sit unanswered until someone notices |
| Tasks in your head | Weekly updates get reconstructed from memory, and memory drops things |

In the original live pilot, the very first mail-and-chat scan surfaced a colleague's request that had sat unanswered for five days — flagged as unconfirmed until the user approved it.

---

## Architecture — three private pieces per person

```
┌─────────────────────────────────────────────────────────────┐
│  THE OFFICE  ·  a private assistant project                 │
│  Holds the secretary's rules (the instruction template).    │
│  This is where you and the secretary talk.                  │
└───────────────┬─────────────────────────────┬───────────────┘
                │ reads & writes              │ reads only
                ▼                             ▼
┌───────────────────────────┐   ┌─────────────────────────────┐
│  THE NOTEBOOK             │   │  THE SOURCES                │
│  Live "My Focus" doc:     │   │  Your own work account:     │
│  · Today's plan           │   │  · Calendar                 │
│  · Open tasks             │   │  · Mail                     │
│  · Waiting on             │   │  · Team chat                │
│  · Done this week         │   │  Strictly read-only.        │
│  Every entry dated.       │   │  Can look; can never send   │
│  You can hand-edit.       │   │  or change anything.        │
└───────────────────────────┘   └─────────────────────────────┘
```

Nothing is shared between people. Each person's project, notebook, and connected account are theirs alone.

**Reference stack:** Claude Projects (custom instructions) · Claude Docs (the live notebook) · Microsoft 365 connector, read scope only. Portable to Google Workspace or any assistant with the same three capabilities.

---

## What it does — nine behaviors, one template

Every behavior is defined in a single instruction template ([`TEMPLATE-focus-agent-instructions.md`](./TEMPLATE-focus-agent-instructions.md)). Users trigger them in plain language; there are no commands to memorize.

| # | Behavior | You say | It does |
|---|---|---|---|
| 1 | **Capture** | "add X, due Thursday" · "finished Y" · "waiting on Jordan for Z" | Files it in the right notebook section with today's date; confirms in one line |
| 2 | **Debrief** | "done with the hiring sync — I owe them a checklist by the 16th" | Your commitments → Open tasks; theirs → Waiting on; decisions → dated notes |
| 3 | **Next** | "what should I be doing right now?" | **One** item and the reason: imminent meeting → marked top priority → nearest deadline |
| 4 | **Status** | "what's on my plate?" · "what did I get done this week?" | Reads back from the notebook and live calendar; dated Done entries are the receipts |
| 5 | **Plan the day** | "plan my day" | Rewrites Today's plan from the calendar; flags stale tasks; caps "due today" at five; states which sources it read and which it did not |
| 6 | **Scan** | "am I missing anything?" · "who is waiting on me?" | Searches recent mail and chat for unanswered asks; presents them as *possible, not confirmed*; nothing filed without a yes |
| 7 | **Weekly brief** | "prep me for the team meeting" | Builds five standing answers (priorities, wins, blockers, cross-team needs, flags) from the record, not from memory |
| 8 | **Week rollover** | first Plan the day of a new week | Archives last week's Done entries, carries unfinished tasks forward, asks for the priorities confirmed at the meeting |
| 9 | **Dictation** | rambling voice input | Pulls tasks and updates out of the filler; reads back what it filed |

---

## Guardrails — written into the rules, not bolted on

These are what make the system safe to hand to non-technical users:

- **Read-only, always.** It can never send a message, post in chat, accept an invite, or touch a calendar event. "Send it" means *draft it and show me*. A request to send is not permission to send.
- **Nothing filed without a yes.** Anything found in mail or chat stays *possible, not confirmed* until the user approves it. An unanswered message is a claim about your obligations, not proof of one.
- **Facts carry their source.** "Budget review is at 2:00 — from the calendar invite." A deduction must be labeled, verbatim: *"This is an inference from [what it saw]; treat it as unconfirmed."*
- **Calendar beats hearsay.** A time quoted in an email is a claim; the invitation is the fact. It checks the invite before writing any time into the notebook.
- **Corrections stay visible.** A wrong entry is fixed in place with a dated correction note, never silently overwritten — the trail is how repeat mistakes get caught.
- **Coverage is stated.** Every daily plan ends by naming which sources were actually read this time and which were not. It never implies coverage it did not have.
- **Private and person-final.** It sees only what its own user can see, shows it to no one else, and the user's word — and hand-edits — always win.
- **"I don't know" is a complete answer.** It never guesses a name, date, or deadline.

---

## What a pilot looked like

Tested on live data with one user before any wider rollout:

| Test | Result |
|---|---|
| **Next** — "what should I be doing right now?" | One answer; every fact's source named (calendar invite vs. the user's own notebook) |
| **Scan** — "am I missing anything?" | First run surfaced a five-day-old unanswered request from a colleague, across mail and chat; labeled as inference; nothing filed until approved |
| **Live capture** — a meeting debriefed by voice | Appeared in the notebook in real time, dated and source-labeled |
| **Change control** | Every rule change since launch has a dated entry in the refinement log |

---

## Rollout — self-service, about 10 minutes per person

1. Connect your own calendar/mail/chat account (read-only).
2. Create a private project named `Focus Agent — [Your Name]`.
3. Paste the template; fill in the bracketed values.
4. Type `set up my notebook` — the first chat builds it from your calendar.
5. Paste the notebook link into the template. Done.

Full steps, daily-use table, and troubleshooting in [`SETUP.md`](./SETUP.md).

**Phased, deliberately:** one person for a week → two or three volunteers for a week including a full weekly-meeting cycle → everyone. **One template governs every copy**, so a fix made once reaches every user, and every change is logged ([`REFINEMENT-LOG-template.md`](./REFINEMENT-LOG-template.md)).

---

## Files in this folder

| File | Purpose |
|---|---|
| `TEMPLATE-focus-agent-instructions.md` | The instruction template — paste into a project, fill the brackets |
| `SETUP.md` | One-time setup, daily use, troubleshooting, team rollout |
| `MY-FOCUS-doc-template.md` | The notebook's four-section structure with example entries |
| `REFINEMENT-LOG-template.md` | Dated change log — how the template improves without drifting |

---

## Design principles I'd carry to the next system

1. **Governance goes in the instructions, not in a policy document nobody reads.** Every guardrail above is a line the model reads on every turn.
2. **Separate what was read from what was inferred, every single time.** This one rule did more for user trust than any feature.
3. **Read-only first.** Adoption came from people knowing it *could not* send anything, not from what it could do.
4. **One template, one owner, one log.** Copies drift; a single source of truth with a refinement log does not.
5. **Baby-step documentation.** Assume zero technical background and no IT support — that is what makes a rollout self-service.
6. **Never change a system mid-test.** Integrations wait for the current test cycle to finish cleanly.

---

*This is an independently written reference implementation of a design I built and piloted in a workplace setting in 2026. No employer documents, templates, or data are included.*
Questions: evertonwilsonii.career@gmail.com
