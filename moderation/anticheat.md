---
title: "Anticheat"
description: "What the Stratum anticheat checks, how it escalates, and how to turn it on."
category: "Moderation"
categoryOrder: 5
order: 5
---

The Stratum anticheat checks the server side of player actions for impossible behavior: reaching too far, breaking blocks too fast, moving too far or too fast, and similar cases. It is off by default.

When it is on, it records each violation. Staff see an alert when a player reaches the alert threshold. Recent violations drop out of the alert window after a while, but the punishment count does not. See Punishments below.

## Turn it on

Turn the anticheat on with `Anticheat.Enabled` in `stratum.json`. Each check has its own switch and its own threshold under `Anticheat`. Start with the checks you need, and watch the staff alerts for a few days before you turn on punishments.

```
/stratum set Anticheat.Enabled true
```

## What it checks

| Check | Looks for |
| --- | --- |
| Block reach | Breaking or placing blocks from further away than a player can reach. |
| Block entity range | Interacting with a block entity, such as a chest, from too far away. |
| Entity reach | Attacking or interacting with an entity from too far away. |
| Block break progress | Breaking blocks faster than the tools allow. |
| Movement | Moving too fast, flying, walking on water, or moving through blocks. |
| Position checks | Stepping up a block without jumping, or a sudden jump in horizontal speed between two position updates. |
| Combat | Attacks that do not fit normal play. This check starts in monitor mode, so staff review its data with `/stratum ac` before stricter action is turned on. |
| No fall | Taking no fall damage after a long drop. |
| Multi break | Breaking several blocks in one action. |

Each check has a slack value. The slack is the margin of error that lag and normal play can create.

## Staff alerts

`Anticheat.StaffAlerts` is `true` by default. Online staff see an alert when a player passes the alert threshold. The alert repeats after a set interval, so staff are reminded while a problem continues.

## Punishments

Punishments are off by default, under `Anticheat.Punishments`. They escalate with each player's **accumulated** violation count. That count is kept across sessions and does not expire. It only grows while punishments are on. Undoing a punishment does not reset it. Each step is applied at most once. A burst of violations can pass several thresholds at once and skip the steps in between.

| Step | Default threshold |
| --- | --- |
| Drop the player's inventory | 15 violations |
| Freeze | 25 violations |
| Jail | 40 violations |
| Ban | 60 violations, for 720 hours (30 days) |

The server can also wipe the inventory instead of dropping it, with `WipeInsteadOfDrop`. A wipe deletes the items, and nothing brings them back. Leave it `false`, which is the default, unless you have decided to accept that risk for a false positive. Most checks can kick a player for repeated violations. Kicks are off by default, through `KickConfirmedCheats`. The block entity range check has no kick setting. One exception: the multi-break check can kick at once when a burst of block breaks has the exact centre-hit pattern of a nuker. That is `MultiBreak.KickOnFingerprint`, which is on by default.

The thresholds must rise in order: freeze is never lower than the drop threshold, jail never lower than freeze, and ban never lower than jail. If you raise one, the ones after it rise with it. These thresholds are defaults, not advice. A busy server may want higher numbers, and a server with few players may want lower ones. Review the staff alerts before you rely on punishments.

## False positives

A check can flag normal play, especially after lag, teleports, or fast mounts. Staff should look at the alerts before taking action. The `/whois` and `/seen` commands help with that. If a player is punished by mistake, undo it with the usual moderation tools, and send the report to the Stratum team so the check can be fixed.
