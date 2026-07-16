---
title: "Updating"
description: "How Stratum versions work and how to update."
category: "Getting Started"
categoryOrder: 1
order: 3
---

## Version numbers

Stratum releases are tagged `v<vs-version>-stratum.<rev>`, for example `v1.22.3-stratum.1`.

- The `vs-version` half is the Vintage Story release the build is patched against.
- The `stratum.<rev>` half counts the Stratum releases on that base.

Run the release whose `vs-version` matches the Vintage Story version your server is on.

## Updating

1. Download the new release for your Vintage Story version.
2. Stop the server.
3. Replace the launcher files with the new ones.
4. Start it again.

Your world and settings live in the data folder, so they carry over. When a build targets a new Vintage Story version, the first start fetches the new vanilla files.
