---
title: "Player commands"
description: "Commands Stratum adds for players, and the privilege each one needs."
category: "Commands"
categoryOrder: 3
order: 1
---

This page lists the commands Stratum adds for regular players. Your server may have some of these disabled or may require a specific role to use them. Run `/stratum access` (if you have access to it) or ask a staff member to see what is available on your server.

Do not type the `[` and `]` in syntax examples. They mark optional arguments.

## Teleportation

### /spawn

```
/spawn
```

Teleports you to the server spawn point. If the server has a teleport warmup, stand still until the teleport fires.

### /wilderness

```
/wilderness
```

Teleports you to a random unclaimed location. The radius is set by the server.

### /tpa, /tpaccept, /tpdeny, /tpacancel

```
/tpa <player>
/tpa accept
/tpa decline
/tpa cancel
```

Sends a teleport request to another player asking to teleport to them. The target gets a prompt and a configurable number of seconds to respond.

- `/tpa accept` or `/tpaccept`: accept a request sent to you
- `/tpa decline`, `/tpa deny`, `/tpdeny`, or `/tpdecline`: decline a request
- `/tpa cancel` or `/tpacancel`: cancel your own outgoing request

### /tpahere

```
/tpahere <player>
```

Asks another player to teleport to you. They accept or decline the same way as a normal `/tpa`.

### /home, /homes, /sethome, /delhome

```
/home [name]
/homes
/sethome [name]
/delhome [name]
```

Save and use personal home locations.

- `/home` or `/home <name>`: teleport to a saved home. Without a name, uses the home named `home` or shows your list if you have multiple.
- `/homes`: list all your saved homes with coordinates.
- `/sethome [name]`: save your current position. Without a name, saves as `home`.
- `/delhome [name]`: delete a home.

Home names may only contain letters, numbers, hyphens, and underscores (maximum 32 characters). The server sets a limit on how many homes you can save.

### /back

```
/back
```

Teleports you to the position you were at before your last Stratum teleport or death. Stratum records teleports (spawn, home, tpa, wilderness) and deaths.

## Communication

### /msg, /tell, /w

```
/msg <player> <message>
/tell <player> <message>
/w <player> <message>
```

Sends a private message to an online player. Only the sender and recipient see it.

### /reply, /r

```
/reply <message>
/r <message>
```

Replies to the last player who messaged you or the last player you messaged.

## Information

### /near

```
/near [radius]
```

Lists players within the given radius in blocks. Uses the server's default radius if you omit it.

### /seen

```
/seen <player>
```

Shows when a player was last online. Works for offline players.

### /uptime

```
/uptime
```

Shows how long the server has been running since the last restart.

### /rules, /discord, /website, /motd

```
/rules
/discord
/website
/motd
```

Shows the server's configured rules, Discord link, website URL, or message of the day. Returns an error if the server has not configured that text.

## Character

### /class

```
/class
/class list
/class change <class> [reason]
/class cancel
```

Manages your character class.

- `/class` or `/class info`: shows your current class and any open change request.
- `/class list`: shows available class codes.
- `/class change <class> [reason]`: changes your class or submits a request for staff approval.

**Default behavior:** The server default requires staff approval for every class change (`SelfServiceChanges = 0`). When you run `/class change`, it submits a request rather than changing immediately. A staff member approves or denies it. The class changes at once if you are online, or on your next join if you are offline.

If the server grants self-service changes, the class changes immediately and one use is deducted. Check how many you have with `/class`.

## Kits

### /kit

```
/kit
/kit <name>
```

- `/kit`: lists the kits assigned to you with their cooldowns.
- `/kit <name>`: redeems a kit. Items go directly into your inventory. Overflow is dropped at your feet.

A kit may have a cooldown, a once-per-life limit, or a role requirement. The `/kit` list shows what applies to each kit.

**Note:** roles with auto-grant (normally `admin`) receive `stratum.kits` automatically. If `/kit` is not available to you, ask a server admin to grant your role access.

## PvE combat

### /pvecombat

```
/pvecombat [on|off|toggle|status]
```

Shows or toggles cooperative PvE mode. When on, creature hit invulnerability is reduced so multiple players can hit the same target at once. This is a server-wide setting, not per-player. Requires the `stratum.coopcombat` privilege.

## Quick reference

| Command | What it does | Default privilege |
|---|---|---|
| `/spawn` | Go to spawn | `stratum.spawn` |
| `/wilderness` | Random teleport | `stratum.wilderness` |
| `/tpa <player>` | Request teleport to a player | `stratum.tpa` |
| `/tpahere <player>` | Ask a player to come to you | `stratum.tpahere` |
| `/home [name]` | Go to a saved home | `stratum.home` |
| `/homes` | List saved homes | `stratum.home` |
| `/sethome [name]` | Save a home | `stratum.sethome` |
| `/delhome [name]` | Delete a home | `stratum.delhome` |
| `/back` | Return to last location | `stratum.back` |
| `/msg <player> <message>` | Private message | `stratum.msg` |
| `/reply <message>` | Reply to last PM | `stratum.msg` |
| `/near [radius]` | List nearby players | `stratum.near` |
| `/seen <player>` | When was a player online | `stratum.seen` |
| `/uptime` | How long the server has been up | `chat` |
| `/rules` | Show server rules | `stratum.info` |
| `/discord` | Show Discord link | `stratum.info` |
| `/website` | Show website | `stratum.info` |
| `/motd` | Show message of the day | `stratum.info` |
| `/class` | View your class info | `stratum.classchange` |
| `/class change <class>` | Change or request a class change | `stratum.classchange` |
| `/kit [name]` | List or redeem kits | `stratum.kits` |
| `/pvecombat` | Toggle coop PvE mode | `stratum.coopcombat` |
