<p align="center">
  <img src="assets/brand/repo-card-1280x640.png" alt="Lamar — designed to work with your thinking style" width="100%">
</p>

# Lamar

> Designed to work with your ~~throwing~~ *thinking* style.

Lamar is an interview-driven spec tool. It talks with you, asks the right questions, and produces a
build-ready PRD and implementation plan so the building can be automated — shaping the spec around **how you think**
instead of forcing you into a fixed PRD format. It meets three kinds of people where they are:

- **Knows what they want, but not how to build it** — Lamar draws out the spec in plain terms.
- **A strong technical spec already** — Lamar challenges assumptions and stress-tests it.
- **A complete novice** — Lamar walks through the tradeoffs and nails down the spec before building.

## Why "Lamar"?

Named after **Lamar Latrelle** in *Revenge of the Nerds* (1984). In the Greek Games javelin event,
the nerds don't retrain Lamar's throw — they **engineer a javelin around his throwing style**, and it
whips down the field to win. Same idea here: the tool adapts to your style instead of making you adapt
to it.

> "Wormser's a master at aerodynamics, and he designed the javelin to go along with Lamar's
> limp-wristed throwing style." … "Wormser! **It worked!**"
> — *Revenge of the Nerds* (1984)

## How it works

Lamar is the **thinker**. Your coding agent is the **builder**.

```
/lamar  →  asks the right questions        →  writes a PRD and an implementation plan
        →  you hand both to your agent     →  it builds task by task and checks its own work
```

Lamar refines your wish until it's unambiguous, then writes two files. The **PRD** says
what you are getting, in your own level of language. The **plan** is a checklist of
small, independently verifiable tasks, each with the check that proves it is done. Your
coding agent reads both, builds one task, runs the check, ticks the box, and moves
on. No second tool, no servers, no private infrastructure.

**What Lamar promises.** One hand-off, and a check for every task, so the agent can tell
whether it finished. Lamar does not promise correct code on the first pass with no
review. If a check keeps failing, the plan tells the agent to stop and report which
check failed. It must not mark the task done. The ticked boxes show where it stopped.

## Install

```bash
# add this repo as a plugin marketplace, then install the lamar plugin
/plugin marketplace add henryjrobinson/lamar
/plugin install lamar
```

Then start any non-trivial build with `/lamar` (or just describe what you want — Lamar
triggers on "let's build", "add", "fix", "change", …). It sizes the work and, for
anything beyond a trivial change, runs a short refinement conversation before emitting
the package.

## Run it (the hand-off)

Lamar writes `<feature>-prd.md` and `<feature>-plan.md` into your `docs/` folder. Say
yes when Lamar offers to start the plan, or give both files to any coding agent with
one message:

```
Read docs/<feature>-prd.md, then carry out docs/<feature>-plan.md.
Follow the rules at the top of the plan.
```

If the session ends early, start a new one with the same message. The agent continues
from the first unticked task.

### Optional: build with Ralph instead

Earlier versions of Lamar wrote a `prd.json` for the
[Ralph](https://github.com/snarktank/ralph) runner, which restarts a coding agent once
per story. That path still works. Set `execution_target: ralph` in your `.lamar.md`,
then:

```bash
git clone https://github.com/snarktank/ralph
cp prd.json ralph/prd.json          # the prd.json Lamar wrote
cd ralph && ./ralph.sh --tool claude
```

The Ralph path needs a clone of snarktank/ralph (which needs
[`jq`](https://jqlang.github.io/jq/)) and the
[`claude`](https://github.com/anthropics/claude-code) CLI.

## Configure per repo (optional)

Drop a [`.lamar.md`](.lamar.md.example) in your repo root to set the tier floor, coding
standards, test expectations, data class, and the execution target. The default target
is `plan` (the PRD and the plan). See [`.lamar.md.example`](.lamar.md.example).

## Example

[`examples/pagination/`](examples/pagination/) walks a real fuzzy wish ("the list is
slow, page through it, newest first") all the way to the PRD and the plan Lamar writes.
It also keeps the `prd.json` for the optional Ralph path.

## Brand assets

See [`assets/brand/`](assets/brand/) — full spec, sizes, and regeneration notes in
[`assets/brand/BRAND.md`](assets/brand/BRAND.md). Highlights:

- `repo-card-1280x640.png` — social card / README banner
- `promo-video.mp4` — short promo clip
- `icon-javelin-mark.png` + `favicon-*` — favicon; `icon-character-badge.png` — avatar
