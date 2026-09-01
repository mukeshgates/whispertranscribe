# Word-Timestamp Transcriber

A free, static web page that transcribes audio with word-level timestamps —
entirely inside the visitor's browser. No server, no API keys, no cost per use.

Built for syncing narration to captions/effects in video editing (e.g. Remotion).

## How it works

This is a single `index.html` file. It uses [Transformers.js](https://huggingface.co/docs/transformers.js)
to run OpenAI's Whisper speech-recognition model via WebAssembly/WebGPU, directly
in the browser tab. Audio never leaves your device — there's no backend at all.

## Deploy to GitHub Pages

1. Create a new GitHub repo (or use an existing one) and push this folder's contents
   (`index.html`, `README.md`, `.nojekyll`) to it.
2. On GitHub: **Settings → Pages → Source → Deploy from a branch → `main` / `(root)`** → Save.
3. Wait a minute, then open the URL GitHub gives you (looks like
   `https://<username>.github.io/<repo>/`).
4. Drop in an audio file, pick a model quality, click Transcribe.

That's it — no build step, no dependencies to install, no server to run.

## Model quality options

| Option | Model | Notes |
|---|---|---|
| Fast | whisper-tiny | Quickest, ~60MB download, least accurate on unusual words |
| Balanced | whisper-base (default) | Good tradeoff, ~90MB |
| Accurate | whisper-small | Best quality, ~250MB, slower |

The model downloads once per browser (cached automatically after that) — first
transcription on a machine will pause to download it.

## Output

- **.txt** — plain transcript
- **.srt** — ready-to-use subtitle file, auto-grouped into readable lines
- **.json** — every word with its exact start/end time in seconds, for feeding
  into Remotion or any other tool that needs precise sync data

## Notes

- Works best in Chrome or Edge (WebGPU support = much faster). Firefox/Safari fall
  back to slower CPU-only WebAssembly but still work.
- Long files (60+ minutes) on CPU-only browsers can take a while — this is normal.
- Verified working end-to-end before shipping (transcript + word timestamps checked
  against known reference audio).
