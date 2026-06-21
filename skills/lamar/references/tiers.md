# Sizing: how much Lamar?

Sort every wish into one tier by **job complexity × blast radius**. Same
skeleton each time — more or fewer gates turned on. When complexity and
blast radius disagree, the higher one wins (a one-line change to payment
code is Tier 2).

## Tier 0 — Just Do It
Typo, copy tweak, config flip, obvious one-liner, low risk. No spec, no
gates, terse confirmation. This is the "just do it" path — preserve it; do
not bureaucratize trivial work.

## Tier 1 — Standard
A bounded feature or a real bugfix. Light refinement: a few-line spec
(problem · intent · acceptance criteria), a test approach, build, diff
review, handoff note. Hand off to a runner if the gates go green; otherwise
build interactively.

## Tier 2 — Major
New subsystem, schema/data-model change, anything touching customer data,
anything hard to reverse. Full treatment: complete spec, machine-checkable
acceptance criteria + tests, explicit security/data-classification gate,
plan, review, docs. Designed to emit a fully autonomy-ready package.

## The four dials (the process flexes, it never forks)
1. **Job complexity** → sets the base tier (above).
2. **Org complexity** → turns up *written-ness*. Solo: a Tier 1 spec can stay
   terse / partly in-head. With teammates: the same Tier 1 must be fully
   written so someone can pick it up cold. (Read from the profile's `org` field.)
3. **Repo idiosyncrasy** → sets the *floor* and the rules (from `.lamar.md`).
4. **Autonomy-readiness** → orthogonal; the gates in `references/gates.md`.

## How to detect the tier from a wish
- Look for blast-radius words: "schema", "migration", "payment", "auth",
  "customer data", "delete", "production" → lean Tier 2.
- Look for boundedness: a single page/component/function, reversible → Tier 1.
- Look for triviality: rename, copy, config, format, a one-liner → Tier 0.
- If unsure between two tiers, state both and pick the higher; let the user
  bump it down.

## Worked examples
- "rename a button label" → Tier 0 (trivial, reversible).
- "add CSV export to the reports page" → Tier 1 (bounded feature, low blast radius).
- "add a billing subsystem that charges customer cards" → Tier 2 (new subsystem + money + customer data).
