---
title: "Groups"
description: "Staff tools for player groups: adding players, locks, roster freezes, kinds, relations, and tags."
category: "Moderation"
categoryOrder: 5
order: 2
---

`/group admin` gives staff control over player groups that vanilla leaves to the players. Staff can add a player to a group without an invite, pin a player in place, freeze a group's roster for an event, label groups by kind, record how two groups relate, and show a tag in chat and on nametags.

Nothing changes until someone uses these commands. A server that never runs `/group admin` behaves as it did before.

## Access

`/group admin` needs the `manageotherplayergroups` privilege, and the `controlplayergroups` privilege of the vanilla `/group` command. Changing `Commands.GroupAdmin.Privilege` can narrow access, but it cannot open the subcommand to ordinary players.

## Commands

```
/group admin add <group> <player> [access]
/group admin remove <group> <player>
/group admin lock <group> <player> [duration]
/group admin unlock <group> <player>
/group admin freeze <group> [on|off]
/group admin kind <group> [kind|none]
/group admin tag <group> [text|none]
/group admin relation <group> <other group> [ally|enemy|neutral]
/group admin info <group>
/group admin kinds
```

`<group>` is the group name or its number. `<player>` can be offline, as long as the player has joined once.

`access` sets the role inside the group: `1` for Member (the default), `2` for Op, `3` for Owner. Use `remove` to take a player out. Access `0` is refused, because it is not a membership.

`duration` is `perm` (until staff unlock it), or a time such as `30s`, `90m`, `4h`, `3d`, or `2w`.

Every change is written to the audit log with the name of the staff member who made it.

## Locks

`/group admin lock` pins a player into a group. A locked player cannot leave the group, and a group operator cannot kick them out. The owner cannot disband a group that has a locked member. Staff can still remove the player, and removing someone clears their lock.

Locks survive a restart and apply to offline players. A timed lock ends the next time it is checked.

## Roster freeze

`/group admin freeze <group> on` stops the roster from changing. Without `on` or `off`, the command toggles the freeze. No one can join, be invited, leave, be kicked, or disband the group. Staff commands still work. Use a freeze for the length of an event. Use locks when you only need to hold one player.

## Kinds

A kind is a label on a group that controls how it behaves in combat. Kinds are defined in `stratum.json` under `Groups.Kinds`, and you assign one to a group with `/group admin kind`. A group with no kind is unclassified.

| Kind setting | Meaning |
| --- | --- |
| `Code` | The name you use with `/group admin kind`. |
| `Exclusive` | A player can hold at most one group of this kind. Two different exclusive kinds do not conflict. |
| `MaxMembers` | The member limit. `0` means no limit. Players are refused at the limit. Staff adds go past it, and the audit log records that. |
| `Lockable` | Whether locks work on groups of this kind. |
| `CountsAsSameSide` | Whether sharing a group of this kind makes two players allies for friendly fire. |
| `PlayerCreatable` | Whether `/group create` can produce this kind. |
| `Tag` | The tag for groups of this kind that have no tag of their own. |
| `TagPriority` | When a player holds several tagged groups, the highest value wins. |

`CountsAsSameSide` matters for administrative groups. Before kinds existed, any shared group turned friendly fire off between its members. An event sign-up group could switch off PvP for everyone in it. Give such a group a kind with `CountsAsSameSide` set to `false`, and it stops doing that. The default is `true`, so nothing changes until you set it.

Kinds and relations affect combat only. Map privacy still treats two players in the same group as able to see each other.

## Relations

`/group admin relation <group> <other> ally|enemy|neutral` records how two groups stand toward each other. The relation is stored on both groups, so it reads the same from either side.

When `Groups.AlliesCountAsSameSide` is `true`, an ally relation turns friendly fire off between the two groups, as long as their kinds count as the same side. When a player holds several groups, an enemy relation between any two of them overrides an ally relation. That lets you carve one pairing out of a wider alliance.

## Tags

`/group admin tag <group> <text>` puts a tag in front of the group's members in chat and on nametags. A tagged staff member might read `[RED] [Admin] Alice`. Use `none` to remove a tag.

## Config

All of these are in `stratum.json`, under `Groups`:

| Key | Default | Effect |
| --- | --- | --- |
| `Enabled` | `true` | Turns group administration on or off. |
| `DefaultKind` | `null` | The kind given to groups created with `/group create`. |
| `RelationsEnabled` | `true` | Turns `/group admin relation` on or off. |
| `AlliesCountAsSameSide` | `true` | Whether allied groups count as one side for friendly fire. |
| `ShowStateToPlayers` | `true` | Whether `/group info` shows the kind, tag, roster state, and relations to ordinary players. Set it to `false` to show that only to staff. |
| `ShowTagInChat` | `true` | Shows group tags in chat. |
| `ShowTagInNametag` | `true` | Shows group tags on nametags. |
| `TagFormat` | `[{tag}]` | How the tag is wrapped. It must contain `{tag}`. |
| `TagColor` | `null` | The tag color, as a color name or `#rrggbb`. |
| `MaxTagLength` | `8` | The longest tag `/group admin tag` accepts. |
