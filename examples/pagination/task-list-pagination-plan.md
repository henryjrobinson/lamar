# Implementation plan: task list pagination

PRD: `examples/pagination/task-list-pagination-prd.md`

## Rules for the agent
1. Read the PRD before you start. It is the source of truth for what "done" means.
2. Do the tasks in order, one at a time.
3. After each task, run its check. Tick the box only after the check passes.
4. If a check fails, fix the work and run the check again. If it still fails after
   three attempts, STOP. Do not tick the box. Write what failed under "Report".
5. If the plan and the codebase disagree (a file is missing, a function differs),
   do not guess. If the fix is small and no acceptance criterion changes, adapt and
   log it under "Deviations". If an acceptance criterion would change, STOP and report.
6. Do not build anything listed under "Held back".
7. If you can give each task to a sub-agent with a fresh context, do that.
8. When every box is ticked, run the full quality gate, then fill in the "Report".

## Context
- Stack: TypeScript, Node server, React UI.
- Quality gate: `npm test && npx tsc --noEmit`
- Standards: `CLAUDE.md`. Parameterize every query input. No string-built SQL.
- Branch and commits: work on `lamar/task-list-pagination`. Commit after each ticked task.

## Tasks
- [ ] **T1 — Paginated query for tasks, newest first**
  - Covers: AC-1, AC-2, AC-6
  - Files: `src/server/tasks/queries.ts`, `src/server/tasks/queries.test.ts`
  - Change: Add `getTasksPage(page, pageSize = 25)`, ordered by id descending, and
    `getTaskCount()`. All inputs are query parameters.
  - Check: `npm test -- queries && npx tsc --noEmit` → a new test shows
    `getTasksPage(2)` returns rows 26–50 in descending id order, `getTaskCount()`
    returns the row total, and the existing tests still pass.

- [ ] **T2 — Serve one page from the list endpoint**
  - Covers: AC-2, AC-3
  - Files: `src/server/tasks/list-handler.ts`, `src/server/tasks/list-handler.test.ts`
  - Change: Read `?page` (default 1). Return `{ tasks, page, pageCount }`. A missing,
    non-numeric, zero, or negative page becomes 1.
  - Check: `npm test -- list-handler && npx tsc --noEmit` → new tests show
    `GET /tasks?page=2` returns the second page and `pageCount`, and
    `GET /tasks?page=abc` returns page 1.

- [ ] **T3 — Pager control in the task list UI**
  - Covers: AC-4, AC-5
  - Files: `src/components/TaskList.tsx`
  - Change: Fetch the current page. Show "Page X of Y" with Prev and Next. Disable
    Prev on page 1 and Next on the last page. Changing page re-fetches the list only.
  - Check: `npx tsc --noEmit` passes. Then open the task list in a browser: the label
    reads "Page 1 of Y", Prev is disabled, Next shows tasks 26–50 with no full reload,
    and Next is disabled on the last page.

## Held back (not in this plan)
- None. Every part of this wish passed the gates.

## Report (the agent fills this in)
- Passed:
- Stopped:
- Deviations:
- Decisions made / needs your eyes:
