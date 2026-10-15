---
title: "Migrating from the stock server"
description: "Move an existing Vintage Story server to Stratum without losing the world."
category: "Getting Started"
categoryOrder: 1
order: 5
---

Stratum runs in place of the stock dedicated server, and it loads the same world files. You can move an existing server to Stratum without rebuilding the world. The steps below were checked with a world saved by the stock server.

## Before you start

Stop the stock server. Then copy the whole data folder somewhere safe. That copy is your way back if anything goes wrong.

Use a Stratum release that matches your server's Vintage Story version. [Connecting and versions](/docs/compatibility/connecting-and-versions) explains the two version numbers.

## Run Stratum on your data folder

Run Stratum with `--dataPath` pointing at the folder your stock server used. Stratum loads the world from that folder and keeps the files already there.

```bash
./StratumServer --dataPath /path/to/your/data
```

Check the startup log. You should see a line that says the savegame loaded, and a line that says the config was created or loaded. A line that says `Failed to load config` or a fatal error means the start did not finish. Stop the server and check the message before you go on.

Stop the server with `/stop` in the server console. Stratum saves the world on the way out.

## Moving the data folder

The server stores absolute paths in `serverconfig.json`. The two that matter are `SaveFileLocation`, which points at the world file, and the path to the `Mods` folder. If you move or copy the data folder, edit both paths in `serverconfig.json` before you start the server. Otherwise the server uses the old location, and it writes to that location too.

## What Stratum changes by default

A fresh Stratum config turns on several features that a stock server does not have. Check these after the move, and turn off any you do not want.

- Anonymous statistics are sent to the Stratum statistics endpoint. [Privacy and outbound requests](/docs/configuration/privacy-and-outbound-requests) lists every field and how to turn it off.
- The update check runs at startup and every 12 hours.
- Chat rate limiting is on, with a minimum of 750 milliseconds between messages from one player.
- Pregeneration is on. It pauses while players are online.
- Inventory guards are on. Other players cannot see the contents of containers the player has not opened.
- The anticheat is off.

[Configuration overview](/docs/configuration/overview) explains where each setting lives.

## Roles and privileges

Stratum's commands each need a privilege. Roles with auto-grant, normally `admin`, receive the Stratum privileges that are registered on the server. Other roles receive none, so grant them what they need. Check a role with `/roles info <rolename>`. [Roles and privileges](/docs/commands/roles-and-privileges) lists each privilege and its command.
