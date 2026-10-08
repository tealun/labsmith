# Reenactments: scripted lab films, follow mode and video

A reenactment replays a lesson in the lab itself: the same domain commands a learner would issue,
with a shot list and captions. One script serves three uses: **watch** (cinematic playback in the
lab), **follow** (the learner performs each step; the domain judges it) and **video** (frames
rendered offline into MP4 for the guide pages).

## 1. Script = data

```json
{
  "id": "scene-shaft-outside", "version": 1, "scene": "shaft-outside",
  "start": { "gauge": "…digital.150", "workpiece": "…shaft.25.00", "feature": "od" },
  "title": { "key": "common.title.outside" },
  "beats": [
    { "id": "zero", "do": [{ "type": "DriveJoint", "jointId": "slider", "positionMm": 0 }, { "type": "SetOrigin", "at": 2.6 }],
      "expect": { "reading": 0 }, "shot": { "size": "insert", "target": "display", "move": "push-in", "duration": 5, "speed": 0.5 },
      "caption": "common.zero.digital" }
  ]
}
```

- `do`: ordinary store commands, each optionally `at` film seconds into the beat. Never write state directly.
- `expect`: what the domain must show after the beat (reading, contact, validity, verdict; `notPass` for wrong use and rejected parts). Tests and follow mode use the same field.
- `shot`: size (establishing … extreme → a framed width in mm around an anchor), move (push-in, pull-out, dolly, orbit, orbit-out, crane, track), duration in **film** seconds, `speed` 0.25–1 for slow motion.
- `caption`: an i18n key. Shared captions (`common.*`) take their numbers and names from the domain when the beat starts, so a caption never states a value the model did not produce.
- Version in the script; published media paths carry it and are never overwritten. A generator
  bumps the version of any script whose content changed; hand edits bump it by hand.

## 2. Generate the routine ones

When many presets share a structure (one per task type), generate their scripts from the preset file
with a template per task: title → zero check → place → contact (slow) → read with a lower third →
outro; wrong-use presets add "do it wrong → the same lower third with the wrong result". Keep the
generated files in the repository (reviewable, testable); hand-written films are not overwritten.

## 3. Tests (gate before any render)

Run every beat of every script through the store, without resetting between scripts: no command
refused, every `expect` met, a pass only with valid contact, correct measurements pass, `-reject`
presets and wrong-use demos never pass, every preset has a script, every caption key exists in all
locales. Script ids are looked up by own key only.

## 4. Runtime

- **Film clock vs animation clock.** The player maps film time to beats and fires due commands once;
  the animation clock advances at the beat's speed. Take GSAP's root timeline off its ticker
  (`gsap.ticker.remove(gsap.updateRoot)`, then `gsap.updateRoot(t)`), and make hand-written glides
  read the same clock — slow motion then reaches every tween, and the domain is untouched.
- **Director** owns the camera while playing: frame anchors by shot size, ease every move, keep the
  readout's own "up" when square to it (an instrument standing on end must not show sideways digits).
- **Captions in a 2D canvas** over the WebGL canvas: the live player and the rendered frames share
  the pixels. Mirror the caption into an `aria-live` region; under `prefers-reduced-motion` cut to
  each beat's final framing and drop fades.
- Watch mode hides panels, offers pause / replay / exit, does not write the bench to the URL and
  restores the previous state on exit. Follow mode keeps panels and shows one step at a time.
- Offer watch / follow / download when the current setup equals a script's start.

## 5. Offline render

`?play=<id>&render=1` (honoured only under automation, `navigator.webdriver`): canvas
`frameloop="never"`, pixel ratio 1, no quality downgrade. The harness calls a page hook per frame:
fire due commands → let the UI commit (a few macrotasks) → advance R3F one fixed step → composite
WebGL + captions per language → PNG. Two renders must be byte-identical (compare frame hashes).
Encode H.264 `yuv420p` `+faststart`; budget ≤ 30 MB per film. Expect ~1–2 s per 1080p frame under
software GL: render drafts sparsely (every Nth frame) for review, full cuts in the background.

## 6. Publish

Videos and posters to object storage on a media subdomain (versioned keys, immutable cache,
`Content-Disposition: attachment` still plays in `<video>`); captions (WebVTT from the rendered cue
list) on the site itself so `<track>` needs no CORS. Register files, sizes, titles and the
script's content hash in a manifest the app and the page generator both read. The page generator
fails when a registered video's script changed or its version differs (stale video), and lists the
articles still waiting for one — so a later agent cannot silently ship a film that no longer
matches its lesson. Put the whole pipeline, as commands, in the project's agent instructions. Add the media origin to CSP `media-src` and `img-src`
only, and run the security review before pushing that header change. Credentials live in an
ignored `.env` or the environment, scoped to the one bucket.
