# Lamar — observations from real runs (candidate doc gaps / bugs)

Field notes from using Lamar on real wishes. Each entry: what happened, whether
it's a genuine bug, and the proposed fix. Triage these into the references when
confirmed.

---

## 2026-06-22 — Mixed-readiness build order has no documented hand-off pattern

**Repo:** corruption-o-bot (greenfield OSS political-trade tracker → control-plane signal).

**What happened.** Intake produced a sound v1 spec whose build order had **mixed
autonomy-readiness across steps**: the core loop (scaffold, adapter interfaces,
mock data adapter, diff/seen-state, page, webhook sink, control-plane proposal
sink, tests) was gates-green and cleanly decomposable into a ralph `prd.json`; but
one step — parsing the free House Clerk PTR **PDFs** (messy, hand-typed, ~86%
parse reliability) — was gates-red (Gate 1 ambiguous, Gate 2 not machine-checkable),
i.e. genuinely exploratory and correctly an *interactive* build, not a ralph story.

**Is it a bug?** Not in Lamar's *judgment* — the gates correctly isolated the
exploratory step, and holding it for interactive build is the right call. The gap
is in the **references**: `package.md` ("Ralph vs. interactive build") frames the
choice as **per-package binary** ("gates green AND the work decomposes → ralph;
gates red → interactive"). It gives no guidance for the common real case where a
single wish is *partly* autonomy-ready. An agent following the docs literally could:
  (a) refuse to hand off at all because one story is red, or
  (b) cram the exploratory step into the prd.json to make it "complete."
Both are wrong; the right move is **emit a prd.json for the green prefix, hold the
red tail for interactive, and say so in the ledger.**

**Proposed fix.** Add a short section to `package.md` (and a line in `gates.md`):
> **Partial hand-off (mixed-readiness build orders).** When a wish decomposes into
> some gates-green stories and some gates-red (exploratory) ones, do NOT treat it as
> all-or-nothing. Emit a `prd.json` covering the **green prefix** (dependency-ordered,
> independently verifiable), and carry the red tail as a **Flagged** ledger item to be
> built interactively next. Note the split explicitly so the user knows what ralph will
> and won't touch. The green prefix should not depend on the held-back red stories.

**Status:** PATCHED 2026-06-22. Added `package.md` §"Partial hand-off (mixed-readiness
build orders)" and a cross-reference in `gates.md` ("Gates apply per story, not just per
wish"). Confirmed by Henry.
