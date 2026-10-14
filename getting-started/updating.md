---
title: "Updating"
description: "How Stratum versions work and how to update."
category: "Getting Started"
categoryOrder: 1
order: 3
---

## Version numbers

Releases are tagged `v<vs-version>-stratum.<rev>`, for example `v1.22.7-stratum.2`.

- The `vs-version` part is the Vintage Story release the build is patched against.
- The `stratum.<rev>` part counts the Stratum releases on that base.

Run the release whose `vs-version` matches the Vintage Story version your server runs.

## Update

1. Download the zip for the new release.
2. Stop the server.
3. Replace the launcher files with the new ones. Leave the data folder alone.
4. Start the server.

Your world and settings are in the data folder, so they stay. When a release targets a new Vintage Story version, the first start prepares the new vanilla files.

## Configuration changes in version 4

When you upgrade from a configuration older than version 4, the first start runs the version 4 migration. It sets `Hardening.InventoryGuards` to `true` once, even if it was `false` before. A `false` that you set after the upgrade stays as you set it.

Inventory contents that other players could see before the upgrade may now be hidden from them. To turn the setting off, run:

```
/stratum set Hardening.InventoryGuards false
```
