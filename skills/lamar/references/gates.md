# Autonomy gates + loose-end policy

## The three gates
A wish is autonomy-ready only when all three are green:
1. **Spec unambiguous** — no open question an agent would have to ask mid-flight.
2. **Success machine-checkable** — acceptance criteria backed by tests/evals
   that say pass/fail without the user looking. (If you want to hand it off,
   write the test first.)
3. **Blast radius bounded** — reversible, or guarded by the loose-end policy below.

Green at any tier → offer the hand-off. Red → stay in the run-up / interactive
build until they go green. Always say WHICH gate is red and what would turn it green.

Gates apply **per story, not just per wish.** When a build order mixes green and
red stories, hand off the green prefix and hold the red tail interactively — see
`package.md` §"Partial hand-off (mixed-readiness build orders)".

## The loose-end policy
Apply to every unknown — during refinement AND during build. **The bar for
"block and ask" is blast radius, not uncertainty.**

- **Low-stakes** (naming, formatting, a reasonable default exists)
  → DECIDE it, LOG it ("Assumed X."). Never blocks.
- **Consequential but reversible**
  → DECIDE it, FLAG it loudly in the ledger for override.
- **Irreversible / customer-data / security / cost**
  → STOP and ask. The only things allowed to block.

Uncertain-but-safe → proceed and log. That single rule converts the chat loop
into walk-away autonomy.

## The ledger
Maintain and deliver, at the end of any unattended run, a section titled:

> ## Decisions Made / Needs Your Eyes
> **Decided (low-stakes):** … (assumptions logged)
> **Flagged (reversible, please confirm):** …
> **Blocked (waiting on you):** …

This ledger is also the teaching feed and, with a team, the audit trail.

## Before you emit — two hard checks
Run these the moment before you output the package (SKILL.md §6). They are gates,
not advice: failing one means re-work the package, not ship it.

1. **Register check (non-technical / teach ON).** The package is just the last
   *consequential* message, so it goes through the same **plain-language pass** as every
   intake message (`intake.md` §"The plain-language pass"): dispatch the fresh-context
   subagent on the *drafted user-facing* package, and send what it returns. It strips
   any token the user cannot weigh — SQL (`COUNT(*)`, `ORDER BY`), API params
   (`limit` / `offset`), state / variable / function names (`storiesTotalCount`,
   `stories.map`), bare stack nouns — replacing each with its plain gloss
   (`package.md` §"Render the package in the user's register") or cutting it. The split:
   the technical `prd.json` handed to the runner stays technical and does NOT go through
   the pass; only the summary shown to the *user* does. A non-technical user who cannot
   read the package back and validate it = failed gate, however correct the spec is.

2. **Stress-test flag floor.** If the posture is Stress-test on a non-trivial spec,
   "zero flags / fully locked" is itself the failure (`intake.md`). Before emitting,
   walk the unnamed-input classes the spec left implicit — empty / zero / negative /
   NaN / non-integer / out-of-range / wrong-type — and for EACH either decide-and-log
   a trivial default or *flag* a consequential one in the ledger. A clean verdict with
   an empty "Flagged" list on a non-trivial spec means you rubber-stamped instead of
   probing — go back and probe before you emit.
