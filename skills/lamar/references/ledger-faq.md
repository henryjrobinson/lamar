# Return-trip ledger — FAQ

A quick orientation for anyone meeting the return-trip ledger for the first
time. For a worked example, see `references/ledger-example.md`; for the policy
it reports against, see `references/gates.md`.

**What is the return-trip ledger?**
It's the short, honest report an unattended run hands back when you return. It
lists every judgement call the run had to make on your behalf, sorted into
three buckets so you can trust walking away.

**Why do I need it?**
When you hand off a wish and leave, the run inevitably hits choices the spec
didn't pin down. The ledger means you come back to a tidy list of exactly what
was assumed, what to double-check, and what still needs you — not a wall of
diff to reverse-engineer.

**What are the three buckets?**
- **✅ Decided** — low-stakes calls a sensible default covered. Logged as FYI;
  nothing for you to do.
- **⚠️ Flagged** — consequential but reversible calls. Done, but worth a glance
  to confirm.
- **⛔ Blocked** — irreversible, security, cost, or customer-data forks. The run
  refused to guess and left these for you to decide.

**Who decides which bucket a call goes in?**
The loose-end policy in `references/gates.md`. The ledger is just the run's
report of how that policy sorted each call.

**What should I do with it?**
Skim Decided, sanity-check Flagged, and act on Blocked. The Blocked items are
the ones that actually need your eyes before the work is truly finished.

---
See also `references/ledger-example.md` (a full worked ledger) and
`references/gates.md` (the policy this ledger reports against).
