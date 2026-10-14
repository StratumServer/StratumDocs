---
title: "Anticheat"
description: "What the Stratum anticheat checks, how it escalates, and how to turn it on."
category: "Moderation"
categoryOrder: 5
order: 5
---

The Stratum anticheat checks the server side of player actions for impossible behavior: reaching too far, breaking blocks too fast, moving too far or too fast, and similar cases. It is off by default.

When it is on, it records each violation. Staff see an alert when a player reaches the alert threshold. A player's violations are kept for a while, and then they expire.

## Turn it on

Turn the anticheat on with `Anticheat.Enabled` in `stratum.json`. Each check has its own switch and its own threshold under `Anticheat`. Start with the checks you need, and watch the staff alerts for a few days before you turn on punishments.

```
/stratum set Anticheat.Enabled true
```

## What it checks

| Check | Looks for |
| --- | --- |
| Block reach | Breaking or placing blocks from further away than a player can reach. |
| Entity reach | Attacking or interacting with an entity from too far away. |
| Block break progress | Breaking blocks faster than the tools allow. |
| Movement | Moving too far or too fast, including step height and acceleration. |
| Movement authority | Positions that do not match the server's record of where the player is. |
| Combat | Attacks that do not fit the player's state. |
| No fall | Taking no fall damage after a long drop. |
| Multi break | Breaking several blocks in one action. |

Each check has a slack value. The slack is the margin of error that lag and normal play can create.

## Staff alerts

`Anticheat.StaffAlerts` is `true` by default. Online staff see an alert when a player passes the alert threshold. The alert repeats after a set interval, so staff are reminded while a problem continues.

## Punishments

Punishments are off by default, under `Anticheat.Punishments`. They escalate with the number of confirmed violations:

| Step | Default threshold |
| --- | --- |
| Drop the player's inventory | 15 violations, if `DropInventoryAfterFlags` applies |
| Freeze | 25 violations |
| Jail | 40 violations |
| Ban | 60 violations, for 720 hours (30 days) |

The server can also wipe the inventory instead of dropping it, with `WipeInsteadOfDrop`. Each check can kick the player for repeated violations. Kicks are off by default, through `KickConfirmedCheats`, and they apply after a set number of violations.

These thresholds are defaults, not advice. A busy server may want higher numbers, and a server with few players may want lower ones. Review the staff alerts before you rely on punishments.

## False positives

A check can flag normal play, especially after lag, teleports, or fast mounts. Staff should look at the alerts before taking action. The `/whois` and `/seen` commands help with that. If a player is punished by mistake, undo it with the usual moderation tools, and send the report to the Stratum team so the check can be fixed.
