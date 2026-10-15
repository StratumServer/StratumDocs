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

The version in the zip filename omits the leading `v`. A release can also have a dotted revision or a prerelease suffix, such as `v1.22.5-stratum.2-rc.1`.

Run the release whose `vs-version` matches the Vintage Story version your server runs.

## Update

1. Download the zip for the new release.
2. Stop the server.
3. Replace `StratumServer` (or `StratumServer.exe`) and `StratumServer.runtimeconfig.json` with the files from the new zip. By default the data folder sits inside the install folder, so do not delete the whole folder. Replace only the launcher files, or keep your data folder outside the install folder with `--dataPath`.
4. Start the server.

Back up the whole data folder before updating, including `stratum.json`, `stratum-commands.json`, and `stratum-performance.json`. A config migration rewrites `stratum.json` in place. The startup log reports when it upgrades the configuration version. GitHub's latest-release check ignores prereleases, so a prerelease may not appear in the normal update reminder.

After a base Vintage Story version change, the launcher replaces the install's `assets` folder. To prepare the new base without starting the server, run `./StratumServer --stratum-prepare-only --stratum-no-banner`; success prints `Stratum: prepare complete`.

Your world and settings are in the data folder, so they stay. When a release targets a new Vintage Story version, the first start prepares the new vanilla files.

## Configuration migration to version 4

The number 4 is the `ConfigVersion` stored in `stratum.json`, not a Stratum release number. When you upgrade from a configuration older than version 4, the first start runs the migration. It sets `Hardening.InventoryGuards` to `true` once, even if it was `false` before. A `false` that you set after the upgrade stays as you set it.

The earlier migrations also matter if you use scripts or hand-edited settings. Version 2 changed `Performance.Physics.ParallelThreshold` from 256 to 32. Version 3 moved theme and nametag settings under `Appearance`, moved MOTD and rules text under `ServerInfo`, and moved the old chat rate limit under `Chat.RateLimit.MinimumIntervalMs`. After migration, the startup log reports the old and new config versions. To roll back, reinstall the previous Stratum release and restore the matching data-folder backup, including the world and config files.

Inventory contents that other players could see before the upgrade may now be hidden from them. To turn the setting off, run:

```
/stratum set Hardening.InventoryGuards false
```
