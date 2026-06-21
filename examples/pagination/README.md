# Worked example — pagination

A complete trip through the public flow: a **fuzzy wish** → Lamar's refinement →
the **autonomy-ready `prd.json`** Lamar emits → handed to snarktank/ralph to build.

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

## 3. What Lamar emits — `prd.json` (snarktank/ralph schema)

See [`prd.json`](./prd.json) in this folder. Note the shape: one story per
independently verifiable step, dependency-ordered by `priority`, each
`acceptanceCriteria` ending in a concrete pass/fail signal (`Typecheck passes`,
`Tests pass`, `Verify in browser…`).

## 4. Build it (the hand-off)

```bash
# one-time: install the runner (see repo README "Run it")
git clone https://github.com/snarktank/ralph
cp examples/pagination/prd.json ralph/prd.json
cd ralph && ./ralph.sh --tool claude
```

Ralph picks the highest-priority unfinished story, builds it, runs the quality
checks, commits, flips `passes: true`, and moves to the next — stopping when every
story passes.
