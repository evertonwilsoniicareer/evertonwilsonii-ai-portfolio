# Focus Agent — Setup Guide

About 10 minutes, once. After that you just talk to it. No IT help required.

## What you need
- An AI assistant that supports **project-level custom instructions**, a **document it can read and write**, and **read-only connectors** to your calendar, mailbox, and team chat. This guide uses Claude Projects with the Microsoft 365 connector; the steps map directly to Google Workspace.
- Your own work account. The secretary sees only what you can see.
- The file `TEMPLATE-focus-agent-instructions.md` from this folder.

## Part A — One-time setup

**A1. Connect your calendar, mail, and chat (read-only).**
In the assistant's settings, open Connectors, choose Microsoft 365 (or Google Workspace), and sign in with your work account. Grant only read scopes if the connector offers a choice. If you have already connected it for another project, skip this step.

**A2. Create a private project.**
Name it `Focus Agent — [Your Name]`. Keep it private. This is the secretary's office: the only place it lives.

**A3. Paste the instructions.**
Open the project's instructions box. Open `TEMPLATE-focus-agent-instructions.md`, copy everything below the horizontal rule, and paste it in. Replace every `[BRACKETED]` value: your first name, your time zone, your calendar/mail/chat products, and the name of your weekly team meeting. Leave `[PASTE NOTEBOOK LINK]` exactly as it is for now. Save.

**A4. Let it build your notebook.**
Start a chat inside the project and type: `set up my notebook`
The secretary creates `My Focus — [Your Name]` with four sections, fills Today's plan from your calendar, and gives you the link. (If your assistant cannot create documents, create one yourself from `MY-FOCUS-doc-template.md` and share the link with the project.)

**A5. Wire the notebook in.**
Copy the link. Reopen the project instructions, replace `[PASTE NOTEBOOK LINK]` with it, save. This is the only line you will ever need to edit again.

**A6. Test.**
New chat, type: `what should I be doing right now?`
Pass = one answer, with the source of each fact named ("from the calendar invite," "from your notebook"). If you get a list instead of one item, or a fact without a source, the instructions did not save — repeat A3.

**A7. Bookmark the notebook.**
The notebook opens as a live page in your browser and updates as the secretary writes. Bookmark it. Your whole day is then one click away, no chat needed.

## Part B — Daily use

There is nothing to learn. Say things; ask things.

| You say | What happens |
|---|---|
| `add: send the revised budget, due Friday` | Filed under Open tasks with today's date |
| `finished the vendor comparison` | Moved to Done this week |
| `waiting on Jordan for the laptop image` | Filed under Waiting on, with the name |
| `done with the hiring sync — I owe them a checklist by the 16th` | Debrief: your commitments → Open tasks, theirs → Waiting on |
| `what should I be doing right now?` | One item and the reason |
| `what's on my plate?` / `what did I get done this week?` | Read back from the notebook and calendar, dated |
| `plan my day` | Today's plan rewritten from the calendar; stale tasks flagged; max five due today |
| `am I missing anything?` | Mail and chat scanned for unanswered asks; nothing filed until you approve |
| `prep me for [weekly meeting]` | Your five standing answers, built from the week's record |

Dictation works. Ramble; it pulls out the tasks and reads back what it filed.

## What it will never do
- Send, reply, forward, post, or touch a calendar event. "Send it" produces a draft for you to send.
- Put anything on your list without your yes.
- Show your information to anyone else.
- State a guess as a fact. Guesses carry the inference label; facts carry their source.

## Troubleshooting
| Symptom | Fix |
|---|---|
| "I can't see your calendar" | Connector not connected or expired — repeat A1 |
| It made a new list instead of using the notebook | Say `use my notebook`. If it recurs, the link in A5 did not save |
| Wrong date, name, or task | Tell it. It fixes the entry and adds a dated correction note |
| It listed ten things as due today | The five-item cap is missing from the instructions — re-paste the Plan the day section |

## Running it for a team
One person owns the template. Each teammate copies it into their own private project and connects their own account. Nothing is shared between people. When a rule needs to change, the owner changes the template once, logs it in `REFINEMENT-LOG-template.md`, and tells everyone to re-paste — so an improvement made once reaches every copy. Recommended rollout: one person for a week → two or three volunteers for a week including one full weekly-meeting cycle → everyone.
