---
title: "Server presentation"
description: "Chat formatting, info commands, join messages, and nametags."
category: "Configuration"
categoryOrder: 2
order: 2
---

Stratum can handle the small presentation pieces most servers end up wanting. These live in the `Chat`, `Theme`, and `Nametags` sections of the config.

## Chat

The chat formatter can add role prefixes to messages and turn URLs into links. It also has rate controls to cut down on spam, including a minimum time between messages and duplicate message dropping.

## Info commands

Stratum reads a few pieces of server info text from the config and shows them through commands:

- `/rules`
- `/discord`
- `/website`
- `/motd`

Set the text and links in the config and players can pull them up whenever they need.

## Join and leave messages

The theme can style join, leave, welcome, and disconnect messages. Each of those is a separate toggle, so you can style only the parts you want.

## Nametags

Stratum can add a role tag in front of a player's name and apply role based coloring. It applies on join and can refresh when a player's role changes, without a reconnect.
