# Worked example — the voice engine

A longer, real case study: a **non-technical PM with a strong intuition** and a model working
together turned a fuzzy, almost-philosophical wish ("make the AI write like the *actual person*")
into a tested implementation and a locked engineering decision — over one long session.

Unlike the [pagination](../pagination/) example (a clean wish → `prd.json` → build), this one is
messy on purpose. It shows what Lamar is *for*: not transcribing a spec the user already has, but
**drawing a real spec out of taste and instinct the user can't yet express in technical terms** —
and it shows the human catching the model when the model's first instinct was wrong.

> Honesty up front: the quality verdicts below come from **two judges (the PM and his wife)**,
> on **N=2 essays**, using **written** samples (the product's real input is *spoken* transcripts),
> and the "how much input is enough" question came back **unresolved**. This is a process case
> study, not a benchmark.

## 1. The fuzzy wish

> "I want the memoir to read like the *actual person* wrote it. And the person **cannot be a
> generic profile** — it has to be based on their real language, *mathematically*."

That's not a spec. It's a feeling with a hard constraint buried in it ("mathematically", "not
generic"). The whole job was to make it buildable without sanding off what the user actually meant.

## 2. The collaboration arc (told straight)

**The PM (Henry) supplied the irreplaceable parts:**
- The **idea and the constraint** — voice from the person's own words, not a persona.
- The **pushback that bent the work.** When the model proposed a tidy "voice profile," he rejected
  it cold: *"you've reduced my vector/fine-tune idea to a scoring rubric that's biased by topic and
  has no guarantee of beating just pasting a sample into the model and saying 'write like this.'"*
  He was right, and it changed the whole approach.
- The **reframe.** *"It's character, not author — like faithful fan-fiction. Atticus Finch is
  invented, yet you'd know his voice in a new chapter."* That single move relocated the problem
  from forensic stylometry (telling authors apart) to **characterization** (writing *as* someone) —
  a different field with different tools.
- The **insistence on a real test.** No hand-waving — hold out one of his own essays, regenerate
  it from stripped facts, and judge blind.
- **A second judge.** He brought his **wife** in to rate voice independently — the one signal the
  model fundamentally cannot produce.
- **Product taste under sensitivity.** On a "life completeness" meter, he flagged the
  death-countdown risk before any research did.

**The model (Claude) supplied the legwork — the grad student to his advisor:**
- Two deep, adversarially-verified research passes (the first in the *wrong* field — stylometry —
  until the reframe; the second on character/persona generation).
- Built the pieces: a corpus extractor, an evidence-grounded "voice bible," a character
  ghostwriter, an idempotent save path, and a severity-tiered fact-fidelity gate — each with tests.
- Ran the held-out bake-offs and reported results, including an **honest null** (the attempt to
  *automate* the voice-floor measurement failed).

## 3. The pivotal moment — the human's instinct beat the model's engineered default

The model's first instinct was the elaborate artifact: analyze the person's writing into a
structured "bible" of rules and condition generation on that.

The bake-off said otherwise:
- **Round 1:** plain "here's a pile of their raw writing, sound like this" **matched or beat** the
  bible on voice. The PM's original skepticism — vindicated.
- **Round 2 (controlled, 2 judges):** the truth was sharper than either side's opening position —
  **bible + a *large* amount of raw writing won on voice *and* halved fabrication**, while
  bible-alone and thin-raw both failed. Neither "just summarize them" (model's instinct) nor "just
  paste raw text" (naive baseline) was the answer; the *combination* was.

The model would have shipped the bible. The human's "I don't buy it, prove it" forced the test
that produced the actual answer.

## 4. How it maps to the Lamar flow

| Lamar stage | What happened | 
|---|---|
| **Intuition → intake** | Worked. The wish was a feeling; the intake's job was to make "mathematically, not generic" concrete without flattening it. The reframe ("character not author") came *from the user mid-intake* — intake has to stay open to that. |
| **Tiering** | This was Tier 2+ (real logic, customer-data class) — correctly never treated as "just do it." |
| **Spec** | Emitted as a real tracker (`prd.json`, experiment-first ordering). |
| **Build → verify** | Where it got interesting: **verification was a human-judgment loop, not `typecheck passes`.** Lamar/Ralph's standard "machine-checkable acceptance" did *not* cover the load-bearing criterion ("does it sound like him?"). |

**The move that mattered, and that Lamar doesn't name today: experiment-first.** Before committing
the spec to an approach, build the *cheapest test that discriminates between approaches* (the
bake-off) and let the result pick. That single reordering is why the wrong approach (bible-only)
didn't get built.

## 5. The meta-lesson

A non-technical PM's **taste** + a model's **legwork** produced a spec **neither would have alone**.
The model could research, build, and measure tirelessly — but it could *not* judge "that's him,"
and left to its own defaults it would have shipped the wrong design with confidence. The human
couldn't write the code — but his taste was the ground truth the whole thing was optimizing toward.
Lamar's premise (engineer around the user's style, don't retrain the user) held: the value was in
*keeping the human's judgment in the loop at the exact points the model is weakest*, not in
extracting a clean spec up front.

## 6. What this says Lamar should do better

1. **Add an "experiment-first" gate for high-uncertainty specs.** When an approach is *assumed* but
   unproven (here: "the bible will capture voice"), Lamar should refuse to lock the spec and
   instead emit a tiny *discriminating experiment* as the first story — build the cheap test,
   read the result, *then* spec the build. Treat "we don't actually know if this works" as a
   first-class intake signal, not an afterthought.
2. **Make "human-judgment acceptance criteria" a first-class citizen.** Lamar/Ralph assume
   machine-checkable pass/fail (`typecheck`, `tests`). Many real specs have an irreducibly human
   criterion ("sounds like him," "feels dignified"). Lamar should let a story declare a
   **human-eval gate** — who judges, the rubric, blind protocol — and treat it as a legitimate,
   non-automatable acceptance step rather than forcing everything into a green checkmark.
3. **Capture and protect the user's reframes.** The biggest leverage came from the user's mid-intake
   reframe ("character, not author"). Lamar should explicitly listen for and *preserve* these
   reframes in the ledger ("the user relocated the problem from X to Y") — and re-run its own
   framing when one lands, rather than steering back to its first interpretation.

---

*Source work lives in the `family-legacy` repo: design `docs/CHAPTER_TAB_REWRITE_UNDERSTANDING.md`,
tracker `docs/prds/voice-memoir-system-prd.json`, research `docs/research/`. This file is the
narrative; the spec/tracker is the artifact.*
