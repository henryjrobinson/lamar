# Lamar — Brand Asset Spec

The hero/social image for the **Lamar** skill (the interview-driven tool that produces a
build-ready `ralph.md` spec shaped around how *you* think, instead of forcing you into a PRD format).

## The reference

Named after **Lamar Latrelle** (Larry B. Scott) in *Revenge of the Nerds* (1984). In the Greek
Games javelin event, the nerds don't retrain Lamar's throw — Wormser **engineers a javelin around
his throwing style**, and it whips/zig-zags down the field to win.

> "Wormser's a master at aerodynamics, and he designed the javelin to go along with Lamar's
> limp-wristed throwing style." … "Wormser! It worked!"
> — *Revenge of the Nerds* (1984)

The skill is the same move: it designs the spec around the user's natural style rather than
coaching the user into a format. That's the whole metaphor.

## Locked design

- **Concept:** original comic-book cel illustration (parody/homage — NOT movie footage, NOT the
  actor's likeness). A slim, effeminate young Black athlete in '80s track shorts + leg warmers
  mid-leap, having just released a **plain javelin bent into a wavy/undulating flight path** (it
  "flaps" like a bird). Baffled muscle-bound jocks at left with "??/!!" bubbles. Crowd of
  spectators in **Greek-fraternity-letter** shirts (nod to the film's frat premise). No distance
  numbers, no in-art banners.
- **Tagline:** **"Designed to work with your ~~throwing~~ thinking style."** — rendered with
  "throwing" struck through and "thinking" handwritten (Bradley Hand) above it, so the movie
  homage is explicit (the film: "…designed the javelin to go along with Lamar's … throwing style").
- **Wordmark:** "Lamar", bold modern sans (Helvetica Neue 800), navy `#0b1d3a` with a white halo,
  positioned **bottom-center in the grass** (clear, fully legible).
- **Aspect / sizes:** GitHub social preview = **1280×640 (2:1)**; same asset doubles as the README
  banner. 2× = 2560×1280.

## Files

| File | Use |
|---|---|
| `repo-card-1280x640.png` | GitHub social preview + README banner (canonical) |
| `repo-card-2560x1280.png` | 2× crisp version |
| `promo-video.mp4` | Promo clip (9.4s): throw → shortened flight → lands past two short javelins → tagline end card. No crowd reaction (jocks lost). |
| `hero-16x9-clean.png` | Website hero — clean art, no text (1920×1072) |
| `icon-javelin-mark.png` | Square logo mark — the wavy javelin (best for favicon) |
| `favicon-512.png` / `favicon-64.png` | Favicon sizes from the javelin mark |
| `icon-character-badge.png` | Square character-in-circle badge — repo avatar / profile image |
| `source-comic-e1.png` | Chosen source frame, pre-text, with banner removed |
| `_card-base-2x.png` | 2:1 cropped base used for the text overlay |
| `brand-card.html` | Editable text overlay — change the tagline/wordmark here and re-render |

## Regenerate the text overlay

```bash
CHROME="/Applications/Google Chrome.app/Contents/MacOS/Google Chrome"
"$CHROME" --headless --disable-gpu --hide-scrollbars --force-device-scale-factor=2 \
  --screenshot=repo-card-2560x1280.png --window-size=1280,640 "file://$PWD/brand-card.html"
magick repo-card-2560x1280.png -resize 1280x640 repo-card-1280x640.png
```

## Provenance (the generator)

- Tool: Higgsfield CLI, model **Nano Banana 2** (`nano_banana_2`), `--aspect_ratio 16:9`.
- Banner removed via image-to-image edit (`--image` of the chosen frame).
- Text composited locally (Chrome headless + ImageMagick); never AI-rendered, to keep typography crisp.

## Legal posture

Original transformative parody — no studio footage, no reproduction of the actor's face.
Stronger fair-use footing than a movie still. (Not legal advice.)
