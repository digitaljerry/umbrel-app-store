# Personal Umbrel App Store

A community app store for umbrelOS.

## Apps

| App | Port | Notes |
| --- | --- | --- |
| **Paperclip Edge** (`homelab-paperclip`) | 8861 | A second, fully separate [Paperclip](https://github.com/paperclipai/paperclip) instance running the official image pinned to a nightly build. Runs alongside the App Store version (port 8860). |

## Install

umbrelOS → App Store → ⋯ → Community App Stores → paste this repository's URL → Add.

## Updating Paperclip Edge

Umbrel pulls a new image only when the `image:` line changes, so the image is pinned to an immutable `sha-<commit>` tag.

1. Pick a build from the [upstream image tags](https://github.com/paperclipai/paperclip/pkgs/container/paperclip): `nightly/v*` git tags map to `sha-<short commit>` images.
2. Update `image:` in `homelab-paperclip/docker-compose.yml` and bump `version:` in `homelab-paperclip/umbrel-app.yml`.
3. Push. Umbrel shows an update for Paperclip Edge.

Pre-release builds migrate the database forward; an instance cannot be moved back to an older build.

## Running an unreleased pull request

Actions → **Build Paperclip PR image** → Run workflow → enter the upstream PR number.
The workflow builds `ghcr.io/<owner>/paperclip:pr-<number>-<sha>` (amd64) and, with *pin* enabled, commits the new image and version so Umbrel offers the update.

After the first build, set the `paperclip` package's visibility to **Public** (Profile → Packages → paperclip → Package settings) so Umbrel can pull it.

PR builds can contain database migrations that never land upstream; treat the instance's data as disposable when running one.
