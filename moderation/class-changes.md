---
title: "Class changes"
description: "Let players change their character class, and how staff approve the changes players cannot make on their own."
category: "Moderation"
categoryOrder: 5
order: 4
---

`/class` lets a player change their character class in game. `/classrequests` lets staff approve or deny the changes a player cannot make on their own. Both work with the normal game client.

## Access

`/class` needs the `stratum.classchange` privilege. `/classrequests` needs `stratum.classrequests`. The built-in moderator roles, `sumod` and `crmod`, receive `stratum.classrequests` on a fresh server. On a server that already exists, nobody has `stratum.classchange`, so `/class` does nothing for players until you grant it: `/roles grant <rolename> stratum.classchange`. Check any role with `/roles info <rolename>`.

## How a change works

A player runs `/class change <class> [reason]`.

While the player has self-service changes left, the change happens at once, and one change is used up.

When none are left, the change becomes a request. Staff who can use `/classrequests` see it in chat, with the player's current class, the requested class, and the reason. Staff who join later see how many requests are waiting.

A player can have one open request at a time. A new request replaces the old one.

An approval changes the class at once if the player is online. If the player is offline, it applies at their next join. A denial is shown to the player the same way.

A player must have created a character before `/class change` works.

Changes made through the vanilla character dialog do not use up self-service changes, and the dialog keeps working as before.

When a class changes, the player keeps their appearance, and the new class's starting gear is not handed out. This matches a vanilla reselection outside creative mode.

## Config

In `stratum-commands.json`, under `Commands.ClassChangeSettings`:

| Key | Default | Meaning |
| --- | --- | --- |
| `SelfServiceChanges` | `0` | Changes a player may make before a request is needed. `-1` means unlimited. `0` means every change is a request. |
| `MaxReasonLength` | `200` | The longest reason a player may give, from 1 to 1000 characters. |

You can change both on a running server with `/stratum set`, for example `/stratum set Commands.ClassChangeSettings.SelfServiceChanges 1`.

## Player commands

```
/class
/class list
/class change <class> [reason]
/class cancel
```

`/class` alone shows your class, your remaining self-service changes, and your open request. `/class list` shows the class codes. The class is a code from that list, in any case. `/class cancel` withdraws your open request.

## Staff commands

```
/classrequests
/classrequests list [pending|approved|denied|cancelled|all]
/classrequests info <id>
/classrequests approve <id> [note]
/classrequests deny <id> [reason]
```

`/classrequests` alone lists pending requests, newest first. Pass a status to list other requests. `info` shows one request in full, including who settled it.

`approve` and `deny` settle a request, and the note or reason is shown to the player. A request can be settled only once. If the requested class no longer exists on the server, approving it fails, so deny it instead.

Every change, request, approval, denial, and withdrawal is written to the audit log.
