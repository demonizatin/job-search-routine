# The tracker

One Google Sheet with five tabs. The agent reads all of it at the start of a run and writes to three of the tabs. Two tabs are mine to edit.

## Main tab (agent writes, I read)

One row per application or outreach target.

| Column | Meaning |
|---|---|
| Date found | When the agent added the row |
| Company, Role | As stated on the posting |
| Type | `Application` or `Outreach` |
| Source | Where it came from, including the board and posting date |
| Job link | The company's own apply link where one exists |
| Remote eligibility | A category letter plus the stated location (see below) |
| Match (1-5) | Fit with my profile; rows below 3 are not added |
| Resume used | `General` or `AI-focused` |
| Contact email | Only ever a published address |
| Status | `Ready`, `Draft ready`, `Sent`, `Replied`, `Interview`, `Rejected`, `Closed`, `Needs me` |
| Date sent, follow-up dates, last reply date | Filled as the thread moves |
| Notes | Apply link, unanswered form questions, anything odd |

Location categories:

- **A** remote at a company outside India, open to India
- **B** remote within India
- **C** based in Bengaluru
- **D** another Indian city, flagged as relocation

## Run log (agent writes)

One row per run: time, roles seen, rows added, drafts created, replies found, follow-ups drafted, errors, boards checked. This is how I tell a run that found nothing from a run that did not look.

## Boards (agent writes)

Every company board the agent has tried: company, which applicant-tracking system, the address that worked, whether it works, and when it was last checked. The agent starts each run from the working ones.

## Answers (I write)

Standard answers the agent uses when preparing applications: current and expected pay, notice period, work authorisation, time zones, reason for leaving. An answer here replaces the default in the prompt.

## Rules (I write)

Search criteria I can change without touching the prompt: titles to include or skip, experience range, locations, companies to prefer or exclude, and the caps. The prompt tells the agent to apply only rows of those kinds and to report any row that asks for something else.
