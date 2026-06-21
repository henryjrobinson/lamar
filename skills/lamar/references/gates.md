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
