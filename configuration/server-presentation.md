---
title: "Server presentation"
description: "Chat formatting, info commands, join messages, and nametags."
category: "Configuration"
categoryOrder: 2
order: 2
---

Stratum can handle the presentation pieces most servers want. Chat, join and leave messages, and role tags live in the `Chat` and `Appearance` sections of `stratum.json`. The info command text lives in `ServerInfo`.

## Chat

Chat is on by default. Global and group chat each have their own switch, and join and leave messages can be shown or hidden.

Chat rate limiting is also on by default. A player can send one message every 750 milliseconds (`Chat.RateLimit.MinimumIntervalMs`). Identical messages from the same player inside a 3 second window are dropped (`Chat.RateLimit.DropDuplicates` and `Chat.RateLimit.DuplicateWindowMs`). Commands are exempt by default.

| Key | Default | Effect |
| --- | --- | --- |
| `Chat.Enabled` | `true` | Turns Stratum chat formatting on |
| `Chat.LinkifyUrls` | `true` | Makes URLs in chat clickable |
| `Chat.Global.Enabled` | `true` | Turns global chat on |
| `Chat.Groups.Enabled` | `true` | Turns group chat on |
| `Chat.ConnectionMessages.ShowJoins` | `true` | Shows join messages in chat |
| `Chat.ConnectionMessages.ShowLeaves` | `true` | Shows leave messages in chat |
| `Chat.ConnectionMessages.ShowDisconnects` | `true` | Shows disconnect messages in chat |
| `Chat.RateLimit.Enabled` | `true` | Turns chat rate limiting on or off |
| `Chat.RateLimit.MinimumIntervalMs` | `750` | Minimum time between messages from one player |
| `Chat.RateLimit.DropDuplicates` | `true` | Drops identical messages inside the duplicate window |
| `Chat.RateLimit.DuplicateWindowMs` | `3000` | Duplicate-message window in milliseconds |
| `Chat.RateLimit.ExemptCommands` | `true` | Exempts commands from chat rate limiting |

## Info commands

Four commands show text from the `ServerInfo` section:

- `/rules` shows `ServerInfo.Rules`.
- `/discord` shows `ServerInfo.DiscordUrl`.
- `/website` shows `ServerInfo.WebsiteUrl`.
- `/motd` shows `ServerInfo.Motd`.

Set the text and links in `stratum.json`. Write line breaks in a rules text as `\n` inside the JSON string. A raw line break makes the file invalid.

## Role prefixes

Role prefixes add one or more colored tags before a player's name in chat. They are on by default (`Appearance.RolePrefixes.Enabled`). The tag text, color, bold style, and priority come from the role entries under `Appearance.RolePrefixes.Roles`. Higher priority prefixes appear first. A role can have multiple prefixes by using an array of entries.

## Join, leave, and welcome messages

The theme styles these messages. Each part is a separate switch under `Appearance.Theme`:

- `StyleJoinLeaveMessages` styles join and leave messages.
- `StyleWelcomeMessages` styles the welcome message for a joining player.
- `StyleDisconnectScreens` styles the disconnect screen.

Set a switch to `false` to use the vanilla message for that part.

## Nametags

Nametags add role text above a player's head. They are off by default. To turn them on, set `Appearance.Nametags.Enabled` to `true`.

When nametags are on, `Appearance.Nametags.ApplyRolePrefix` adds the role tag to the name, and `PrefixFormat` sets how the tag looks. The default format is `[{tag}] `, with a trailing space.
