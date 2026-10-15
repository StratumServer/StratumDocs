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

Before you enable it, collect the mod IDs your players already use. Ask each player to copy the `modid` value from each mod's `modinfo.json`. Use the ID, not the mod's display name. Otherwise, players with unlisted mods are disconnected when they join.

## How the whitelist works

With `StrictWhitelist` at `true` (the default), a player whose client has a mod that is not on the whitelist is disconnected. With it at `false`, the policy does not disconnect anyone for extra mods.

The core Vintage Story mods are always allowed.

`IncludeServerUniversalMods` is `true` by default. It adds the mods the server marks as universal to the whitelist, so you do not need to list them by hand.

`AllowModIds` is a list of extra entries added to the whitelist. Each entry is used as you write it, with spaces at the ends removed. The list is empty by default.

For example, if the mod's `modid` is `examplemod`, add that ID:

```json
{
  "ClientModPolicy": {
    "AllowModIds": ["examplemod"]
  }
}
```

Merge this object into the top level of `stratum.json` and keep its existing settings. A server universal mod is already allowed when `IncludeServerUniversalMods` is `true`.

## Config

In `stratum.json`, under `ClientModPolicy`:

| Key | Default | Effect |
| --- | --- | --- |
| `ClientModPolicy.Enabled` | `false` | Turns the policy on. |
| `ClientModPolicy.StrictWhitelist` | `true` | Disconnects players who have mods that are not allowed. |
| `ClientModPolicy.IncludeServerUniversalMods` | `true` | Allows the mods the server marks as universal. |
| `ClientModPolicy.AllowModIds` | `[]` | Extra mod IDs to allow. |

## Roll it out safely

Keep `Enabled` set to `false` while you collect IDs. Add the IDs you intend to allow, then enable the policy during a test session or maintenance window. Test with one client that has an allowed mod and one with an unlisted mod. The first should join; the second should be disconnected. If a regular player gets disconnected, turn the policy off, add the missing ID, and try again.
