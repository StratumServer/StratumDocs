---
title: "First launch"
description: "What happens the first time you start Stratum."
category: "Getting Started"
categoryOrder: 1
order: 2
---

The first start prepares the vanilla server files that Stratum needs. You do not download them yourself.

## What happens

1. The launcher reads the release manifest for the vanilla build that the release targets.
2. It downloads the official Vintage Story server archive and verifies it.
3. It unpacks the archive.
4. It writes Stratum's patched files on top.
5. It starts the server.

Later starts use the prepared install. Stratum prepares again only when the Stratum version or the base Vintage Story version changes, or when you pass `--stratum-refresh`.

## Launcher flags

The launcher reads these flags. Anything else goes to the server, such as `--port` and `--dataPath`.

| Flag | Effect |
| --- | --- |
| `--stratum-version` | Print version information and exit |
| `--stratum-help` | Print the launcher options and exit |
| `--stratum-refresh` | Download and prepare the base server again |
| `--stratum-skip-bootstrap` | Skip the first-run preparation |
| `--stratum-prepare-only` | Prepare the install, then exit. Exits with an error if the build contains no patched files. Cannot be combined with `--stratum-skip-bootstrap` |
| `--stratum-no-banner` | Start without the startup banner |

## Set up access

A new dedicated server is invite-only. Before you configure it to run as a service, start it once in a terminal and wait for startup to finish. At the server console, run these commands, replacing the names with your own:

```
/op <your player name>
/whitelist add <player name>
```

Run `/whitelist add` for each player who should be able to join. Then type `stop` in the console. You can now set up the [systemd service](/docs/getting-started/running-as-a-service). A systemd service has no interactive console. You can also run these commands in game if your account already has the `grantrevoke` privilege.

## Data folder

The world and settings are stored in the data folder. Without `--dataPath`, that is a `Data` folder next to `StratumServer`, and the launcher prints the path at startup. Pass `--dataPath` to choose another location.

## Next steps

Read the [configuration overview](/docs/configuration/overview) to change settings, and the [player commands](/docs/commands/player-commands) to see what players can use.
