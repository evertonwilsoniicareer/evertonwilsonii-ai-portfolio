# Focus Agent — Refinement Log

Every change to the instruction template gets one line here. The log is the difference between a system that improves and one that drifts.

| Date | Version | What changed | Why (what went wrong or what was learned) | Changed by |
|---|---|---|---|---|
| 2026-01-05 | 1.0 | Initial template | — | [NAME] |
| 2026-01-07 | 1.1 | Added "max five DUE TODAY" cap to Plan the day | Agent listed eleven items as due today; user stopped reading the list | [NAME] |
| 2026-01-09 | 1.2 | Calendar-beats-message rule added to Truth Rules | Agent filed a meeting time quoted in an email that conflicted with the invite | [NAME] |
| 2026-01-12 | 1.3 | Plan the day must state which sources were and were not read | Brief implied mail had been checked when the connector had timed out | [NAME] |

## Rules for this log
- One line per change. Link or paste the changed passage if it is long.
- Record the failure that prompted the change, not just the fix. The failure is the lesson.
- If several people run their own Focus Agent from one shared template, only the template owner edits the template; everyone else reports issues to the owner so the fix reaches every copy.
