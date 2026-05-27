# MinerWatch — Umbrel Community App Store

This folder is a **ready-to-publish Umbrel Community App Store**. Unlike the
official App Store (`getumbrel/umbrel-apps`, which needs a PR + review), a
community store is just a GitHub repo that Umbrel users add by URL — no
approval needed.

## Layout

```
umbrel-app-store.yml          # store id "imlenti" + name "MinerWatch Store"
imlenti-minerwatch/
  umbrel-app.yml              # app manifest (id MUST be imlenti-minerwatch)
  docker-compose.yml          # pulls ghcr.io/imlenti/minerwatch:<version>
  1.jpg 2.jpg 3.jpg           # gallery screenshots (ADD THESE — see below)
  icon.svg                    # app icon (optional; ADD if desired)
```

The app `id` (`imlenti-minerwatch`) is prefixed with the store `id`
(`imlenti`) — Umbrel requires this, and the folder name must match the app id.

## Prerequisite: the Docker image must exist

Umbrel **pulls** the image referenced in `docker-compose.yml`
(`ghcr.io/imlenti/minerwatch:1.6.0`); it does not build from source. Publish it
first via the `.github/workflows/docker-publish.yml` workflow in the main
MinerWatch repo (push a `vX.Y.Z` tag, or run it manually with the version
input). For production, pin to an immutable digest:

```
image: ghcr.io/imlenti/minerwatch:1.6.0@sha256:<digest>
```

Get the digest with:

```bash
docker buildx imagetools inspect ghcr.io/imlenti/minerwatch:1.6.0
```

## Publishing the store

1. Create a new public GitHub repo (e.g. `imlenti/minerwatch-app-store`), or
   click **Use this template** on
   <https://github.com/getumbrel/umbrel-community-app-store>.
2. Copy the **contents of this folder** to the repo root (so
   `umbrel-app-store.yml` is at the top level, with `imlenti-minerwatch/`
   beside it).
3. Add the gallery images `1.jpg`, `2.jpg`, `3.jpg` (1280×800 recommended) and
   optionally `icon.svg` inside `imlenti-minerwatch/`.
4. Commit and push.

## Installing on Umbrel

On the umbrelOS device: **App Store → ⋯ → Community App Stores →** paste the
repo URL → MinerWatch appears under the "MinerWatch Store" section → **Install**.

## Updating

Bump `version` in `imlenti-minerwatch/umbrel-app.yml`, update the image tag (and
digest) in `imlenti-minerwatch/docker-compose.yml`, and push. Umbrel offers the
update to every installed instance. The `${APP_DATA_DIR}/data` volume (DB, push
keys, settings) is preserved across updates.

> Note: in-app updates from MinerWatch's own "Update" page are intentionally
> disabled under Umbrel/Docker (the image is immutable). The page still shows
> when a newer release exists, but updating happens via the store, not the
> button.
