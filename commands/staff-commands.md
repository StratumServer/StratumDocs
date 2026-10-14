---
title: "Staff commands"
description: "The main staff and admin commands, and the privilege each one needs."
category: "Commands"
categoryOrder: 3
order: 2
---

This page lists the main staff and admin commands Stratum adds. Most require a specific privilege set in `stratum-commands.json`. Run `/stratum access` to see which commands are enabled and what privilege each requires on your server.

Do not type the `[` and `]` in syntax examples. They mark optional arguments.

## Investigation

### /seen

```
/seen <player>
```

Shows when a player was last online. Works for offline players. If the player is online, shows how long they have been connected.

**Config key:** `Commands.Seen` | **Default privilege:** `stratum.seen`

### /whois

```
/whois <player>
```

Shows staff details for a player: UID, role, privilege count, first join, last join, current position (if online), ping, and whether they are vanished or frozen.

**Config key:** `Commands.Whois` | **Default privilege:** `stratum.whois`

### /near

```
/near [radius]
```

Lists players within the given radius in blocks. Excludes spectators and vanished players.

**Config key:** `Commands.Near` | **Default privilege:** `stratum.near`

## Location

### /back

```
/back
```

Teleports you to the position you were at before your last Stratum teleport, death, or jail move.

**Config key:** `Commands.Back` | **Default privilege:** `stratum.back`

### /setspawn

```
/setspawn
```

Sets the global server spawn point to your current position.

**Config key:** `Commands.SetSpawn` | **Default privilege:** `setspawn`

## Chat and messaging

### /msg, /tell, /w

```
/msg <player> <message>
```

Sends a private message to an online player.

**Config key:** `Commands.Message` | **Default privilege:** `stratum.msg`

### /reply, /r

```
/reply <message>
```

Replies to the last player who sent you a private message.

**Config key:** `Commands.Message`

### /staffchat

```
/staffchat <message>
```

Sends a message visible only to online players who have the `stratum.staffchat` privilege. Appears with a `[Staff]` prefix.

**Config key:** `Commands.StaffChat` | **Default privilege:** `stratum.staffchat`

### /staffbroadcast, /sbc

```
/staffbroadcast <message>
```

Broadcasts a highlighted message to all players across all chat groups.

**Config key:** `Commands.StaffBroadcast` | **Default privilege:** `stratum.staffbroadcast`

### /slowmode

```
/slowmode [seconds|off|status]
```

Sets a minimum interval in seconds between chat messages for all players. Use `0` or `off` to disable. A server-configured maximum limits how high it can be set.

**Config key:** `Commands.ChatControl` | **Default privilege:** `stratum.chatcontrol`

### /lockchat

```
/lockchat [on|off|toggle|status] [reason]
```

Locks or unlocks player chat. An optional reason is broadcast with the lock message.

**Config key:** `Commands.ChatControl`

### /chattoggle, /togglechat

```
/chattoggle [global|group|all|status] [on|off|toggle]
```

Enables or disables global or group chat channels. Use `all` to affect both at once.

**Config key:** `Commands.ChatControl`

### /chatclear, /clearchat

```
/chatclear [lines]
```

Clears visible chat history by sending blank lines. Uses the server default line count if no argument is given. Capped at 200 lines.

**Config key:** `Commands.ChatControl`

## Moderation

### /warn

```
/warn <player> <reason>
```

Records a warning with a reason and timestamp. The player is notified.

**Config key:** `Commands.Warn` | **Default privilege:** `stratum.warn`

### /warnings

```
/warnings <player>
```

Lists all warnings for a player with IDs, timestamps, actors, and reasons.

**Config key:** `Commands.Warn`

### /delwarn

```
/delwarn <player> <id> [reason]
```

Removes a warning by its ID from `/warnings`. The optional reason is logged.

**Config key:** `Commands.Warn`

### /mute

```
/mute <player> <duration> <reason>
```

Mutes a player. Duration format: `10m`, `2h`, `7d`, or `perm`/`permanent`/`forever`. A reason is required. A muted player cannot use global chat, private messages, or staffchat. The mute persists across reconnects.

**Config key:** `Commands.Mute` | **Default privilege:** `stratum.mute`

### /unmute

```
/unmute <player> [reason]
```

Removes an active mute immediately.

**Config key:** `Commands.Mute`

### /mutestatus

```
/mutestatus <player>
```

Shows whether a player is currently muted and when it expires.

**Config key:** `Commands.Mute`

### /setjail

```
/setjail
```

Sets the jail location to your current position. Must be done before `/jail` can be used.

**Config key:** `Commands.Jail` | **Default privilege:** `stratum.jail`

### /jail

```
/jail <player> [reason]
```

Teleports a player to the jail location and holds them there. Jailed players are returned to jail if they move away.

**Config key:** `Commands.Jail`

### /unjail

```
/unjail <player> [reason]
```

Releases a jailed player and returns them to their pre-jail position.

**Config key:** `Commands.Jail`

### /jailstatus

```
/jailstatus <player>
```

Shows whether a player is currently jailed.

**Config key:** `Commands.Jail`

### /note

```
/note <player> add <text>
/note <player> list
/note <player> delete <id>
```

Manages private staff notes for a player. Notes are not visible to the player.

**Config key:** `Commands.Notes` | **Default privilege:** `stratum.notes`

### /notes

```
/notes <player>
```

Lists all staff notes for a player with IDs, timestamps, and text.

**Config key:** `Commands.Notes`

### /report

```
/report <player> <reason>
```

Files a report against a player. Online staff with `stratum.reports` access are notified.

**Config key:** `Commands.Report` | **Default privilege:** `stratum.report`

### /reports

```
/reports [list|info|claim|close] [id] [resolution]
```

Manages the report queue.

- `/reports` or `/reports list`: shows open reports
- `/reports info <id>`: full details for one report
- `/reports claim <id>`: mark a report as being handled
- `/reports close <id> [resolution]`: close a report

**Config key:** `Commands.ReportManage` | **Default privilege:** `stratum.reports`

## Player state

### /freeze

```
/freeze <player> [on|off|toggle]
```

Freezes or unfreezes an online player. Without an argument, toggles. Accepts target selectors like `@staff`.

**Config key:** `Commands.Freeze` | **Default privilege:** `stratum.freeze`

### /revive

```
/revive <player>
```

Revives a dead online player in place without sending them to spawn. Useful for one-life event recovery. Accepts target selectors.

**Config key:** `Commands.Revive` | **Default privilege:** `stratum.revive`

### /vanish

```
/vanish [on|off|toggle|status]
/vanish hideothers [on|off|toggle]
```

Hides you from other players. Vanished players do not appear in the player list, on the map, or in `/near`. See Vanish for a full description of what is and is not hidden.

**Config key:** `Commands.Vanish` | **Default privilege:** `stratum.vanish`

## Lives system

### /lives

```
/lives [list|leaderboard]
/lives check <player>
/lives give <player|group:<id>> [amount]
/lives take <player|group:<id>> [amount]
/lives set <player|group:<id>> <amount>
```

Manages the optional lives system. The `group:<id>` form applies the operation to all members of a player group.

**Config key:** `Commands.Lives` | **Default privilege:** `stratum.lives`

## Class management

### /classrequests

```
/classrequests [list [status]]
/classrequests info <id>
/classrequests approve <id> [note]
/classrequests deny <id> [reason]
```

Manages class change requests submitted by players. See Class Changes.

**Config key:** `Commands.ClassChangeManage` | **Default privilege:** `stratum.classrequests`

## Kit management

### /kitedit

```
/kitedit list
/kitedit create <name>
/kitedit additem <name>
/kitedit removeitem <name> <index>
/kitedit preview <name>
/kitedit delete <name>
/kitedit rename <name> <newName>
/kitedit setrole <name> <role>
/kitedit setcooldown <name> <seconds>
/kitedit setscope <name> all|hotbar
/kitedit onrespawn <name> on|off
/kitedit oneperlife <name> on|off
/kitedit give <name> <player>
```

Creates and manages kits. See Kits for the full workflow.

**Config key:** `Commands.KitEdit` | **Default privilege:** `stratum.kitedit`

**Important:** `stratum.kitedit` is not granted to any role by default, including `admin`. Grant it explicitly.

## World and server controls

### /mobs

```
/mobs [status]
/mobs set <hostile|neutral|passive|all> <on|off>
/mobs scale <0..1>
/mobs wipe <hostile|neutral|passive|all> confirm
```

Controls natural mob spawning. `wipe confirm` removes all currently loaded creatures of the given category.

**Config key:** `Commands.MobSpawning` | **Default privilege:** `controlserver`

### /pvp

```
/pvp [on|off|status]
```

Shows or toggles global PvP.

**Config key:** `Commands.Pvp` | **Default privilege:** `stratum.pvp`

### /friendlyfire

```
/friendlyfire [on|off|toggle|status]
```

Toggles whether players in the same group can damage each other. See Moderation Workflows for when to use this.

**Config key:** `Commands.FriendlyFire` | **Default privilege:** `stratum.friendlyfire`

### /clearitems

```
/clearitems [now|cancel]
```

Clears all dropped items on the ground. Without an argument, starts a countdown broadcast. `now` clears immediately. `cancel` stops a pending countdown.

**Config key:** `Commands.ClearItems` | **Default privilege:** `stratum.clearitems`

### /restart

```
/restart <1-60>
/restart cancel
```

Schedules a server restart with countdown announcements. The delay is in minutes from 1 to 60.

**Config key:** `Commands.Restart` | **Default privilege:** `stratum.restart`

### /tps

```
/tps
```

Shows current TPS and milliseconds per tick. Requires `controlserver`.

### /roles

```
/roles list
/roles info <role>
/roles grant <role> <privilege>
/roles revoke <role> <privilege>
/roles applyall <role> [confirm]
```

Edits server roles at runtime. `applyall` shows a preview without `confirm`. Requires `controlserver`.

**Config key:** `Commands.RoleEditing`

## Group administration

### /group admin

Manages player groups with staff authority. See Groups for the full reference.

**Config key:** `Commands.GroupAdmin` | **Default privilege:** `manageotherplayergroups`

## Quick reference

| Command | What it does | Default privilege |
|---|---|---|
| `/seen <player>` | Last online time | `stratum.seen` |
| `/whois <player>` | Player details | `stratum.whois` |
| `/near [radius]` | Nearby players | `stratum.near` |
| `/back` | Return to last location | `stratum.back` |
| `/setspawn` | Set spawn to current position | `setspawn` |
| `/staffchat <msg>` | Staff-only chat | `stratum.staffchat` |
| `/staffbroadcast <msg>` | Broadcast to all | `stratum.staffbroadcast` |
| `/slowmode [seconds]` | Chat rate limit | `stratum.chatcontrol` |
| `/lockchat [on/off]` | Lock player chat | `stratum.chatcontrol` |
| `/chattoggle [global/group]` | Toggle chat channels | `stratum.chatcontrol` |
| `/chatclear [lines]` | Clear chat history | `stratum.chatcontrol` |
| `/warn <player> <reason>` | Issue a warning | `stratum.warn` |
| `/warnings <player>` | View warnings | `stratum.warn` |
| `/delwarn <player> <id>` | Remove a warning | `stratum.warn` |
| `/mute <player> <dur> <reason>` | Mute a player | `stratum.mute` |
| `/unmute <player>` | Remove a mute | `stratum.mute` |
| `/mutestatus <player>` | Check mute state | `stratum.mute` |
| `/setjail` | Set jail location | `stratum.jail` |
| `/jail <player>` | Jail a player | `stratum.jail` |
| `/unjail <player>` | Release from jail | `stratum.jail` |
| `/jailstatus <player>` | Check jail state | `stratum.jail` |
| `/note <player> add <text>` | Add a staff note | `stratum.notes` |
| `/notes <player>` | View staff notes | `stratum.notes` |
| `/report <player> <reason>` | File a report | `stratum.report` |
| `/reports` | Manage report queue | `stratum.reports` |
| `/freeze <player>` | Freeze/unfreeze | `stratum.freeze` |
| `/revive <player>` | Revive dead player | `stratum.revive` |
| `/vanish` | Toggle staff vanish | `stratum.vanish` |
| `/lives check <player>` | Check lives | `stratum.lives` |
| `/classrequests` | Manage class requests | `stratum.classrequests` |
| `/kitedit` | Manage kits | `stratum.kitedit` |
| `/mobs` | Mob spawning control | `controlserver` |
| `/pvp [on/off]` | Toggle PvP | `stratum.pvp` |
| `/friendlyfire [on/off]` | Toggle group friendly fire | `stratum.friendlyfire` |
| `/clearitems` | Clear ground items | `stratum.clearitems` |
| `/restart <minutes>` | Schedule restart | `stratum.restart` |
| `/tps` | Current TPS | `controlserver` |
| `/roles` | Edit roles | `controlserver` |
| `/group admin` | Group administration | `manageotherplayergroups` |
