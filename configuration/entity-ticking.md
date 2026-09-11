---
title: "Entity tick throttling"
description: "How Stratum throttles entity ticking by distance, what is exempt, and how to test against it."
category: "Configuration"
categoryOrder: 2
order: 3
---

Stratum throttles server-side entity ticking by distance to the nearest player. The
settings live under `Performance.EntityTicking` in the `stratum-performance.json`
sidecar, and the feature is on by default. A `Performance` block in `stratum.json` is
read only when no sidecar exists, and the next save moves it into the sidecar. This
page covers the exact semantics: what anchors the distance bands, what is exempt, and
what to expect when testing against the throttle. All values below are the shipped
defaults.

## Distance bands

Each entity ticks every Nth server tick based on the 3D distance to the nearest
anchoring player:

| Band | Distance (blocks) | Interval | Keys |
|---|---|---|---|
| Near | up to 32 | every tick | `NearDistanceBlocks` |
| Mid | up to 64 | every 2nd tick | `MidDistanceBlocks`, `MidTickInterval` |
| Far | up to 96 | every 5th tick | `FarEntityDistanceBlocks`, `FarEntityTickInterval` |
| Very far | beyond | every 10th tick | `VeryFarTickInterval` |

Band edges are clamped so they never invert (near, then mid, then far), and the
intervals so they never decrease with distance.

## What anchors the bands

Distances are measured to **every online player that has a player entity**, whatever
its connection state. The server creates that entity as soon as it admits the client,
after authentication and any join queue, so a client anchors while it is still
loading, well before it reaches the Playing state. The entity is usually placed and
spawned at that moment. When placement is put off (spawn chunk not loaded yet, a
random spawn radius for new players, or crowd spawn spreading), the client anchors
from the position its entity held before placement until the server moves it to the
spawn point. For a returning player that is where they logged out. This matters more
than it looks:

- A client that is still connecting, authenticating or waiting in the join queue has
  no player entity yet and anchors nothing. Once admitted it anchors from its spawn
  position, even while it is still downloading assets or loading chunks.
- The measurement is **per dimension**: a player in another dimension does not anchor
  entities in this one.
- With no player entity online at all, the throttle steps aside and every entity
  ticks every tick, as in vanilla. With players online but none in the entity's
  dimension, the entity ticks at the very-far interval. Entities never stop ticking
  entirely.

## Exemptions

Checked per entity, in this order:

- **Player entities** are never throttled.
- **`ThrottleCreatures` / `ThrottleInanimate`** (both `true`): turn off throttling for
  a whole entity class.
- **Moving entities** (`SkipMovingEntities`, `true`): an entity whose motion exceeds
  `MovingEntitySpeedThreshold` (0.01 blocks per tick) is not throttled that tick. Note
  that even a dropped item keeps enough residual motion to stay exempt for a long time.
- **Excluded mod domains** (`ExcludedModDomains`, default `["vsvillage"]`): entities
  from these asset domains are never throttled. They skip the creature budget below
  only when they reach this check. Three kinds of entity leave earlier without the
  exemption: a moving entity, any creature while `ThrottleCreatures` is `false`, and
  any entity while throttling is disabled or no player entity is online. A non-zero
  `MaxCreatureTicksPerTick` can therefore still defer an excluded creature, such as a
  walking villager. Custom AI and pathfinder mods can assume a near-constant tick
  frequency, and throttling them can corrupt their internal state (observed with
  VSVillage's pathfinder: villagers froze at town edges under an earlier version of
  this system).

## Ambient fauna tier

On top of the band interval, entities whose code matches `AmbientFaunaCodePrefixes`
(by default fish, butterflies, dragonflies, bees, rats, chickens, hens, roosters, pigs
and sheep) get an extra stride multiplier (`AmbientFaunaTickMultiplier`, default 2):
background mood does not need full-rate AI
even near a player. Disable with `AmbientFaunaTierEnabled: false`.

## Creature budget

`MaxCreatureTicksPerTick` (default 0, meaning unlimited) hard-caps creature ticks per
server tick with round-robin fairness. It is opt-in, and it has **no effect when
region ticking is on** (`Performance.RegionTicking.Enabled`, off by default): the
region-parallel path never applies the budget, although creatures there are still
distance-throttled. `/stratum regions` warns when this combination is configured.

## Restoring vanilla behavior

`Performance.EntityTicking.Enabled: false` turns off the distance bands, the ambient
tier and the exemptions: every loaded entity then gets its game tick once per
entity-simulation tick, as in vanilla, as long as `MaxCreatureTicksPerTick` stays 0
and `HardDespawnEnabled` stays false (both apply whatever `Enabled` says). The physics
activation range (`Performance.Physics.ActivationRangeEnabled`) is a separate throttle
and is not affected.

## Testing against the throttle

Hard-won notes for anyone writing scenarios or load tests against this system (this
is how the [StratumParity](https://github.com/Pixnop/StratumParity) differential
suite pins these semantics down):

- **Your test client anchors as soon as the server admits it**, from wherever it
  spawned, even if it never gets past Connected. With no player entity online at all,
  the throttle is bypassed and every entity ticks at full rate, so a far probe shows
  no throttling at all.
- **Keep probe entities motionless** or `SkipMovingEntities` will exempt them from
  the very throttle you are measuring. A straw dummy stands perfectly still; a
  dropped item does not.
- **Derive geometry from the player's actual position**, read after any teleport
  settles: joins scatter players around the spawn point, which can silently move a
  probe across a band boundary.
- **Assert exact counts, not rates**: an unthrottled entity ticks exactly once per
  entity-simulation tick, a very-far entity exactly 1 in 10 (up to stride phase at
  the window edges). With the toggle off, both must equal the simulation tick count
  exactly.
- **Same dimension only**: a probe in another dimension than every player measures
  the very-far interval, not the band you placed a player in.
