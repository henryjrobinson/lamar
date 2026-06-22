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

## Hand-off to Ralph (the `prd.json` shape)
When the gates are green and the work decomposes into verifiable steps, emit a
`prd.json` for the [snarktank/ralph](https://github.com/snarktank/ralph) autonomous
runner (or run `/prd` then `/ralph`). Use Ralph's **real** `prd.json` schema: each
iteration `ralph.sh` picks the highest-priority story with `passes:false`, builds it,
runs the project's quality checks, commits, then flips `passes` to `true`; it stops
when every story passes.

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

What makes it autonomy-ready (not just valid JSON):
- **One story per acceptance criterion** — each independently verifiable and
  completable in a single Ralph iteration (one context window). If you can't
  describe the change in 2-3 sentences, split it.
- **`acceptanceCriteria` carry the concrete pass/fail signal** (your package's
  Test approach) — never "make it work". Always end every story with **the
  repo's actual quality gate**, named concretely. Use `"Typecheck passes"` only
  where the repo *has* a typecheck (`npx tsc --noEmit`); otherwise name the real
  command it runs (`"Tests pass: run \`npm test\`"`, a lint, etc.). Add
  `"Tests pass"` for testable logic and `"Verify in browser using dev-browser
  skill"` for UI stories. **Don't hardcode a TypeScript check into a non-TS repo** —
  match the gate to the project's stack.
- **Exact file paths** in each `description`; never "the relevant file".
- **Dependency order** by `priority` — schema/migrations → backend/logic → UI →
  aggregate views; no story depends on a later one.
- **`branchName`** isolates the run: `ralph/<slug>`. `passes` starts `false`
  (Ralph flips it); `notes` starts `""`.
- The **repo-wide quality gate is NOT a `prd.json` field** — Ralph runs whatever
  the project's `CLAUDE.md` / `scripts/ralph/` defines after each story. State it
  there (or in `.lamar.md`), not in the JSON.
- Honor the repo's security rules (e.g. parameterize all queries — no
  string-concatenated user input) inside the story descriptions.

## Ralph vs. interactive build
- **Ralph (or parallel agents):** gates green AND the work decomposes into a
  `prd.json` of independently verifiable steps. Typical: a well-specified
  Tier 2 epic with tests, or a clean Tier 1.
- **Interactive build:** gates red (fuzzy/novel/exploratory) or judgment-heavy.
- Tier 0 never hands off — just do it.

### Partial hand-off (mixed-readiness build orders)
The choice above is NOT all-or-nothing. A single wish often decomposes into some
gates-green stories and some gates-red (exploratory) ones — e.g. a clean core loop
plus one "parse messy PDFs / novel integration" step that can't be machine-checked
yet. Do not collapse that to a single verdict.

- Emit a `prd.json` covering the **green prefix** — the dependency-ordered,
  independently verifiable stories — and hand THAT to ralph.
- Carry the red tail as a **Flagged** ledger item to build interactively next.
- State the split explicitly so the user knows what ralph will and won't touch,
  and make the green prefix not depend on the held-back red stories.

The failure modes this avoids: (a) refusing to hand off at all because one story is
red, and (b) cramming the exploratory step into the prd.json to make it "complete"
— which is how an autonomous runner produces confident garbage.

## Execution target
When the gates are green, emit the package, then offer to run it on the repo's
**configured execution target**, resolved from `.lamar.md` `execution_target`
(schema below).

- **Default — `ralph` (local):** convert the package to Ralph's `prd.json` and
  offer to run it (`/ralph`) in this session. This is the public path: any user
  with Ralph can take a Lamar package to shipped code, no special infra.
- **A configured remote target:** when `execution_target` names a `command`
  (a self-hosted orchestrator, a cloud agent, etc.), offer to hand the package
  to that command instead of — or in addition to — local Ralph. The command
  receives the emitted package / `prd.json`.

Keep this skill free of any specific target's URL, token, or payload: those
live in the repo's (private) `.lamar.md`, never here. The skill only needs to
know *that* a target is configured and *how* to invoke it — not what it is.

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
doc_home: docs/      # where PRDs/handoffs live
execution_target: ralph  # where autonomy-ready packages run.
                         #   ralph (default) → local Ralph: package → prd.json → /ralph
                         #   or an object: { name: <label>, command: <cmd> }
                         #     command receives the package / prd.json. Used for a
                         #     remote orchestrator or cloud agent. Keep any
                         #     URL/token in the command's own config, not here.
---
```

Load it in SKILL.md §1; apply `floor` in §2; apply the rest in §5–§6.
Keep the skill itself free of any specific repo's values — they live here.
