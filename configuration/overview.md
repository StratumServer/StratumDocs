---
title: "Configuration"
description: "The Stratum config files and how to change settings."
category: "Configuration"
categoryOrder: 2
order: 1
---

Stratum keeps its own config, separate from the vanilla server config. The files are created with defaults the first time the server runs.

## The files

Stratum writes three files into the game config folder:

- `stratum.json` is the main config.
- `stratum-commands.json` holds command settings.
- `stratum-performance.json` holds the performance settings.

## Changing settings

You have two ways to change settings.

Edit the file directly, then apply it:

1. Edit `stratum.json` or one of the sidecar files.
2. Run `/stratum reload` in-game to load the changes.

Some settings only take effect at startup, so a full restart is the safe choice after larger changes.

Or work in-game:

- Read a setting with `/stratum get`.
- Change one with `/stratum set`.
- Write the current settings to disk with `/stratum save`.

## What lives in the config

The main config is split into sections. The ones you are most likely to touch:

- Chat, Theme, and Nametags for chat formatting, join and leave messages, and role tags
- PacketLimits, PacketBackPressure, and BlockBreakGuards for server hardening
- LoginProtection and ClientModPolicy for join-time protection
- PlayerPrivacy for map and location privacy
- Performance for chunk, entity, and save behavior
- Diagnostics for logging and preflight

See [Server presentation](/docs/configuration/server-presentation) for the chat and message settings most servers set first.
