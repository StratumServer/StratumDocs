---
title: "Compatibility"
description: "Version matching, mods, and detecting Stratum."
category: "Compatibility"
categoryOrder: 4
order: 1
---

## Match the version

Stratum is built for a specific Vintage Story game version and network protocol version. Run the release for the game's version your server uses. The server rejects clients by network protocol version, so a release may identify both values in its disconnect message.

Players do not need anything extra. They connect with a normal client for the matching game version.

## Running with mods

Most normal server mods work. Stratum patches deeper server internals than a regular mod, so test your mod list before you move a live server over.

Stratum's project policy is that it is not compatible with Synergy or Tungsten because their optimizations overlap with Stratum's. Stratum does not detect or block those mods, so test your mod set and do not assume the server will warn you about them.

## Detecting Stratum from a mod

There is no client-side identity marker in the current build. A server-side mod can detect Stratum by checking whether the `stratum` chat command is registered:

```csharp
bool isStratum = sapi.ChatCommands.Get("stratum") != null;
```

Do not look for `stratum`, `stratumVersion`, or `stratumBaseGameVersion` in `World.Config`; Stratum does not write those keys there.

Stratum patches parts of the game's assemblies directly. Harmony prefixes and postfixes are usually compatible, but a mod transpiler can conflict when it targets a method Stratum has changed. Check startup logs for `harmony visibility` warnings and run `/stratum preflight` when diagnosing a mod issue. A detected compatibility conflict can disable a Stratum speedup while preserving the feature's behavior.
