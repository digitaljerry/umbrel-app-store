# Personal Umbrel App Store

A community app store for umbrelOS.

## Apps

| App | Port | Notes |
| --- | --- | --- |
| **Paperclip Edge** (`homelab-paperclip`) | 8861 | A second, fully separate [Paperclip](https://github.com/paperclipai/paperclip) instance running the official image pinned to a nightly build. Runs alongside the App Store version (port 8860). |

## Install

umbrelOS → App Store → ⋯ → Community App Stores → paste this repository's URL → Add.

## Building and updating Paperclip Edge

Umbrel pulls a new image only when the `image:` line changes, so Paperclip Edge is always pinned to an immutable tag.

Actions → **Build Paperclip image** → Run workflow:

| Input | Meaning |
| --- | --- |
| `source` | `nightly` (newest upstream nightly tag, default), an upstream PR number, or any git ref/sha |
| `patches` | Apply everything in [`patches/`](patches/) on top (default on) |
| `pin` | Commit the new image and version to `homelab-paperclip` so Umbrel offers the update (default on) |

The image is pushed to `ghcr.io/<owner>/paperclip` (amd64). After the first build, set the `paperclip` package's visibility to **Public** (Profile → Packages → paperclip → Package settings) so Umbrel can pull it.

To use an official upstream image instead, set `image:` in `homelab-paperclip/docker-compose.yml` to a `ghcr.io/paperclipai/paperclip:sha-<commit>` tag and bump `version:` in `homelab-paperclip/umbrel-app.yml`.

Pre-release and PR builds migrate the database forward; an instance cannot be moved back to an older build. PR builds can contain migrations that never land upstream, so treat the instance's data as disposable when running one.
