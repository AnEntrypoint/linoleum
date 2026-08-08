# Linear Scene Editor

A single-file, dependency-free video editor that runs entirely in the browser. Drop in a video, let it auto-detect scene cuts, trim and reorder the pieces, mark silent stretches, and export a finished clip — no build step, no server, no upload.

## Run it

Open `index.html` in a browser. That's it — no `npm install`, no dev server.

## Features

- **Load a video** by file drop or URL. Each import prompts for how to add it — load as-is (one uncut scene), split into scenes, or split and mark speech — so raw and pre-processed clips can sit side by side in the same stack.
- **Automatic scene detection** — the source is scanned and split into scenes based on visual changes (color histogram / frame-diff analysis).
- **Scene stack** — reorder scenes by drag-and-drop, toggle individual scenes on/off, and preview the assembled sequence. Silence segments are kept in the stack and highlighted red, never deleted, so you can review and remove them yourself.
- **Trim panel** — adjust in/out points per scene against a live clip preview, with a live playhead and scrub-frame preview shown right next to the handle while dragging.
- **Mark Speech** — re-cuts the enabled scenes into alternating speech and silence segments, gating on RNNoise's own voice-activity-detection score (not a fixed volume threshold, so it holds up across quiet and dynamically-varying recordings) and padding each speech region so cuts don't clip word onsets/offsets. Silence segments stay in the stack, marked red, instead of being removed.
- **Export** — renders the final sequence (video + audio, respecting trims, ordering, and enabled/disabled scenes) to a WebM file directly in-browser via `canvas.captureStream()` + `MediaRecorder`, with audio mixed through a shared `AudioContext`.
- **Speech-detection preprocessing** — for detection purposes only, each source's audio is run through a slow compressor and peak-normalized as soon as it's loaded, so quiet dialog and hot/quiet recordings alike are reliably detected as speech. This processing never touches the exported/downloaded audio, which always reflects the untouched original source.

## How it works

Everything — decoding, scene detection, silence detection, and export encoding — happens client-side using native browser APIs (`<video>`, `<canvas>`, Web Audio, `MediaRecorder`). There is no backend and no ffmpeg dependency; the exported format is constrained to whatever codecs the browser's `MediaRecorder` supports (VP9/VP8 + Opus in WebM).
