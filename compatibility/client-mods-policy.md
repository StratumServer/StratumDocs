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

Before you enable it, collect the mod IDs your players already use. Ask each player to copy the `modid` value from each mod's `modinfo.json`. Use the ID, not the mod's display name. With the policy enabled, the client disables unlisted client and universal mods for that session and can still join. This is a client-side convenience control, not an anticheat; a modified client can ignore the list.

## How the whitelist works

With `StrictWhitelist` at `true` (the default), Stratum sends its allow list to the client. The stock client disables client and universal mods that do not match an entry, then continues joining. With `StrictWhitelist` at `false`, Stratum sends no replacement list and the vanilla `ModIdWhiteList` and `ModIdBlackList` settings apply unchanged. When the Stratum policy is active, the vanilla allow list is merged into its list, but the vanilla deny list is no longer sent.

The core Vintage Story mods `game`, `creative`, and `survival` are always allowed. Matching is a case-sensitive substring check against `modid@version`, so `examplemod` also matches `myexamplemod2`, while `ExampleMod` does not match `examplemod`. Add `@version` to pin an entry, such as `examplemod@1.2.3`. Server-only client mods are not checked.

`IncludeServerUniversalMods` is `true` by default. It adds the mods the server marks as universal to the whitelist as `modid@version` entries, so you do not need to list them by hand.

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
| `ClientModPolicy.StrictWhitelist` | `true` | Sends Stratum's allow list to the client. Set to `false` to leave the vanilla allow and deny lists unchanged. |
| `ClientModPolicy.IncludeServerUniversalMods` | `true` | Allows the mods the server marks as universal. |
| `ClientModPolicy.AllowModIds` | `[]` | Extra mod IDs to allow. |
| `ClientModPolicy.LogPolicyOnStartup` | `true` | Logs the policy state and allow-list entry count at startup. |

## Roll it out safely

Keep `Enabled` set to `false` while you collect IDs. Add the IDs you intend to allow, then enable the policy during a test session or maintenance window. Edit `AllowModIds` in `stratum.json`, since lists cannot be changed with `/stratum set`. Run `/stratum reload` to apply the change for the next join. Test with one client that has an allowed mod and one with an unlisted mod. The first should join with the mod enabled; the second should join with that mod disabled. If a player needs another mod, add its ID and reload.
