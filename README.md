# Personal Umbrel App Store

A community app store for umbrelOS.

## Apps

| App | Port | Notes |
| --- | --- | --- |
| **Paperclip Edge** (`homelab-paperclip`) | 8861 | A second, fully separate [Paperclip](https://github.com/paperclipai/paperclip) instance running the official image pinned to a nightly build. Runs alongside the App Store version (port 8860). |

## Install

umbrelOS → App Store → ⋯ → Community App Stores → paste this repository's URL → Add.

## Building and updating Paperclip Edge

Umbrel pulls a new image only when the `image:` line changes, so Paperclip Edge is always pinned to an immutable tag by the **Build Paperclip image** workflow.

**Automatic stable tracking.** Every 6 hours the workflow checks for a new upstream stable release and, if Edge is currently pinned to a stable build, pins the new release (with `patches/` applied). Umbrel then offers the update. If Edge was pinned manually to a nightly or PR build, scheduled runs leave it alone; run the workflow with `source: stable` to resume tracking.

**Manual runs.** Actions → **Build Paperclip image** → Run workflow:

| Input | Meaning |
| --- | --- |
| `source` | `stable` (latest release, default), `nightly` (newest nightly tag), an upstream PR number, or any git ref/sha |
| `patches` | Apply everything in [`patches/`](patches/) on top (default on) |
| `pin` | Commit the result to `homelab-paperclip` so Umbrel offers the update (default on) |

When no patch applies (or `patches/` is empty) and the source has an official image (`stable`, `nightly`), the official `ghcr.io/paperclipai/paperclip` image is pinned directly and nothing is built. Otherwise the image is built and pushed to `ghcr.io/<owner>/paperclip` (amd64); the package must be **Public** so Umbrel can pull it. A patch that stops applying fails the run, and GitHub emails you.

Builds migrate the database forward. Moving to an older build only works while the newer build's migrations are harmless to older code; check before switching back. PR builds can contain migrations that never land upstream, so treat the instance's data as disposable when running one.
