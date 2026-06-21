<p align="center">
  <img src="assets/brand/repo-card-1280x640.png" alt="Lamar — designed to work with your thinking style" width="100%">
</p>

# Lamar

> Designed to work with your ~~throwing~~ *thinking* style.

Lamar is an interview-driven spec tool. It talks with you, asks the right questions, and produces a
build-ready `ralph.md` so the building can be automated — shaping the spec around **how you think**
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

Lamar is the **thinker**; [Ralph](https://github.com/snarktank/ralph) is the **builder**.

```
/lamar  →  asks the right questions  →  emits a build-ready prd.json
        →  you hand it to Ralph       →  ./ralph.sh builds it, story by story
```

Lamar refines your wish until it's unambiguous, then writes a `prd.json` — a checklist
of small, independently verifiable steps. Ralph reads that checklist, builds the
highest-priority unfinished step, checks its own work, commits, ticks the box, and
moves on — until every box is ticked. No servers, no private infrastructure.

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

## Run it (the hand-off to Ralph)

Lamar emits a `prd.json`. To build it autonomously, install the Ralph runner once and
point it at the file:

```bash
git clone https://github.com/snarktank/ralph
cp prd.json ralph/prd.json          # the prd.json Lamar wrote
cd ralph && ./ralph.sh --tool claude
```

**Honest cost of entry.** "No private infrastructure" is true, but it isn't literally
zero-setup. To run the build loop you need: this Lamar plugin, a clone of
[snarktank/ralph](https://github.com/snarktank/ralph) (which needs [`jq`](https://jqlang.github.io/jq/)),
and the [`claude`](https://github.com/anthropics/claude-code) CLI. All free; three small
installs. Lamar itself only writes the `prd.json` — it never runs the loop for you.

## Configure per repo (optional)

Drop a [`.lamar.md`](.lamar.md.example) in your repo root to set the tier floor, coding
standards, test expectations, data class, and the execution target. The default target
is `ralph` (local). See [`.lamar.md.example`](.lamar.md.example).

## Example

[`examples/pagination/`](examples/pagination/) walks a real fuzzy wish ("the list is
slow, page through it, newest first") all the way to the `prd.json` Lamar emits and the
command that builds it.

## Brand assets

See [`assets/brand/`](assets/brand/) — full spec, sizes, and regeneration notes in
[`assets/brand/BRAND.md`](assets/brand/BRAND.md). Highlights:

- `repo-card-1280x640.png` — social card / README banner
- `promo-video.mp4` — short promo clip
- `icon-javelin-mark.png` + `favicon-*` — favicon; `icon-character-badge.png` — avatar
