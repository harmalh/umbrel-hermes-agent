# App Submission

## App name

Hermes Agent

## 256x256 SVG icon

Attach the icon from the packaging repo: `assets/icon.svg` in [harmalh/umbrel-hermes-agent](https://github.com/harmalh/umbrel-hermes-agent) (or finalize with Umbrel design feedback).

## Gallery images

Upload 3–5 screenshots (1440×900 PNG) of the Umbrel status page and/or upstream Hermes documentation flows, or raw UI if you add a dedicated web UI later.

## I have tested my app on

- [ ] umbrelOS on a Raspberry Pi
- [ ] umbrelOS on an Umbrel Home
- [ ] umbrelOS on Linux VM

## Notes for reviewers

- **Image:** `ghcr.io/harmalh/hermes-agent-umbrel:0.2.0` — multi-arch (amd64 + arm64). Replace `REPLACE_ME` in `umbrel-app.yml` `submission:` with this PR URL after opening.
- **Digest:** After merge consideration, pin `image: …@sha256:<multi-arch-index-digest>` per project conventions (run packaging workflow with `push_image` and copy digest from Actions log).
- **No built-in browser UI:** Hermes gateway targets Telegram/Discord/etc.; `web` is nginx with setup instructions (per Umbrel App Framework guidance for headless apps).
- **First run:** Users run `hermes setup` via `docker exec` or edit `.env` under `APP_DATA_DIR/data`.

## Files to copy into your fork

Copy the directory `submission-getumbrel/hermes-agent/` from the packaging repo into `getumbrel/umbrel-apps/hermes-agent/` on your branch.
