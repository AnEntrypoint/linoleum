# Linear Scene Editor

A single-file, dependency-free video editor that runs entirely in the browser. Drop in a video, let it auto-detect scene cuts, trim and reorder the pieces, strip out silence, and export a finished clip — no build step, no server, no upload.

## Run it

Open `index.html` in a browser. That's it — no `npm install`, no dev server.

## Features

- **Load a video** by file drop or URL.
- **Automatic scene detection** — the source is scanned and split into scenes based on visual changes (color histogram / frame-diff analysis).
- **Scene stack** — reorder scenes by drag-and-drop, toggle individual scenes on/off, and preview the assembled sequence.
- **Trim panel** — adjust in/out points per scene against a live clip preview.
- **Speech Only** — automatically re-cuts the enabled scenes down to just the segments containing speech, gating on signal level (dBFS) and padding each kept region so cuts don't clip word onsets/offsets.
- **Export** — renders the final sequence (video + audio, respecting trims, ordering, and enabled/disabled scenes) to a WebM file directly in-browser via `canvas.captureStream()` + `MediaRecorder`, with audio mixed through a shared `AudioContext`.

## How it works

Everything — decoding, scene detection, silence detection, and export encoding — happens client-side using native browser APIs (`<video>`, `<canvas>`, Web Audio, `MediaRecorder`). There is no backend and no ffmpeg dependency; the exported format is constrained to whatever codecs the browser's `MediaRecorder` supports (VP9/VP8 + Opus in WebM).
