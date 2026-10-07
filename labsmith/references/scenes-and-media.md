# Scene links and lab clips

## 1. Scene presets

A scene is a named, ready-made exercise: instrument, part, feature (task), and optionally a wrong-use scenario or a camera view. Keep presets in one JSON file that both the app and the content generator read.

```json
{
  "vernier-reading": { "gauge": "…vernier.150", "workpiece": "…shaft.25.00", "feature": "od", "view": "face" },
  "error-not-seated": { "gauge": "…digital.150", "workpiece": "…blind.40", "feature": "depth_bore", "scenario": "not_seated" },
  "blind-depth-reject": { "gauge": "…digital.150", "workpiece": "…blind.30", "feature": "depth_bore" }
}
```

- URL: `/?scene=<id>` (and `/zh/?scene=<id>`).
- Look the id up as an **own key only** (`Object.hasOwn`), so `?scene=constructor` or `__proto__` cannot reach the prototype.
- A shared-state hash (full saved state) wins over a scene parameter.
- Apply the setup (select instrument, part, feature; seat; bring the jaws to contact) before the first render; run camera-moving parts (demo focus, readout view) after the opening camera move ends.
- Remove the `scene` parameter from the address after applying it, so URL-state syncing does not re-apply it on reload.

## 2. Tests for every preset

For each preset: the setup applies without rejection; contact is engaged and validity is `valid`; tolerance passes, except presets whose id ends in `-reject`, which must fail; with a scenario, the scenario is active, the mistakes panel is open and the result is not `pass`. Add a test that an unknown part is refused without changing the selection.

## 3. Recording clips

Goal: about 5 s per scene, per site language, as H.264 MP4 (universal playback) plus a WebP poster and a JPEG preview for link cards.

- Serve the production build (`vite build` + `vite preview`); record with Playwright `recordVideo` at 1280×720, software GL.
- Start the clip relative to the opening-move marker: from about 1.2 s before it to about 4 s after, which covers the end of the move and the scene's own action (readout fly-in, demo focus, guidance card).
- Encode: `ffmpeg -ss <start> -t 5.5 -i raw.webm -vf scale=960:-2,fps=24 -c:v libx264 -preset slow -crf 30 -pix_fmt yuv420p -an -movflags +faststart`. Poster: the last frame as WebP (quality ~78); preview: the poster as JPEG.
- Budget: clip ≤ 300 KB, poster ≤ 60 KB. Check a frame strip of each clip (`fps=1,tile=5x1`) for black frames, loading screens or wrong state before publishing.
- Re-record when the UI, models or camera change visibly. Pages fall back to text cards when a clip is missing.

## 4. Using clips on pages

- `<video autoplay muted loop playsinline preload="metadata" poster=…>` with explicit width/height; a small script plays clips only while on screen and not at all under `prefers-reduced-motion`.
- Caption with the scene name and an "open in the lab" link.
- Describe clips in structured data (`VideoObject` with name, description, thumbnail, content URL, upload date, duration) and in a video sitemap.
