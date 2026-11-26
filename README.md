# Job search routine

A scheduled AI agent that runs my job search three times a day. It finds product manager roles I can actually take, prepares each application, drafts outreach emails in my voice, tracks replies and queues follow-ups. I approve what goes out.

I designed it and built it with Claude Code in one day, as a product exercise as much as a job-search tool. This repo has the agent's instructions, the tracker design and what the first runs taught me.

## What it does on each run

1. **Checks replies.** For every application or email already sent, it searches Gmail for a response and updates the tracker.
2. **Queues follow-ups.** No reply after 5 days gets a drafted follow-up, another at 12 days, and the thread is closed at 19.
3. **Finds roles** from three kinds of source:
   - LinkedIn job-alert emails that land in Gmail (it never touches LinkedIn itself)
   - Company job boards, read through the public feeds of Greenhouse, Lever, Ashby and Workable
   - Job boards such as Remote OK, We Work Remotely, Cutshort and IIMJobs
4. **Filters** by title, seniority, years of experience and where the role can be done from.
5. **Prepares** each one: picks one of two resumes, notes the apply link and any form question my standard answers do not cover.
6. **Drafts outreach** to companies that fit but have no opening, only when a careers address is published.
7. **Logs the run** so I can see what it looked at and what it rejected.

## How it is put together

| Part | What it is |
|---|---|
| Agent | A Claude Code cloud routine on a cron schedule (01:00, 09:00, 17:00 UTC) |
| Instructions | One prompt, in [`routine-prompt.md`](routine-prompt.md) |
| Memory | A Google Sheet with five tabs, described in [`tracker.md`](tracker.md) |
| Hands | Gmail, Google Drive and Google Sheets connectors, plus a shell for reading job feeds |
| Guard rails | A network allowlist, drafts-only output, and a human approval step before anything is sent |

The agent has no memory between runs. Everything it needs to know about the past is in the sheet: which roles it has already added, which company boards work, what was sent and when.

## Design decisions

**Drafts only, with a human approval step.** The agent can read the web, my inbox and my files, which is exactly the combination where a malicious page could try to misuse it. So it cannot send or submit. It prepares, I approve a batch, and then it goes out.

**The sheet is the interface.** Two tabs are for me to edit: `Answers` (salary, notice period, why I am leaving) and `Rules` (titles, experience range, locations, caps). Changing the search is a cell edit, not a prompt rewrite. The `Rules` tab is limited to search criteria, so a row there cannot make the agent take an action.

**It keeps a list of what works.** Company job-board addresses are guessable but not known. The agent records every board it tries in a `Boards` tab, so each run starts from the ones that resolved and spends its effort on new companies.

**It does not pad.** A cap of 100 roles a day is a ceiling, not a target. Adding roles I am not eligible for would cost me review time and add no chance of an interview.

**No LinkedIn automation.** LinkedIn's terms ban it and accounts get restricted. Job alerts by email give the same roles without the risk.

## What the first day showed

| Run | What changed | Result |
|---|---|---|
| 1 and 2 | First version | Found candidates by search but could not open any listing. The cloud environment's network allowlist had no job sites on it. |
| 3 | Job sites allowed | Listings opened, but it checked a handful of boards in one minute and stopped. Zero roles. |
| 4 | Rewrote the search: named company list, direct feeds, a minimum number of boards per run | 270 boards tested, 83 working. 512 product manager postings read. Zero passed. |
| 5 | Added India-based roles and Ashby boards | 11 eligible roles and 6 outreach targets. |

Run 4 was the useful failure. The pipeline worked and the answer was still zero, which told me the constraint was the market and not the tool: remote product roles at foreign companies that hire from India at my level are close to an empty set on public job boards. Widening the location rule fixed in one change what no amount of extra searching would have.

Other things I learned:

- **A vague instruction gets a shallow run.** "Find roles" produced a one-minute effort. "Test at least 40 new companies and record each one" produced ten minutes of real work.
- **Read feeds, not pages.** Fetching listing pages returned truncated text. Reading each board's JSON feed with a short script was faster and complete.
- **Have the agent report what it rejected.** The run log records postings seen against postings passed. Without that I would not have known whether zero meant "nothing exists" or "it did not look".

## What is not here

My personal details, salary figures and resume are removed from the prompt and replaced with placeholders. The tracker itself is private.

## Status

Running daily since 26 November 2026. I will add reply and interview rates here once there is enough data to mean something.
