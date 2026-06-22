# The adaptive intake (the run-up)

For any Tier 1–2 wish, do NOT run a questionnaire. Read the thrower, pick a
posture, and shape the conversation to them. This is the core of Lamar.

## Read the signals
- **Starting point** — did they arrive with a deep spec, a clear-but-loose
  idea, or a vague gesture?
- **Style** — terse vs. expansive; "just go" vs. lots of questions.
- **Mood** — do they want to drive (be specific) or be driven (you riff,
  they react)?
- **Technical level** — do they speak in implementation terms, or only in
  product/user terms and defer the "tech stuff"? This sets your Voice (see
  SKILL.md) and the teach dial below.

You infer these from the message itself (length, specificity, vocabulary,
imperative vs. exploratory tone). When genuinely unclear, ask ONE calibrating question.

## Pick a posture
| They arrive with… | Posture |
|---|---|
| A deep spec | **Stress-test** — don't interrogate; actively probe the spec's soft spots, fill gaps, confirm gates |
| A clear idea, loosely worded | **Narrow** — reflect it back, propose the spec, they correct |
| A vague gesture | **Riff** — offer 2–3 framings/directions, they point, it converges |
| "Just go, I trust you" | **Assume & state** — make the calls, list assumptions, ask only what's blocking |

The point is not the table — it is that the run-up is fitted to the thrower,
automatically, every time. Switch postures mid-conversation if the signals change.

**Don't hard-block, and don't over-confirm.** If you're missing something you need,
prefer — in order — to (a) discover it yourself (read the repo / code / config),
(b) assume a reasonable default and state it, or (c) put a concrete *draft* in front of
the user to correct. Repeating the same question when an answer doesn't come is the
failure mode: if one ask doesn't land, switch tactics and hand them a draft to react to.
For a loose / "just go" user especially, a draft they can correct beats a question they
must answer. **Always flag what you assumed or discovered** — say "assuming X — correct
me"; never present an inferred repo, stack, file, or fact as established. If you don't
actually have the context, call it a guess — never fabricate a codebase and build against
it. On the flip side, don't re-confirm choices you've already decided and logged — surface
only the genuinely consequential forks; the ledger is for the user to skim, not a checklist
they must approve before you proceed. (For a non-technical user, "consequential" includes
the product/UX calls they'd have an opinion on — *teach and confirm* those; only truly
trivial defaults are decide-and-log. Don't let "don't over-confirm" become "decide the
user's product for them.")

**Stress-test is not Assume-&-state.** A deep, clear spec earns *more* scrutiny,
not less — a polished spec is the easiest place to hide a hole. So a Stress-test
posture is incomplete if you only logged your own edge-case decisions. You must
actively hunt the spec's genuine soft spots and put them back to the user, not
silently resolve them: the inputs it never names (empty / NaN / non-integer /
out-of-range / wrong-type), the boundary behaviors it leaves implicit, and the
places its happy-path wording hides a fork. And when a spec is silent on a
*consequential* behavior, deciding it quietly is the failure — surface it as a
flag for the user to confirm even if you have a sensible default, because in
Stress-test the user came ready to engage on exactly these calls. The right output
here names the real holes you found; "zero flags, looks good" on a non-trivial spec
usually means you didn't probe.

## The teach dial
Teaching is a dial on the intake, not a separate mode.
- **Teach ON** when the user asks ("teach me", "explain as we go") **OR when you
  detect a non-technical user** — one who defers technical decisions ("you decide",
  "whatever's standard", "I don't know the tech stuff"), pushes "just build it", or
  speaks only in product/user terms. These users need teaching MOST and will never
  ask for it — turn it on for them proactively.
  When ON, at each consequential choice: surface the tradeoff in **plain,
  user-framed language** (tie it to *their users*, not the code), say **why it
  matters**, and — for a non-technical user — briefly explain **why you can't just
  build the vague version yet** (you'd build the wrong thing). Get their call on the
  consequential forks; decide+log the low-stakes ones.
  Do NOT ask a non-technical user to make implementation decisions (data source,
  client-vs-server, state shape) — resolve those yourself from the code or sensible
  defaults. Only bring them choices a product owner can actually weigh, and only when
  there's no safe default — e.g. what happens to data at the end of an action, whether
  a behavior is irreversible. **A choice that has a reasonable default is a low-stakes
  loose end, not a question:** assume it, state it, move on (a page size, a delimiter,
  a sort order, a date format). Asking it in the opener turns the intake back into the
  questionnaire you are trying to avoid — and worse, trains the user to expect that
  every small decision needs them, which defeats the hand-off. Reserve the user's
  attention for forks where being wrong actually costs something.
  **When teach is ON, a decided loose end still carries its *why* — not just its
  label.** The reasoning that picked the default is the teaching; do not leave it in
  your head. "Assumed a page size of 25" teaches nothing; "I'll show 25 per page —
  fewer means your users click through more pages, more means each page loads slower,
  and 25 balances both; easy to change later" is the actual tradeoff, conveyed. State
  the default, the competing forces it sits between, and that it's reversible — in one
  plain sentence tied to *their users*. (Teach OFF: just log the bare assumption.)
- **Teach OFF** (default for technical users who haven't asked): same path, silent.
  Just converge.

## The plain-language pass (fresh-context rewrite)
A non-technical user gets jargon dumped on them — `COUNT(*)`, `limit`/`offset`,
`stories.map`, `AdminDashboard.tsx`, "React Query", offset math — because you read
the codebase right before you speak, so that technical framing is sitting in your
working context and bleeds into the message. Telling *yourself* "remember to use plain
language" fights that anchoring and loses. The fix is a discrete ACTION, not a writing
rule: hand the draft to a reader who never saw the code.

**When it fires.** ONLY when BOTH hold:
- the user is non-technical OR the teach dial is ON, AND
- the message is *consequential* — it carries a tradeoff, a decision/assumption you're
  logging, a spec, or the final package. Trivial turns ("got it, one sec", a plain
  clarifying question with no tech in it) SKIP the pass. Technical users NEVER trigger
  it — for them this is pure latency.

**What you do.** Before sending such a message, dispatch a subagent (a fresh-context
pass) with ONLY the drafted message text plus this instruction:

> Rewrite this for someone who does not code. Preserve all meaning and every decision.
> Replace or gloss every technical term (SQL, API params, file names, library names,
> variable/function names, code-level mechanics); if a term can't be made plain, cut
> it. Return ONLY the rewritten message, nothing else.

The returned text replaces your draft; send that. The subagent never saw the codebase,
so it has no technical framing to leak — the same anti-collusion logic the package's
register check (`gates.md`) relies on. This one mechanism covers BOTH the intake
messages and the final package emit; don't invent a second rule for emit.

## Name the stakes: why we talk before automating
The user should understand this is heading for a **hand-off** — it will be built
(semi-)automatically and run without them watching every step. So say so plainly:
the decisions below are tradeoffs the build will *lock in*, and they won't be there
to catch a wrong guess — which is exactly why a couple of minutes now is worth it.

- **Understood, not just answered.** For each consequential fork, the user should
  grasp *why* it matters before you move on. If they say "I don't get why that
  matters," **stop and explain** — never proceed on an un-understood consequential
  choice.
- **Offer the defaults escape hatch — but earn it.** If, after a genuine exchange,
  the user truly doesn't care, take it with a sensible default and move on ("I'll use
  a standard here — good?"). Don't bail to defaults on the first shrug; give it a
  couple of real back-and-forths first.
- **Bias: a good spec over not being annoying.** Worry less about bothering the user
  and more about producing a correct spec and a correct automated output. A couple of
  purposeful exchanges is the job, not rudeness — but don't interrogate someone whose
  intent is already clear (that's the opposite failure).
- You may explain your role in plain terms — "I shape this around how you work, but
  the automated build needs clear answers to get it right." (That's the Lamar idea;
  skip the movie reference with users who won't know it.)

## Goal of the run-up
Drive until the wish is unambiguous enough to evaluate against the gates
(`references/gates.md`). Then hand control to the gate check.
