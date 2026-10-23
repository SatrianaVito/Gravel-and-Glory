# Gravel & Glory, Episode 1: "If Buffum Can Do It" (pixel-art short)

A 16-bit-style animated retelling of the pilot, drawn and scored entirely in code.
`index.html` is a single self-contained file: no images, fonts, libraries or network requests.

- `index.html`: the film.
- `ep01.txt`: the pilot script it's based on.
- `docs/brief.md`: the production brief (scenes, cast, palette, build order).

## Status

Built in stages. **Stage 1 (done): the engine and the look, plus scene 0 (title) and scene 1 (Briggs Grade cold open).**
Stages 2 to 6 (Michigan, Calumet, Texas, Scotland, the sound pass and the export script) are still to come.

## Open and play

Open `index.html` in a browser. Opening it straight from disk (`file://`) works, or you can serve the folder:

```sh
npx serve .        # or: python3 -m http.server
```

| Key / action | What it does |
| --- | --- |
| Click the picture or press `Space` | Play / pause (the first click also starts the audio) |
| `←` / `→` | Previous / next scene |
| `M` | Mute |
| Scrubber bar (bottom) | Click or drag to seek; the ticks mark the scenes |

The bar hides itself while the film plays; move the mouse to bring it back.

### URL options

- `?t=12.5` starts at 12.5 seconds.
- `?scene=1&at=6` starts 6 seconds into scene 1.
- `?export=1` hides all UI, sizes the canvas to 1920×1080 and hands the clock to a recorder via `window.renderFrame(n)` (frame `n` at 12 fps). `window.FILM` reports `fps`, `frames` and the scene list.
- `?test=font` shows the bitmap-font specimen and a sample speech bubble.

## How it's built

- **Canvas:** native 320×180, scaled up by a whole-number factor and letterboxed, `image-rendering: pixelated`. Everything is drawn on whole pixels.
- **Clock:** one global timeline of scenes `{ id, title, duration, draw(t) }`. The picture is quantised to 12 fps; every frame is a pure function of time, so playback and export are deterministic. There's no `Math.random()`: randomness comes from a seeded hash.
- **Sprites:** string maps (`"..kk.."`) with a palette lookup, or char grids painted by code, cached to small canvases. Cars pitch by shearing column by column, which keeps the pixel grid intact.
- **Text:** a hand-made 5×7 bitmap font (capitals, lower case, digits, punctuation, `£ $ ° #`), speech bubbles with a pixel border and tail, typed on at 30 characters a second.
- **Sound:** Web Audio, all synthesised: engine notes tied to the car, type blips, the landing crunch, the crow, wind. The full chiptune score comes in the sound pass.

## Export

Coming in stage 6: a Playwright + ffmpeg script in `export/` that renders `gravel-and-glory-ep01.mp4` at 1920×1080 and a GIF of the Hump jump.
