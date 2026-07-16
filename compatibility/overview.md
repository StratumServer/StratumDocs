---
title: "Compatibility"
description: "Version matching, mods, and detecting Stratum."
category: "Compatibility"
categoryOrder: 4
order: 1
---

## Match the version

Stratum is built against a specific Vintage Story version. Run the release that matches the game version your server is on.

Players do not need anything extra. They connect with a normal client for the matching game version.

## Running with mods

Most normal server mods work. Stratum patches deeper server internals than a regular mod, so test your mod list before you move a live server over.

Stratum is not compatible with Synergy or Tungsten. Those optimizations are already built into Stratum, so running them together conflicts.

## Detecting Stratum from a mod

Stratum stamps its identity into `World.Config` at startup. Mods on either side can read it:

- `stratum` is a bool that is true on a Stratum server.
- `stratumVersion` is the Stratum version string.
- `stratumBaseGameVersion` is the Vintage Story version it was built against.

Gate optional behavior behind the `stratum` key rather than checking for internal types.
