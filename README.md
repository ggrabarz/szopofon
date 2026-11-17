# Phone Bench

A phone test site for standing in a store with a demo unit. Open it on any phone, tap through the tabs, and decide whether the display, speakers, camera, touchscreen and sensors are worth buying.

Built with MkDocs Material and deployed to GitHub Pages on every push to `master`.

## Pages

| Tab | What it proves |
| --- | --- |
| Start | Live hardware readout: screen size, pixel ratio, measured refresh rate, CPU cores |
| Music | Login-free radio streams, YouTube embeds, your Spotify tracks, and sine sweeps for speaker and stereo checks |
| 4K Video | Real 4K MP4 clips with a badge reporting the decoded resolution, plus a YouTube 4K HDR embed |
| Photo | Shoot with the phone's camera or load a photo, then judge it at fit, 1:1 pixel scale, and a wipe against a second photo |
| Screen | Solid colour fills, gradient banding, grey uniformity steps, dead pixel hunt |
| Touch | Multi-touch drawing, event rate, tap accuracy, edge dead-zone map |
| Sensors | Orientation, motion, vibration, microphone level |
| Benchmarks | Verified outbound tools for deeper graphics and network checks |

Nothing is uploaded. Photos stay in the browser and are released when you clear them.

## Run locally

```bash
uv venv .venv
uv pip install --python .venv/Scripts/python.exe mkdocs-material
.venv/Scripts/python.exe -m mkdocs serve
```

`site_url` is set to the GitHub Pages address, so the local preview serves from `/szopofon/`. Open `http://127.0.0.1:8000/szopofon/` rather than the bare root.

## Deploy

`.github/workflows/pages.yml` builds with `mkdocs build --strict` and publishes through the Pages workflow. Pages is already configured with `build_type: workflow`.

## Adding a station or a clip

Radio and video sources live in the page files under `docs/`. Every external URL in this repo was checked for a live response, so verify a new one before committing it. YouTube live streams rotate their IDs and go dead; prefer static videos.
