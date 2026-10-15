---
title: "First launch"
description: "What happens the first time you start Stratum."
category: "Getting Started"
categoryOrder: 1
order: 2
---

The first start prepares the vanilla server files that Stratum needs. You do not download them yourself.

## What happens

1. The launcher reads the Vintage Story stable and unstable version manifest and selects the vanilla build that the Stratum release targets.
2. It downloads the official Vintage Story server archive and verifies it.
3. It unpacks the archive.
4. It writes Stratum's patched files on top.
5. It starts the server.

Later starts use the prepared install. Stratum prepares again only when the Stratum version or the base Vintage Story version changes, or when you pass `--stratum-refresh`. Refresh reuses a cached archive when its checksum is valid. To force a new download, remove the cached archive from `.vanilla-cache` first.

## Launcher flags

The launcher reads these flags. Anything else goes to the server, such as `--port` and `--dataPath`.

| Flag | Effect |
| --- | --- |
| `--stratum-version` | Print version information and exit |
| `--stratum-help` | Print the launcher options and exit |
| `--stratum-refresh` | Re-extract and rewrite the base server files and patched files. It downloads the archive only when the cached copy is missing or fails checksum verification. |
| `--stratum-skip-bootstrap` | Skip vanilla preparation and the patched-file overlay on every run. Use this only when both are already prepared. |
| `--stratum-prepare-only` | Prepare the install, then exit. Exits with an error if the build contains no patched files. Cannot be combined with `--stratum-skip-bootstrap` |
| `--stratum-no-banner` | Start without the startup banner |

## Set up access

A new dedicated server is invite-only. Before you configure it to run as a service, start it once in a terminal and wait for startup to finish. At the server console, run these commands, replacing the names with your own:

```
/op <your player name>
/whitelist add <player name>
```

Run `/whitelist add` for each player who should be able to join. Stop the server with `/stop` in the console, then set up the [systemd service](/docs/getting-started/running-as-a-service). A systemd service has no interactive console. In game, `/op` requires the `grantrevoke` privilege and `/whitelist` requires the separate `whitelist` privilege.

## Data folder

The world and settings are stored in the data folder. Without `--dataPath`, that is a `Data` folder next to `StratumServer`. The launcher prints the path in its startup banner unless you pass `--stratum-no-banner`. Pass `--dataPath <path>` or `--dataPath=<path>` to choose another location.

The first start creates `serverconfig.json`, `serverroles.json`, `stratum.json`, `stratum-commands.json`, and `stratum-performance.json` in the data folder. If preparation fails, the launcher prints the failure and exits. Fix the reported cause and run it again; the vanilla base marker is written only after preparation succeeds. The `Mods` folder in the install directory also receives Stratum's patched `VSEssentials.dll` and `VSSurvivalMod.dll` assemblies.

For a headless install, run `./StratumServer --stratum-prepare-only --stratum-no-banner`. A successful preparation prints `Stratum: prepare complete` and exits. A failure prints `Stratum: vanilla asset bootstrap failed: <message>` and exits with an error. The launcher suggests `--stratum-skip-bootstrap` as a recovery option only when the needed vanilla assets are already present.

## Next steps

Read the [configuration overview](/docs/configuration/overview) to change settings, and the [player commands](/docs/commands/player-commands) to see what players can use.
