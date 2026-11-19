# Worked example — pagination

A complete trip through the public flow: a **fuzzy wish** → Lamar's refinement →
the **PRD and implementation plan** Lamar writes → handed to a coding agent to build.

The file paths in this example are illustrative. There is no task-list app in this repo.

## 1. The fuzzy wish (what the user typed)

> "The task list page loads everything at once and it's getting slow. Can we make it
> page through results instead? Newest first."

## 2. What Lamar does (intake, abridged)

Lamar sizes this as Tier 1–2 (real logic, bounded blast radius), then refines until
the wish is unambiguous — surfacing the decisions a builder would otherwise guess:

- **Page size?** → 25 per page (decided; flagged for your eyes).
- **Newest first** → `ORDER BY id DESC`.
- **How does the UI know the total page count?** → a `COUNT` query before paging, so
  the last page isn't a guess.
- **Acceptance check?** → existing list tests stay green; a new test asserts page 2
  returns rows 26–50; typecheck passes.

It carries those choices in the return-trip ledger (Decided / Flagged / Blocked) so
you can trust walking away.

## 3. What Lamar writes

- [`task-list-pagination-prd.md`](./task-list-pagination-prd.md) — for you. It states
  the problem, what is out of scope, and numbered acceptance criteria in plain language.
- [`task-list-pagination-plan.md`](./task-list-pagination-plan.md) — for the coding
  agent. Note the shape: one task per independently verifiable step, in dependency
  order. Each task names the criteria it covers, the exact files, and the check that
  proves it is done. The rules at the top tell the agent to stop and report when a
  check keeps failing.

## 4. Build it (the hand-off)

Give both files to your coding agent with one message:

```
Read examples/pagination/task-list-pagination-prd.md, then carry out
examples/pagination/task-list-pagination-plan.md. Follow the rules at the top of the plan.
```

The agent builds one task, runs its check, ticks the box, and moves to the next. It
stops when every box is ticked or when a check keeps failing.

## Optional: the Ralph path

A repo that sets `execution_target: ralph` in `.lamar.md` gets a
[`prd.json`](./prd.json) instead of the plan (snarktank/ralph schema): one story per
verifiable step, ordered by `priority`.

```bash
# one-time: install the runner
git clone https://github.com/snarktank/ralph
cp examples/pagination/prd.json ralph/prd.json
cd ralph && ./ralph.sh --tool claude
```

Ralph picks the highest-priority unfinished story, builds it, runs the quality
checks, commits, flips `passes: true`, and moves to the next — stopping when every
story passes.
