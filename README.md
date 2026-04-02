# umbrel-hermes-agent

## Purpose

Packaging repo for **Hermes Agent** on Umbrel. It currently ships a **minimal
nginx stub** so you can validate the native install path via the community
store before replacing it with a real Hermes container.

## Intended Umbrel app role

Run Hermes (or a companion service) as a native Umbrel app with a web UI or
setup page. The stub proves compose, hooks, and store metadata; swap in a real
image when ready.

## Canonical install surface

Add the community store in umbrelOS:

`https://github.com/harmalh/umbrel-community-store`

Install **Hermes Agent** (`harmalh-hermes-agent`) from that store. Copy
`docker-compose.yml`, `umbrel-app.yml`, and `hooks/` into the store’s
`harmalh-hermes-agent/` folder when you cut a release (see store
`docs/RELEASE_CHECKLIST.md`).

## Current status

**Minimal nginx stub** (`nginx:1.25-alpine`) with `hooks/pre-start` seeding
`index.html` under `APP_DATA_DIR/www`. Not a production Hermes deployment.

## Local development notes

- `id` in `umbrel-app.yml` is `harmalh-hermes-agent` to match the community store
  folder name and Umbrel’s `APP_HOST` (`harmalh-hermes-agent_web_1`).
- Replace `web` service with your real image; keep `app_proxy` env in sync with
  `{folder}_{service}_1`.

## Local testing notes

- Standalone `docker compose config` fails without Umbrel’s `app_proxy` merge; CI validates YAML only.
- Device testing: follow `umbrel-community-store/docs/UMBREL_DEVICE_TESTING.md`.

## CI/CD overview

| Workflow | Purpose |
|----------|---------|
| `.github/workflows/ci.yml` | YAML validation on `main` (compose struct via PyYAML; no full `compose config`). |

## TODO / roadmap

- Replace nginx with the real Hermes Agent image and configuration.
- Custom icon and gallery in `umbrel-app.yml`.
- Image build + GHCR (see TODO in `ci.yml`).
- Optional `.env.example` when env surface is known.
