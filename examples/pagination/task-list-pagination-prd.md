# PRD: page through the task list

## Problem
The task list page loads every task at once. The list has grown, so the page is slow.

## What you want
Show the tasks one page at a time, newest first, so the page loads fast again.

## Not in this work
- No endless scrolling. You move between pages with Prev and Next.
- No change to how a task row looks. Only the page controls are new.

## How we know it is done
- **AC-1** — Asking for page 2 gives tasks 26 to 50, newest first. Each page holds 25.
- **AC-2** — The page knows how many pages exist. We ask the database how many tasks
  there are before we page, so the last page is correct and not a guess.
- **AC-3** — The list address accepts a page number. A missing or broken page number
  shows page 1.
- **AC-4** — The list shows "Page X of Y" with Prev and Next. Prev is off on page 1.
  Next is off on the last page.
- **AC-5** — Changing page shows the new tasks without reloading the whole site.
- **AC-6** — Nothing a user types goes straight into a database command. This blocks a
  common attack where typed text is run as a command.

## How each one is checked
- AC-1, AC-2, AC-3, AC-6: automated tests, run with `npm test`. All must pass.
- AC-4, AC-5: a browser check of the task list. The plan says what to look for.
- Every task also runs the type check (`npx tsc --noEmit`). It catches code that does
  not fit together before anything runs.

## Data and risk
- Data: no personal or customer data is involved.
- Risk: limited to the task list page. To undo it, revert the commits on the branch.

## Decisions made for you (check these)
- **Decided:** 25 tasks per page.
- **Flagged for your eyes:** "newest first" uses the order tasks were created in. An
  edited old task does not move to the top.
