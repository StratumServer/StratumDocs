---
title: "Roles and privileges"
description: "How Stratum commands are gated by privilege, and how to grant them to roles."
category: "Commands"
categoryOrder: 3
order: 3
---

Stratum commands each use their own named privilege rather than a single shared gate. This lets you give players `/spawn` without giving them `/vanish`, and give moderators `/warn` without giving them `/restart`.

## How privileges work

Each command has a config block in `stratum-commands.json` with a `Privilege` field. At startup Stratum registers that privilege name so it appears in `/list privileges`. Most commands check both the built-in Vintage Story privilege gate and the Stratum-specific privilege. Some commands, including `/roles` and `/mobs`, have a built-in `controlserver` gate that cannot be opened to ordinary players by changing their Stratum `Privilege` setting. The server console bypasses Stratum privilege checks.

To grant a Stratum privilege to a role:
```
/roles grant <rolename> <privilege>
```

To grant it to one player directly:
```
/player <playername> privilege grant <privilege>
```

To check what a role has:
```
/roles info <rolename>
```

To see all registered Stratum privileges:
```
/list privileges
```

To see which commands are enabled and what privilege each requires on this server:
```
/stratum access
```

This command itself requires `controlserver`, like the other `/stratum` actions.

Use `/stratum access command <command>` to inspect one command, or `/stratum access role <role>` to list the commands a role can use.

`/list privileges` requires the vanilla `readlists` privilege. `/player <name> privilege grant <privilege>` requires `grantrevoke`.

## First-time setup: giving your staff role access

On a fresh server, identify your staff role. The built-in moderator roles are `sumod` and `crmod`. Replace `<staffrole>` below with the role your moderators use, and grant it the privileges they need:

```
/roles grant <staffrole> stratum.warn
/roles grant <staffrole> stratum.mute
/roles grant <staffrole> stratum.jail
/roles grant <staffrole> stratum.freeze
/roles grant <staffrole> stratum.vanish
/roles grant <staffrole> stratum.whois
/roles grant <staffrole> stratum.notes
/roles grant <staffrole> stratum.reports
/roles grant <staffrole> stratum.chatcontrol
/roles grant <staffrole> stratum.staffchat
/roles grant <staffrole> stratum.staffbroadcast
/roles grant <staffrole> stratum.classrequests
```

The privileges above cover the moderation tools in the staff commands page.

## Player privileges

Grant these to the roles your players use. On a fresh server, the base player role is `suplayer`. Check a role's current grants with `/roles info <rolename>`.

| Privilege | Commands | Config key |
|---|---|---|
| `stratum.spawn` | `/spawn` | `Commands.Spawn` |
| `stratum.wilderness` | `/wilderness` | `Commands.Wilderness` |
| `stratum.tpa` | `/tpa`, `/tpaccept`, `/tpdeny`, `/tpacancel` | `Commands.TeleportRequests.Request` |
| `stratum.tpahere` | `/tpahere` | `Commands.TeleportRequests.Here` |
| `stratum.home` | `/home`, `/homes` | `Commands.Homes.Home` |
| `stratum.sethome` | `/sethome` | `Commands.Homes.SetHome` |
| `stratum.delhome` | `/delhome` | `Commands.Homes.DeleteHome` |
| `stratum.back` | `/back` | `Commands.Back` |
| `stratum.msg` | `/msg`, `/tell`, `/w`, `/reply` | `Commands.Message` |
| `stratum.near` | `/near` | `Commands.Near` |
| `stratum.seen` | `/seen` | `Commands.Seen` |
| `stratum.info` | `/rules`, `/discord`, `/website`, `/motd` | `Commands.InfoCommands` |
| `stratum.report` | `/report` | `Commands.Report` |
| `stratum.classchange` | `/class` | `Commands.ClassChange` |
| `stratum.kits` | `/kit` | `Commands.Kits` |

**Note:** roles with auto-grant (normally `admin`) receive the Stratum privileges registered on the server automatically, including `stratum.kits`. Give kit access to other roles explicitly. Grant `stratum.report` to player roles that should be able to file reports.

## Staff privileges

Grant these to your staff roles.

| Privilege | Commands | Config key |
|---|---|---|
| `stratum.whois` | `/whois` | `Commands.Whois` |
| `stratum.staffchat` | `/staffchat` | `Commands.StaffChat` |
| `stratum.staffbroadcast` | `/staffbroadcast`, `/sbc` | `Commands.StaffBroadcast` |
| `stratum.chatcontrol` | `/slowmode`, `/lockchat`, `/chattoggle`, `/chatclear` | `Commands.ChatControl` |
| `stratum.vanish` | `/vanish` | `Commands.Vanish` |
| `stratum.freeze` | `/freeze` | `Commands.Freeze` |
| `stratum.revive` | `/revive` | `Commands.Revive` |
| `stratum.jail` | `/setjail`, `/jail`, `/unjail`, `/jailstatus` | `Commands.Jail` |
| `stratum.warn` | `/warn`, `/warnings`, `/delwarn` | `Commands.Warn` |
| `stratum.mute` | `/mute`, `/unmute`, `/mutestatus` | `Commands.Mute` |
| `stratum.notes` | `/note`, `/notes` | `Commands.Notes` |
| `stratum.reports` | `/reports` | `Commands.ReportManage` |
| `stratum.lives` | `/lives` | `Commands.Lives` |
| `stratum.classrequests` | `/classrequests` | `Commands.ClassChangeManage` |
| `stratum.kitedit` | `/kitedit` | `Commands.KitEdit` |
| `stratum.pvp` | `/pvp` | `Commands.Pvp` |
| `stratum.friendlyfire` | `/friendlyfire` | `Commands.FriendlyFire` |
| `stratum.clearitems` | `/clearitems` | `Commands.ClearItems` |
| `stratum.restart` | `/restart` | `Commands.Restart` |
| `stratum.coopcombat` | `/pvecombat` | `Commands.CoopCombat` |

**Note:** `stratum.kitedit` is registered like every other Stratum privilege, so roles with auto-grant (normally `admin`) receive it. Give it to other roles explicitly.

`stratum.revive`, `stratum.restart`, and `stratum.clearitems` are not in the access list that `/stratum access` shows, and roles with auto-grant do not receive them. Grant them explicitly if staff need them. A default admin role has `root`, which lets an in-game admin grant a privilege even when they do not hold it; the server console also bypasses Stratum privilege checks.

## Admin privileges

These use the built-in Vintage Story `controlserver` privilege or a vanilla equivalent.

| Privilege | Commands | Config key |
|---|---|---|
| `controlserver` | `/roles` | `Commands.RoleEditing` |
| `controlserver` | `/mobs` | `Commands.MobSpawning` |
| `setspawn` | `/setspawn` | `Commands.SetSpawn` |
| `manageotherplayergroups` | `/group admin` | `Commands.GroupAdmin` |

`/tps` uses `controlserver` directly and has no config key.
The `/stratum` command also requires `controlserver` and has no per-command config block.
`/group admin` also requires the vanilla `controlplayergroups` privilege, and the command returns an error while `Groups.Enabled` is `false`.

Non-root callers can edit only roles below their own role level. They also cannot grant a privilege they do not hold. Root callers and the server console bypass these checks. `/roles applyall` skips players who already hold a staff role and cannot be used to overwrite staff roles. Successful changes save to `serverroles.json` and refresh online players.

## Other Stratum privileges

| Privilege | Config key | Purpose |
|---|---|---|
| `stratum.privacy.bypass` | `PlayerPrivacy.StaffOverride` | Lets staff see real map pins when `PlayerPrivacy` is enabled. Not registered on the current release, so grant it explicitly |
| `stratum.combatlog.exempt` | `CombatLog.ExemptPrivilege` | Exempts a player from combat-log tagging. This privilege is not registered automatically, so grant it explicitly |

## Changing a command's privilege

To restrict `/spawn` to a custom privilege:

```
/roles grant <rolename> stratum.spawnaccess
/stratum set Commands.Spawn.Privilege stratum.spawnaccess
/stratum reload
```

`/stratum set` saves the config immediately. A new privilege name is registered after `/stratum reload` or a restart, which is also when an auto-grant role receives it. To change list values such as `AllowModIds`, edit the JSON file and reload it; lists cannot be set with `/stratum set`.

To disable a command entirely:
```
/stratum set Commands.Spawn.Enabled false
```

To remove a Stratum privilege requirement, set `Privilege` to an empty string in `stratum-commands.json`, then run `/stratum reload`. `/stratum set` cannot set an empty value. This removes only the Stratum-specific gate; `/roles`, `/mobs`, and `/stratum` still require the built-in `controlserver` privilege, and other commands still require their built-in gate, usually `chat`.

Each command access block can also set `CooldownSeconds` and `CooldownBypassForStaff`. The defaults are `0` and `true`. The bypass applies to holders of `stratum.staffchat`, regardless of their role name. For example:

```json
"Spawn": {
  "Enabled": true,
  "Privilege": "stratum.spawn",
  "CooldownSeconds": 0,
  "CooldownBypassForStaff": true
}
```

Changing a command's `Enabled` value to `false` blocks it at runtime. Re-enabling a command that was disabled when the server started requires a restart so Stratum registers it again.
