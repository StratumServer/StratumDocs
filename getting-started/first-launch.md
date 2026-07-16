---
title: "First launch"
description: "What happens the first time you start Stratum."
category: "Getting Started"
categoryOrder: 1
order: 2
---

The first time you start Stratum it prepares the vanilla server files it needs. You do not download them yourself.

## What happens

1. Stratum downloads the matching vanilla server build from the official CDN.
2. It unpacks those files next to the launcher.
3. It applies the patched Stratum runtime on top.
4. The server starts.

This runs once. Later starts go straight to running the server.

## Launcher flags

Stratum reads a few of its own flags. Everything else is passed straight through to the server, such as `--port` and `--dataPath`.

- `--stratum-version` prints version info and exits
- `--stratum-help` prints the launcher options and exits
- `--stratum-refresh` re-downloads and re-extracts the vanilla assets
- `--stratum-skip-bootstrap` skips the first-run download
- `--stratum-no-banner` starts without the startup banner

## Data path

If you leave off `--dataPath`, Stratum uses the local `Data` folder next to the launcher.
