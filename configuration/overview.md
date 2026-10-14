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
- `stratum-commands.json` holds the command settings: which commands are on, and the privilege each one needs.
- `stratum-performance.json` holds the performance settings.

[Config reference](/docs/configuration/config-reference) lists the main settings and their defaults.

## Changing settings in-game

Use these commands while the server runs:

- `/stratum get <path>` reads a setting, for example `/stratum get Performance.Pregen.Enabled`.
- `/stratum set <path> <value>` changes a setting.
- `/stratum save` writes the current settings to all three files.
- `/stratum reload` reads the three files again and applies them.

## Changing settings in the files

Edit the file, then run `/stratum reload` in-game. Without the reload, the edit is still only on disk.

Stratum keeps the settings in memory. Several actions write all three files from memory, including `/stratum set`, `/stratum save`, and the `/friendlyfire` and `/pvecombat` toggles. An edit that you have not reloaded is overwritten at the next write.

Some settings take effect only at startup, such as the Windows timer resolution. Restart the server after a change to one of them.

## Sections

The main config is split into sections. These are the ones most servers touch:

- `Appearance` and `Chat` for chat formatting, join and leave messages, role tags, and nametags. [Server presentation](/docs/configuration/server-presentation) explains them.
- `Hardening`, `PacketLimits`, `PacketBackPressure`, and `BlockBreakGuards` for server protection.
- `LoginProtection` and `ClientModPolicy` for join-time checks.
- `PlayerPrivacy` for map pins and coordinates.
- `Performance` for chunk, entity, and save behavior. [Performance tuning](/docs/configuration/performance-tuning) and [Entity tick throttling](/docs/configuration/entity-ticking) cover it.
- `Diagnostics` for startup logging and preflight checks.
- `Backup` for scheduled world backups. [Backups and restore](/docs/configuration/backups-and-restore) explains them.
