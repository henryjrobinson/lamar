# The autonomy-ready package + repo profile

## Package fields (scale depth to tier)
The portable output Lamar emits when the gates are green:
- **Problem** — what we're solving and why it exists.
- **Intent** — what the user actually wants (not just the literal words).
- **Non-goals** — explicitly out of scope.
- **Acceptance criteria** — machine-checkable where possible; each maps to a test.
- **Data classification** — personal / customer / none (from the profile).
- **Blast radius / risk** — and the rollback path.
- **Test approach** — name the *concrete verification step*, not just a
  category. Say HOW each acceptance criterion is checked and the actual
  signal that says pass/fail before hand-off: the command to run (the test
  file / suite / lint / type-check), or — for UI/UX, visual, exploratory
  work — the manual/browser-driven check and what "pass" looks like.
  An autonomy-ready package must state this even when the criteria are
  already testable: the runner (and a user who can't read the code) needs
  to know what gets executed to confirm the work is done, not just what
  "done" means. "The criteria are testable" is not a test approach; "run
  `<suite>`; all green" is.
- **Open assumptions + Decisions ledger** — carried from `gates.md`.

## Render the package in the user's register
The package is for the user as much as the runner — so it inherits the Voice
and teach dial from the intake; it does not reset to engineer-speak. **The teach
dial does not switch off when you emit the package.** This is where it most often
fails: the early tradeoffs get explained in plain language, then the final package
dumps the most technical decisions — the ones the user can *least* validate —
as bare jargon. If the user is non-technical (or teach is ON), every package field
must stay in their register: a `COUNT` query is "we ask the database how many rows
match before paging, so the page count is right"; "ORDER BY id descending" is
"newest first"; a "regression suite" is "the existing checks that confirm we didn't
break anything else." State the term if useful, but always with the plain-language
gloss attached — never a term the user can't weigh. The acceptance criteria and
test approach are the most consequential decisions in the package, so they get the
*most* plain-language justification, not the least. A package a non-technical user
cannot read back and validate has failed, regardless of how correct it is.

## Hand-off: the PRD and the implementation plan (default)
When the gates are green and the work decomposes into verifiable steps, write two
markdown files into the profile's `doc_home` (default `docs/`). The user gives both
to a coding agent **one time**. Name no specific agent or harness in either file —
they must work in any of them.

**Scale to the tier.** Tier 1: a short PRD (a line or two per field) and a plan with
few tasks. Tier 2: the full treatment. At any tier the plan keeps its rules, and every
task keeps its `Covers`, `Files`, `Change`, and `Check` lines.

**What this promises, and what it does not.** One hand-off, and a check for every
task, so the agent can tell whether it finished. It does NOT promise correct code on
the first pass with no review. Say so if the user asks.

### 1. `<slug>-prd.md` — for the user
The package fields above, in the user's register (it goes through the plain-language
pass). Number the acceptance criteria `AC-1`, `AC-2`, … so the plan can point at them.

### 2. `<slug>-plan.md` — for the coding agent
Technical; it does not go through the plain-language pass. Use this shape:

```markdown
# Implementation plan: <feature>

PRD: <path to the PRD>

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
- Stack: <language / framework, as found in the repo>
- Quality gate: `<the repo's real command>`
- Standards: <CLAUDE.md or the profile's standards>
- Branch and commits: <the repo's rule; commit after each ticked task if it allows.
  If the project is not a git repository, say so and say how to undo the work>

## Tasks
- [ ] **T1 — <one deliverable>**
  - Covers: AC-1
  - Files: `<exact/path.ext>`
  - Change: <two or three sentences: what must be true after this task>
  - Check: `<command>` → <what a pass looks like>

## Held back (not in this plan)
- <exploratory or unready work, and why>

## Report (the agent fills this in)
- Passed: <task ids>
- Stopped: <task id, the check that failed, the output>
- Deviations: <what differed from the plan, and what you did>
- Decisions made / needs your eyes: <choices you made that the plan did not cover>
```

What makes the plan ready to hand off (not just well-formatted):
- **Every acceptance criterion maps to at least one task, and every task names the
  criterion it covers.** Check both directions before you emit. Long plans drift from
  the PRD; a task with no `Covers:` line, or a criterion no task covers, is a defect.
- **One task per independently verifiable step.** If you can't describe the change in
  two or three sentences, split it.
- **Every task has a concrete check and says what a pass looks like** — never "make it
  work". Use the repo's real commands. Use a typecheck only where the repo has one.
  For UI tasks, name the browser or manual check and the visible result.
- **Exact file paths**, taken from reading the repo — never "the relevant file".
- **Dependency order** — schema/migrations → backend/logic → UI → aggregate views; no
  task depends on a later one.
- **State the result and the check, not the code.** Do not write the implementation
  line by line. Every extra line of prescribed code is a guess about the codebase,
  and an agent follows a wrong guess confidently.
- **The checkboxes are the resume point.** A new session reads them and continues from
  the first unticked task, so the plan must make sense without this conversation.
- **Rules 4 and 5 are not optional.** One agent with no outside loop has nothing else
  stopping a false "done". Never drop or soften them.
- Honor the repo's security rules (e.g. parameterize all queries — no
  string-concatenated user input) inside the task descriptions.

### When the agent stops with checks still failing
The user sees which tasks passed, which check failed, and where the ticks end. They
bring the failed tasks back to Lamar (the picture has changed — re-run the gates) or
fix them with the agent directly.

## Hand-off vs. interactive build
- **Hand-off:** gates green AND the work decomposes into independently verifiable
  tasks. Typical: a well-specified Tier 2 epic with tests, or a clean Tier 1.
- **Interactive build:** gates red (fuzzy/novel/exploratory) or judgment-heavy.
- Tier 0 never hands off — just do it.

### Partial hand-off (mixed-readiness build orders)
The choice above is NOT all-or-nothing. A single wish often decomposes into some
gates-green tasks and some gates-red (exploratory) ones — e.g. a clean core loop
plus one "parse messy PDFs / novel integration" step that can't be machine-checked
yet. Do not collapse that to a single verdict.

- Write the plan for the **green prefix** — the dependency-ordered, independently
  verifiable tasks — and hand THAT off.
- List the red tail under **Held back** in the plan, and carry it as a **Flagged**
  ledger item to build interactively next.
- State the split explicitly so the user knows what the agent will and won't touch,
  and make the green prefix not depend on the held-back work.

The failure modes this avoids: (a) refusing to hand off at all because one task is
red, and (b) cramming the exploratory step into the plan to make it "complete"
— which is how an autonomous runner produces confident garbage.

## Execution target
When the gates are green, emit the package, then offer to run it on the repo's
**configured execution target**, resolved from `.lamar.md` `execution_target`
(schema below).

- **Default — `plan`:** write the PRD and the plan. Offer to start the plan in this
  session, or leave both files for the user to give to another agent. No extra
  install, no special infra.
- **Optional — `ralph`:** emit a Ralph `prd.json` instead (next section). Use it ONLY
  when the profile sets `execution_target: ralph`. Still write the PRD for the user.
- **A configured remote target:** when `execution_target` names a `command`
  (a self-hosted orchestrator, a cloud agent, etc.), offer to hand the package
  to that command. The command receives the PRD and plan (or the `prd.json`).

Keep this skill free of any specific target's URL, token, or payload: those
live in the repo's (private) `.lamar.md`, never here. The skill only needs to
know *that* a target is configured and *how* to invoke it — not what it is.

## Optional target: Ralph (the `prd.json` shape)
For repos that set `execution_target: ralph`. Write a `prd.json` at the repo root for
the [snarktank/ralph](https://github.com/snarktank/ralph) autonomous runner; the user
copies it into their Ralph folder. Use Ralph's **real** `prd.json` schema: each iteration
`ralph.sh` picks the highest-priority story with `passes:false`, builds it, runs the
project's quality checks, commits, then flips `passes` to `true`; it stops when every
story passes.

```json
{
  "project": "<name>",
  "branchName": "ralph/<feature-slug>",
  "description": "<problem + intent + non-goals + data class — the package's framing>",
  "userStories": [
    {
      "id": "US-001",
      "title": "<one acceptance criterion, as a deliverable>",
      "description": "As a <user>, I want <x> so that <y>. Touch exactly: <exact file paths>.",
      "acceptanceCriteria": [
        "<concrete, checkable deliverable — never 'works correctly'>",
        "<the repo's quality gate, e.g. `npx tsc --noEmit`, `npm test`, or a lint>"
      ],
      "priority": 1,
      "passes": false,
      "notes": ""
    }
  ]
}
```

The readiness rules for the plan apply to the stories too (one story per verifiable
step, exact paths, dependency order by `priority`, concrete checks). Ralph-specific:
- Each story must be completable in a single Ralph iteration (one context window).
- Always end every story's `acceptanceCriteria` with **the repo's actual quality
  gate**, named concretely. Use `"Typecheck passes"` only where the repo *has* a
  typecheck; otherwise name the real command (`"Tests pass: run \`npm test\`"`, a
  lint, etc.). Add `"Verify in browser using dev-browser skill"` for UI stories.
  **Don't hardcode a TypeScript check into a non-TS repo.**
- **`branchName`** isolates the run: `ralph/<slug>`. `passes` starts `false`
  (Ralph flips it); `notes` starts `""`.
- The **repo-wide quality gate is NOT a `prd.json` field** — Ralph runs whatever
  the project's `CLAUDE.md` / `scripts/ralph/` defines after each story. State it
  there (or in `.lamar.md`), not in the JSON.
- For a partial hand-off, the `prd.json` covers the green prefix only.

## Repo profile: `.lamar.md`
If present in the repo root, it overrides defaults. Schema (all optional):

```yaml
---
floor: 0            # minimum tier (0|1|2)
org: solo           # solo | team  → controls written-ness
data_class: none    # none | personal | customer
standards:          # skills/docs to apply
  - CLAUDE.md
tests: optional     # optional | required-for-logic | required-all
security_review: tier2   # never | tier2 | any-data-path
secrets: <how secrets are resolved, generically>
doc_home: docs/      # where the PRD and plan are written
execution_target: plan   # where autonomy-ready packages run.
                         #   plan (default) → PRD + implementation plan for any coding agent
                         #   ralph → local Ralph: package → prd.json → /ralph
                         #   or an object: { name: <label>, command: <cmd> }
                         #     command receives the PRD and plan (or prd.json). Used for a
                         #     remote orchestrator or cloud agent. Keep any
                         #     URL/token in the command's own config, not here.
---
```

Load it in SKILL.md §1; apply `floor` in §2; apply the rest in §5–§6.
Keep the skill itself free of any specific repo's values — they live here.
