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

- **Image:** `ghcr.io/harmalh/hermes-agent-umbrel:0.2.0@sha256:a7ee8728416b9e368b108f645978f4426b6556d64dc20c6a0a7427a21ba8e1ea` — multi-arch index digest from [Actions run 23928504678](https://github.com/harmalh/umbrel-hermes-agent/actions/runs/23928504678). Replace `REPLACE_ME` in `umbrel-app.yml` `submission:` with this PR URL after opening.
- **No built-in browser UI:** Hermes gateway targets Telegram/Discord/etc.; `web` is nginx with setup instructions (per Umbrel App Framework guidance for headless apps).
- **First run:** Users run `hermes setup` via `docker exec` or edit `.env` under `APP_DATA_DIR/data`.

## Files to copy into your fork

Copy the directory `submission-getumbrel/hermes-agent/` from the packaging repo into `getumbrel/umbrel-apps/hermes-agent/` on your branch.
