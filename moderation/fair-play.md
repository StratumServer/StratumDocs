---
title: "Fair play"
description: "What the Stratum anticheat checks, in plain words, and a rules text you can copy for your players."
category: "Moderation"
categoryOrder: 5
order: 6
---

Players read the server rules with `/rules`. The text comes from `ServerInfo.Rules` in `stratum.json`. This page gives you a rules text that matches how the anticheat works, and it explains the anticheat in words your players can follow.

## What the anticheat checks

The anticheat is off by default. Turn it on with `Anticheat.Enabled`. When it is on, the server checks these things:

- **Reach:** whether a player breaks, places, or attacks from further away than the game allows.
- **Block breaking speed:** whether blocks break faster than the tools allow.
- **Movement:** whether a player moves too fast, flies, walks on water, or moves through blocks.
- **Positions:** stepping up a block without jumping, or a sudden jump in horizontal speed between two position updates.
- **Fall damage:** taking no damage after a long fall.
- **Combat:** attacks that do not fit normal play. The combat check starts in monitor mode, so staff review its data before stricter action is turned on.
- **Several blocks at once:** breaking many blocks in one action, such as a nuker or veinmine.

Each check allows some margin for lag and normal play. Staff alerts start only after repeated violations inside a short time window, so one odd event does not raise an alert.

## What happens after a violation

Staff alerts are on by default, so online staff can look at a player who keeps triggering checks. Punishments are a separate setting, and they are off by default. A server that turns them on can drop a player's inventory, freeze, jail, or ban the player after a set number of violations. Kicks are off by default too, with one exception: a nuker caught by the multi-break check can be removed at once, with no warning, through `MultiBreak.KickOnFingerprint`, which is on by default.

A violation count does not expire, so a player who is punished by mistake can still reach the next step later. Staff can review the alerts and undo a punishment, but the count stays.

## If you think a flag was wrong

Tell a staff member and give the time and place. Staff can check the player's history with `/whois` and `/seen`. Do not try to hide a flag or argue with the alert in chat. A false flag is fixed by staff, not by the player.

## Rules text to copy

Set this in `stratum.json` under `ServerInfo`, as the `Rules` value, or with `/stratum set ServerInfo.Rules`. Change the bracketed parts to match your server:

```
Rules for <server name>:
1. Play fair. Do not use mods, scripts, or exploits that give you an advantage over other players.
2. Do not grief, steal from other players, or break their builds.
3. Be respectful in chat. No harassment, threats, or hate speech.
4. Report a bug you find through <how to report it>, and do not abuse it.
5. Staff decisions can be appealed by contacting <contact>.
```

If your server uses the anticheat, say so in the rules, so players know that movement and reach are checked.
