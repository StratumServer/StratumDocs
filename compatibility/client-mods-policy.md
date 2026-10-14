---
title: "Client mods policy"
description: "How Stratum decides which client mods a player may join with, and how to turn the policy on."
category: "Compatibility"
categoryOrder: 4
order: 2
---

Players connect with the normal Vintage Story client. Some client mods change what a player sees or sends, and a server may want to allow only a known set of them. Stratum's client mods policy checks the mods a player joins with against a whitelist.

The policy is off by default. Nothing is checked until you turn it on.

## Turn it on

Set `ClientModPolicy.Enabled` to `true` in `stratum.json`, or use:

```
/stratum set ClientModPolicy.Enabled true
```

Before you enable it, list the mods your players already use. Otherwise players with those mods are turned away the first time the policy runs.

## How the whitelist works

With `StrictWhitelist` at `true` (the default), a player whose client has a mod that is not on the whitelist is disconnected. With it at `false`, the policy does not disconnect anyone for extra mods.

The core Vintage Story mods are always allowed.

`IncludeServerUniversalMods` is `true` by default. It adds the mods the server marks as universal to the whitelist, so you do not need to list them by hand.

`AllowModIds` is a list of extra mod IDs to allow. It is empty by default.

## Config

In `stratum.json`, under `ClientModPolicy`:

| Key | Default | Effect |
| --- | --- | --- |
| `ClientModPolicy.Enabled` | `false` | Turns the policy on. |
| `ClientModPolicy.StrictWhitelist` | `true` | Disconnects players who have mods that are not allowed. |
| `ClientModPolicy.IncludeServerUniversalMods` | `true` | Allows the mods the server marks as universal. |
| `ClientModPolicy.AllowModIds` | `[]` | Extra mod IDs to allow. |

## Test before you turn it on

Join the server with a client that has your usual mods. Then join with a client that has none. If your players have mods you did not expect, add their IDs to `AllowModIds` before you enable the policy on the live server.
