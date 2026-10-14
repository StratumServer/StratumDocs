---
title: "Moderation workflows"
description: "The moderation tools Stratum adds, and a sensible order to use them in."
category: "Moderation"
categoryOrder: 5
order: 1
---

Stratum adds a set of moderation tools: warnings, mutes, jail, freeze, notes, and player reports. This page explains how they fit together. The commands and their syntax are in [Staff commands](/docs/commands/staff-commands). The privileges each tool needs are in [Roles and privileges](/docs/commands/roles-and-privileges).

Stratum does not enforce an order. The order below is a suggestion. Your server's rules decide what each step means.

## Start with a note or a warning

For a first concern, record it with `/note <player> <text>`. Read the notes later with `/notes <player>`. Notes are for staff only.

If the behavior needs a warning, use `/warn <player> <reason>`. Check a player's warnings with `/warnings <player>`, and remove a mistaken one with `/delwarn`.

## Mute when chat is the problem

`/mute <player> <duration>` stops a player from sending chat. Use `/unmute` to lift it, and `/mutestatus` to see the current mute. Durations use short forms such as `10m`, `2h`, or `7d`. `perm` keeps the mute until someone removes it.

## Jail or freeze to hold a player in place

`/setjail` sets the jail location. `/jail <player>` sends a player there, and `/unjail` releases them. `/jailstatus` shows who is jailed.

`/freeze <player>` stops a player from moving. Use it when you need the player still while you talk to them or check something.

## Handle reports from players

Players file reports with `/report`. Staff with the reports privilege see the queue with `/reports` or `/reports list`. The list shows open reports by default, and it takes a status filter.

Work a report in this order:

1. `/reports info <id>` to read the full report.
2. `/reports claim <id>` to mark it as yours, so two staff members do not work the same case.
3. `/reports close <id> [resolution]` to close it and record what you did.

## Keep a life count for events

`/lives` manages the lives that some events use. Kits can also be limited to once per life, for the same kind of event.

## Other tools

- `/vanish` hides a staff member from the player list, the map, and `/near`. Staff use it to watch without being seen.
- `/group admin` manages player groups, including locks and freezes. See [Groups](/docs/moderation/groups).
- `/friendlyfire` turns damage between group members on or off for the whole server.
- `/whois <player>` shows staff details for a player, such as their join history and current position.
- `/seen <player>` shows when a player was last online.
