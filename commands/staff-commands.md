---
title: "Staff commands"
description: "The main staff and admin commands, and the privilege each one needs."
category: "Commands"
categoryOrder: 3
order: 2
---

This page lists the main staff and admin commands Stratum adds. Most require a specific privilege set in `stratum-commands.json`. Run `/stratum access` to see which commands are enabled and what privilege each requires on your server.

`/revive`, `/restart`, and `/clearitems` do not appear in `/stratum access`, and roles with auto-grant do not receive them. Grant them explicitly with `/roles grant <role> <privilege>`, then check the result with `/roles info <role>`. A default admin role has `root`, so an in-game admin can grant a privilege they do not hold; the console also bypasses Stratum privilege checks.

Do not type the `[` and `]` in syntax examples. They mark optional arguments.

## Quick links

- [Investigation](#investigation)
- [Location](#location)
- [Chat and messaging](#chat-and-messaging)
- [Moderation](#moderation)
- [Player state](#player-state)
- [Lives system](#lives-system)
- [Class management](#class-management)
- [Kit management](#kit-management)
- [World and server controls](#world-and-server-controls)
- [Group administration](#group-administration)
- [Teleport and items](#teleport-and-items)
- [Stratum command](#stratum-command)
- [Quick reference](#quick-reference)

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

Replies to the last player with whom you exchanged a private message, whether they sent the last message or you did.

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
/slowmode [duration|off|disable|status]
```

Sets a minimum interval between chat messages for all players. Durations can be given in seconds or with units such as `10s`, `2m`, and `1h`. Use `off`, `disable`, or `0` to turn it off. The maximum defaults to one hour and can be changed with `Commands.SlowmodeMaxSeconds`; `0` removes the cap.

**Config key:** `Commands.ChatControl` | **Default privilege:** `stratum.chatcontrol`

### /lockchat

```
/lockchat [on|off|toggle|status] [reason]
```

Locks or unlocks player chat. An optional reason is broadcast with the lock message.

The lock and slowmode state resets when the server restarts. Players with `stratum.chatcontrol` bypass both controls and can bypass disabled chat channels when `AllowStaffBypass` is enabled. `/chattoggle` writes the channel state to config.

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

Clears visible chat history by sending blank lines. Without an argument it uses `Commands.ClearChatLines` (default 80). The argument is capped at that setting; the setting itself has a maximum of 200.

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

Mutes a player. Duration format accepts `10m`, `2h`, `7d`, `2w`, decimal durations such as `1.5h`, or `perm`/`permanent`/`forever`. A reason is required. A muted player cannot send messages to any chat channel. The mute persists across reconnects.

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

Releases a jailed player. If the player is online, was online when jailed, and `JailSettings.ReturnOnUnjail` is enabled, they return to their pre-jail position. An offline player remains at the jail location until they join and are released.

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

- `/reports` or `/reports list [open|claimed|closed|all]`: shows reports, open ones by default
- `/reports info <id>`: full details for one report
- `/reports claim <id>`: mark a report as being handled
- `/reports close <id> [resolution]`: close a report

The list shows at most 20 reports. Use `/reports list all` to include closed reports.

**Config key:** `Commands.ReportManage` | **Default privilege:** `stratum.reports`

## Player state

### /freeze

```
/freeze <player> [on|off|toggle]
```

Freezes or unfreezes an online player. Without an argument, toggles. Accepts target selectors like `@staff`, which selects players with the built-in `sumod`, `crmod`, or `admin` roles. Freeze state is kept in memory and ends when the player disconnects.

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

Hides you from other players. Vanished players do not appear in the player list, on the map, or in `/near`.

Vanish persists across reconnects and server restarts, but Stratum removes it if the player loses `stratum.vanish`. Other vanish holders can still see you unless they enable `hideothers`; that setting resets when they reconnect. The active-vanish reminder repeats every four seconds by default.

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

Manages bonus lives on top of the world's `playerlives` setting. The `group:<id>` or `group:<name>` form applies the operation to all members of a player group. `/lives give`, `/lives take`, and `/lives set` change the bonus, not the remaining lives; `playerlives = -1` means unlimited lives. Giving or setting a positive total can revive a dead online player. For example, `/lives give group:Vikings 2` gives two bonus lives to the group.

**Config key:** `Commands.Lives` | **Default privilege:** `stratum.lives`

## Class management

### /classrequests

```
/classrequests [list [status]]
/classrequests info <id>
/classrequests approve <id> [note]
/classrequests deny <id> [reason]
```

Manages class change requests submitted by players.

`/classrequests list` accepts `pending`, `approved`, `denied`, `cancelled`, or `all`. Lists show at most 20 entries. Online holders of `stratum.classrequests` receive a notice when pending requests are waiting.

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

Creates and manages kits.

**Config key:** `Commands.KitEdit` | **Default privilege:** `stratum.kitedit`

**Note:** roles with auto-grant (normally `admin`) receive `stratum.kitedit` automatically. Give it to other roles explicitly.

`create` snapshots your inventory, or only the hotbar when the kit scope is `hotbar`. `additem` takes the active hotbar slot. `setrole` toggles a role assignment on or off; a kit with no assigned role is available to everyone with `stratum.kits`. `setscope` changes what the next `create` captures, not the existing kit. See [Kits](/docs/moderation/kits) for the player behavior and scope details.

## World and server controls

### /mobs

```
/mobs [status]
/mobs set <hostile|neutral|passive|all> <on|off>
/mobs scale <0..1>
/mobs wipe <hostile|neutral|passive|all> confirm
```

Controls natural mob spawning. `/mobs set <category> on` also turns the master switch back on. `wipe confirm` removes all currently loaded creatures of the given category. Changes are saved to config.

**Config key:** `Commands.MobSpawning` | **Default privilege:** `controlserver`

### /pvp

```
/pvp [on|off|status]
```

Shows or toggles global PvP. A change updates `AllowPvP` in `serverconfig.json` and announces it to everyone.

**Config key:** `Commands.Pvp` | **Default privilege:** `stratum.pvp`

### /friendlyfire

```
/friendlyfire [on|off|toggle|status]
```

Toggles whether players in the same group can damage each other.

**Config key:** `Commands.FriendlyFire` | **Default privilege:** `stratum.friendlyfire`

### /clearitems

```
/clearitems [now|cancel]
```

Clears dropped items from loaded chunks. Without an argument, warns players and starts a countdown; the default warning is 30 seconds. `now` clears immediately. `cancel` stops a pending countdown.

**Config key:** `Commands.ClearItems` | **Default privilege:** `stratum.clearitems`

### /restart

```
/restart <1-60>
/restart cancel
```

Schedules a server shutdown with countdown announcements. The delay is in minutes from 1 to 60, and `/restart cancel` cancels it. A new `/restart <minutes>` replaces the pending countdown. The server exits when the countdown ends, so a service manager must start it again. By default, Stratum clears ground items before the shutdown.

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

Manages player groups with staff authority. Subcommands are `add`, `remove`, `lock`, `unlock`, `freeze`, `kind`, `tag`, `relation`, `info`, and `kinds`. The command requires `Groups.Enabled`; see [Groups](/docs/moderation/groups) for the syntax.

**Config key:** `Commands.GroupAdmin` | **Default privilege:** `manageotherplayergroups`

## Teleport and items

### /tp

```
/tp <source> <target>
```

Teleports one or more players or entities. `source` is a player name, a comma-separated list such as `luke,alice,bob`, or a selector: `@all`, `@staff`, `@admin`, `@mod`, or `@<role code>`. `target` is a player name or a position.

Examples: `/tp Luke Hayden`, `/tp luke,alice,bob hayden`, `/tp @staff hayden`.

**Default privilege:** `tp` (vanilla)

---

### /giveitem

```
/giveitem <item code> [quantity] [target] [attributes]
```

Gives items to a player. `quantity` defaults to 1. `target` accepts the same names and selectors as `/tp`. If you leave out the target, the items go to you.

**Default privilege:** `gamemode` (vanilla)

---

### /giveblock

```
/giveblock <block code> [quantity] [target] [attributes]
```

Gives blocks to a player, with the same arguments and target rules as `/giveitem`.

**Default privilege:** `gamemode` (vanilla)

---

### /uptime

```
/uptime
```

Shows how long the server has been running.

**Default privilege:** `chat`

---

## Stratum command

### /stratum

```
/stratum <action>
```

The main Stratum command. It needs `controlserver`. Actions include `status`, `version`, `update`, `health`, `reload`, `preflight`, `packets`, `performance`, `timings`, `players`, `player`, `chunks`, `entities`, `queues`, `pathfinding`, `doctor`, `regions`, `ac`, `access`, `chat`, `pregen`, `get`, `set`, and `save`.

Settings are read and changed with `/stratum get`, `/stratum set`, and `/stratum save`, as described in [Configuration](/docs/configuration/overview). Timings use `/stratum timings start`, `stop`, `reset`, and `report`. `/stratum access` shows which commands are enabled and the privilege each one needs.

---

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
| `/tp <source> <target>` | Teleport players or entities | `tp` |
| `/giveitem <item code> [quantity] [target]` | Give items | `gamemode` |
| `/giveblock <block code> [quantity] [target]` | Give blocks | `gamemode` |
| `/uptime` | Server uptime | `chat` |
| `/stratum <action>` | Stratum's main command | `controlserver` |
| `/roles` | Edit roles | `controlserver` |
| `/group admin` | Group administration | `manageotherplayergroups` |
