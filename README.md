# SAM WALKS — the walkable website

An interactive, game-style site for the channel. The brand is "I'm walking and I'm talking" — so the site **is** a walk. One continuous horizontal road, one shot, no cuts. The visitor does the walking; Sam does the talking.

## Run it

No build, no dependencies. Open directly:

```
open creativity/sam-walks-site/index.html
```

or serve the folder with any static server. Works from `file://`.

## Controls

| Input | Action |
|---|---|
| `→` / `D` (hold) | walk forward |
| `←` / `A` (hold) | walk back |
| `Space` / `↑` / `W` | jump (rocks on the road block you — hop them) |
| `Enter` | watch the nearby walk on YouTube |
| `L` | open the index (episode list, teleports) |
| `1` / `2` | answer Sam's questions |
| Click/drag the red line | scrub the walk (it's a scrub bar — seek anywhere) |
| `Esc` | close the index |

Touch devices get ◀ ▲ ▶ buttons (hold to walk, tap to jump).

## What's in the walk

- **Three real published walks** as roadside video cards (EP.001–003), linking to the real YouTube videos (`FAbBtXXbToQ`, `RKPg3u4UHjI`, `m4VSpYm4u-E`) with real titles and durations from `data/videos.csv`.
- **Monologue captions** — burned-in subtitle capsules, lines lifted from the real video descriptions ("The AI keeps doing more for me. My nervous system disagrees."). Sam talks the moment you start — no dead air — and asks the viewer questions mid-walk, exactly like in the videos ("What are you prioritising?").
- **Jumping + street interruptions** — rocks on the road block the walk until you hop them; each gets a one-take remark ("Hop over — still one take."). Dust puffs (flat squares, `steps()` cuts) kick up while walking and on landing. A blinking red → cue next to Sam shows the way until the first steps are taken.
- **Chapter headings** for the content pillars: AI & Work, Own Path, Life Abroad.
- **End of the road**: about block, "the walk is the production value" quote stack, Subscribe → `youtube.com/@sam.isaacs`.

## Design source

Implements the **Sam Walks Design System** (handoff bundle from claude.ai/design, reference copy in `creativity/design-system/`):

- Tokens copied verbatim (`tokens/*.css`): footage-sampled palette (asphalt `#181511`, signal red `#e0312e`, dust, field), Archivo Black / Archivo / Space Mono.
- **The caption bar** is how Sam talks — solid asphalt capsules, white bold text, no scrims.
- **The red line is the scrub bar is the walk** — the bottom HUD progress line doubles as the seek control; stop ticks mark the three videos.
- **The timestamp**: `●REC` badge (the one permitted loop) + mono timecode mapped to walk progress (total 8:24 = sum of the three real videos), km readout, `EP · LISBON · ONE SHOT` meta.
- **No polish, on purpose**: hard 2px borders, hard offset shadows (4px 4px 0), flat colour bands instead of gradients, cuts instead of fades (captions cut in/out; walk cycle uses `steps()`), solid-asphalt index overlay (no blur), unicode icons only (`▶ → ● ✕`).
- VideoCard / EpisodeRow / QuoteBlock / Tag / Button / RecBadge / ScrubBar / MetaLine recreated faithfully from `components/`.

## Why vanilla JS (no game engine)

Phaser/Three.js/PixiJS were considered and rejected on brand grounds: the design system explicitly bans parallax, fades, blur, and decorative motion — "polish is off-brand". A flat, hard-edged DOM side-scroller is the honest version: zero dependencies, instant load, runs from `file://`, fully keyboard-accessible, respects `prefers-reduced-motion`.

## Mobile-first + accessibility

Phones are the primary device. On screens ≤720px: Sam renders 35% larger, captions 18px, video cards 220px wide on taller posts, chapter headings bigger, question chips stack full-width (44px tall), scrub dot enlarged, ◀ ▲ ▶ buttons 72px with `env(safe-area-inset-bottom)` clearance for the iPhone home indicator. `viewport-fit=cover`; pinch-zoom stays enabled.

Accessibility (reviewed against WCAG 2.2 AA): small mono text uses ink-2 on white and ink-4 on asphalt (contrast fixes), scrub bar is a keyboard-operable `role="slider"` (arrows/Home/End, live `aria-valuenow`), the index is a `role="dialog"` with focus trap and focus return, decorative elements (Sam figure, rocks, dust, cue arrow) are `aria-hidden`, captions are an `aria-live="polite"` region, video cards carry "Watch on YouTube: …" labels, touch targets ≥44px, `prefers-reduced-motion` kills all animation.

## Notes

- "SAM WALKS" wordmark is the design system's placeholder name — swap if the channel name lands elsewhere.
- Photos are the three real video stills from the design bundle (`assets/photos/`).
- British English throughout, no emoji, sentence case — per `brand/tone/tone.md`.
