# umbrel-hermes-agent

## Purpose

Umbrel packaging for **[Hermes Agent](https://github.com/NousResearch/hermes-agent)** (Nous Research): a **status/setup web page** (`web` / nginx) plus a **`hermes` service** running `gateway run` with persistent data under `${APP_DATA_DIR}/data` → `/opt/data`.

The official Docker Hub image [`nousresearch/hermes-agent`](https://hub.docker.com/r/nousresearch/hermes-agent) is **amd64-only**. This repo builds a **multi-arch** image (amd64 + arm64) for Umbrel Home / Raspberry Pi and publishes it to **GHCR**.

## Canonical install surface

Add the community store in umbrelOS:

`https://github.com/harmalh/umbrel-community-store`

Install **Hermes Agent** (`harmalh-hermes-agent`). Sync the store from this repo per [`docs/STORE_SYNC.md`](docs/STORE_SYNC.md) and the [umbrel-community-store release checklist](https://github.com/harmalh/umbrel-community-store/blob/main/docs/RELEASE_CHECKLIST.md).

## Image build and digest pin

1. In GitHub: **Actions** → **Build Hermes Agent Image (Manual)** → **Run workflow**.
2. Set **`hermes_ref`** to a upstream tag or SHA (e.g. `main` or a release tag).
3. Set **`image_tag`** to the Umbrel package version you ship (e.g. `0.2.0`) so `docker-compose.yml` and the registry tag match.
4. Enable **`push_image`** to publish to `ghcr.io/harmalh/hermes-agent-umbrel` (or your `image_repository`).
5. Copy the printed **multi-arch digest** from the job log and pin the `hermes` service in `docker-compose.yml`:

   `image: ghcr.io/harmalh/hermes-agent-umbrel:0.2.0@sha256:<digest>`

   Use the **index** digest from the workflow (manifest list), not a single-arch digest.

## First-time Hermes setup on Umbrel

Hermes expects API keys and config under **`APP_DATA_DIR/data`** (`.env`, `config.yaml`, etc.), mounted at `/opt/data` in the container.

- **Option A:** Umbrel **Settings → Advanced → Terminal** (or SSH), then:

  `docker exec -it harmalh-hermes-agent_hermes_1 bash`  
  Run `hermes setup` or the interactive wizard per [upstream Docker docs](https://hermes-agent.nousresearch.com/docs/user-guide/docker/).

- **Option B:** Edit `.env` / `config.yaml` on the host under the app data directory (see [configuration](https://hermes-agent.nousresearch.com/docs/user-guide/configuration)).

Do **not** commit provider API keys to git. Do not use Umbrel `APP_SEED` as a substitute for third-party API keys.

## Resource expectations

Hermes ships Playwright/Chromium and is **large** on disk and RAM. Prefer **2–4 GB** container memory on the host where possible (see upstream [Docker guide](https://hermes-agent.nousresearch.com/docs/user-guide/docker/)).

## Local testing notes

- Standalone `docker compose config` fails without Umbrel’s `app_proxy` merge; CI validates YAML only.
- Device testing: [`umbrel-community-store/docs/UMBREL_DEVICE_TESTING.md`](https://github.com/harmalh/umbrel-community-store/blob/main/docs/UMBREL_DEVICE_TESTING.md).
- **Checklist (umbrel-dev or hardware):** run **Build Hermes Agent Image** with `push_image` first; install from the community store; open the app (status page); confirm `harmalh-hermes-agent_hermes_1` logs show gateway activity after configuration; **restart** the app and confirm files still exist under app data `data/`; optionally **uninstall** and confirm data removal matches expectations.

## CI/CD overview

| Workflow | Purpose |
|----------|---------|
| `.github/workflows/ci.yml` | YAML validation on `main`. |
| `.github/workflows/build-hermes-image.yml` | Manual multi-arch build; optional GHCR push; amd64 `hermes version` smoke test. |

## Official Umbrel App Store

To open a PR to [getumbrel/umbrel-apps](https://github.com/getumbrel/umbrel-apps), copy the app from [`submission-getumbrel/hermes-agent/`](submission-getumbrel/hermes-agent/) into your fork (app id **`hermes-agent`**, `APP_HOST` **`hermes-agent_web_1`**). Use the PR description template in [`submission-getumbrel/PR_DESCRIPTION.md`](submission-getumbrel/PR_DESCRIPTION.md).
