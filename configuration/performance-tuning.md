---
title: "Performance tuning"
description: "Find the setting that matches a symptom, and check the server before you change anything."
category: "Configuration"
categoryOrder: 2
order: 5
---

This page covers `stratum-performance.json`, the file that holds the load settings. The keys below are the names inside that file. To change one in game, add the `Performance.` prefix, for example `/stratum set Performance.ChunkSending.MaxChunksPerServerTick 384`. Most servers can leave these at their defaults. Start with `/stratum timings` and `/stratum performance`. They show where the server spends its time, so you change the setting that matches the problem.

## Read the current state

`/stratum performance` shows which performance features are on and how full their queues are.

`/stratum timings` shows where tick time goes. Timings are off at startup. To turn them on for the next start:

```
/stratum set Performance.Timings.EnabledOnStartup true
```

Restart the server afterwards. While the server runs, `/stratum timings start`, `/stratum timings stop`, and `/stratum timings reset` control the timings.

## Chunk sending

When players move or join, the server sends them terrain chunks. Many moving players at once can spike tick time.

| Key | Default | Effect |
| --- | --- | --- |
| `ChunkSending.MaxChunksPerServerTick` | `384` | Hard cap on chunks sent per server tick |
| `ChunkSending.FairSchedulingEnabled` | `true` | Spreads sends across players instead of serving one player first |
| `ChunkSending.AdaptiveUnderOverload` | `true` | Lowers the send rate when the server is under load |
| `ChunkSending.OverloadTickMs` | `45` | Tick time above which the reduction starts |

If players see long load-in times and other players lag during big joins, lower `MaxChunksPerServerTick`, or reduce join pressure with the settings below.

## Join pressure

| Key | Default | Effect |
| --- | --- | --- |
| `Join.CacheStartupPackets` | `true` | Reuses startup packet payloads for repeated joins |
| `Join.MaxQueueAdmissionsPerPass` | `2` | Queued clients admitted to the join path per tick pass |
| `Join.MaxJoinsPerTick` | `3` | Joins handled per tick |

## Entity ticking

Entity ticking has its own page: [Entity tick throttling](/docs/configuration/entity-ticking). Use it when TPS drops with many animals loaded and no players near them.

## Pathfinding

Creature pathfinding runs on background workers. The queue can back up under pressure.

| Key | Default | Effect |
| --- | --- | --- |
| `Pathfinding.Async` | `true` | Runs path requests on background workers |
| `Pathfinding.MaxQueued` | `512` | Limit on pending path requests |
| `Pathfinding.PriorityEnabled` | `true` | Serves near-player requests first when the queue passes `PriorityQueueThreshold` (default `64`) |
| `Pathfinding.DropFarTasksUnderPressure` | `true` | Drops requests farther than `FarTaskDistanceBlocks` (default `160`) when the queue is under pressure |
| `Pathfinding.MaxTaskAgeMs` | `2000` | Drops tasks older than this many milliseconds |

## Autosave

Autosave spreads write work across ticks instead of doing it all at once.

| Key | Default | Effect |
| --- | --- | --- |
| `AutoSave.IncrementalFlushIntervalSeconds` | `10` | Seconds between incremental flushes |
| `AutoSave.MaxLoadedChunksPerFlush` | `256` | Chunks written per flush |
| `AutoSave.MaxDelaySeconds` | `120` | Longest a save can wait while chunk pressure is high |

If TPS drops every few minutes and the server stalls during a save, lower `MaxLoadedChunksPerFlush`. Each flush then writes less, and the work spreads over more flushes.

## Block ticks

Block ticks (crops, furnaces, growing plants) run on the main thread and add up in a large world.

| Key | Default | Effect |
| --- | --- | --- |
| `BlockTicks.MaxRandomTicksPerChunk` | `8` | Random ticks per chunk per pass |
| `BlockTicks.MaxChunksPerPass` | `512` | Chunks processed per pass |
| `BlockTicks.MaxMainThreadBlockTicksPerPass` | `5000` | Cap on block ticks on the main thread per pass |
| `BlockTicks.AdaptiveUnderOverload` | `true` | Lowers block ticks when the server is under load |
| `BlockTicks.MaxDirtyBlocksPerPass` | `512` | Distinct block positions published to clients per pass. A deeper backlog raises the limit for that pass |

## Pregeneration

Pregeneration builds terrain ahead of player movement. It is on by default and pauses while players are online.

| Key | Default | Effect |
| --- | --- | --- |
| `Pregen.Enabled` | `true` | Turns pregeneration on |
| `Pregen.PauseWhenPlayersOnline` | `true` | Pauses while any player is connected |
| `Pregen.PauseBelowTps` | `15.0` | Pauses when TPS falls below this value |
| `Pregen.MaxColumnsPerSecond` | `500` | Chunk columns generated per second |

To turn pregeneration off:

```
/stratum set Performance.Pregen.Enabled false
```

Check its state with `/stratum pregen`.

Staff can also queue jobs that run one after another, so you can generate several areas without starting each one by hand:

```
/stratum pregen queue radius <radiusChunks> [centerChunkX] [centerChunkZ]
/stratum pregen queue rect <chunkX1> <chunkZ1> <chunkX2> <chunkZ2>
/stratum pregen queue list
/stratum pregen queue clear
```

The queue holds up to 32 jobs and lives in memory only, so a restart empties it. `/stratum pregen skip` ends the current job and starts the next one. `/stratum pregen stop` ends the current job and clears the queue.

## Falling blocks

Vanilla spawns one entity for every sand, gravel, or rock block that falls, so a large cave-in can put thousands of entities into one tick. Stratum limits how many fall at once near players and resolves falls far from players without an entity.

| Key | Default | Effect |
| --- | --- | --- |
| `FallingBlocks.Enabled` | `true` | Turns the system on. It is read at startup, so a change needs a restart |
| `FallingBlocks.MaxFallingLimit` | `1000` | Falling entities alive at once near players. Extra falls wait in a first-in, first-out queue. Clamped from `10` to `10000` |
| `FallingBlocks.MaxInstantPerTick` | `100` | Falls resolved without an entity per tick when no player is in range |
| `FallingBlocks.StuckTimeoutMs` | `15000` | A falling entity older than this is placed where it hangs. `0` turns the timeout off |

A fall counts as far from players when no player is inside the entity tracking range, 128 blocks by default. A lower `MaxFallingLimit` keeps the tick flat and makes a big collapse take longer. The contributor measured a heap of about 13,600 sand blocks: a limit of `1000` gave about 4.4 ms mean tick time and took more than four minutes, `5000` gave about 15 ms and about 190 seconds, and vanilla gave about 22 ms and about 134 seconds.

## Dropped item piles

Resting items of the same type that lie close together are merged into larger stacks, which cuts the number of loose item entities in busy areas. An item takes part after it has settled and waited `DelayMs`. A stack a player has just dropped waits until the vanilla pickup window of one second has passed. Merging uses the vanilla stack rules, so items with different attributes do not merge. The merged stack keeps the longer of the two remaining despawn times, and stacks in different land claims never mix.

| Key | Default | Effect |
| --- | --- | --- |
| `ItemClump.Enabled` | `true` | Turns merging on. It is read at startup, so a change needs a restart |
| `ItemClump.Radius` | `5.0` | Horizontal search radius in blocks. Clamped from `0.25` to `8` |
| `ItemClump.VerticalRadius` | `2.0` | Vertical search radius in blocks. There is no line of sight check, so a value of `2` reaches through a one block floor |
| `ItemClump.DelayMs` | `1500` | Wait after an item spawns or loads, and between retries while it still moves |
| `ItemClump.MaxSettleAttempts` | `20` | Retries while an item has not settled |
| `ItemClump.MaxAttemptsPerTick` | `25` | Merge attempts per tick |
| `ItemClump.MaxGroupSize` | `32` | Items in one merge group |
| `ItemClump.Blacklist` | `[]` | Collectible codes that never merge. Entries accept `*` wildcards, such as `game:gear-*`, and `@regex` patterns that match the whole code including the domain |

## Send queue

The send queue gives each connection its own sender. It retries short writes and disconnects a connection whose unsent data grows past a cap. It is off by default. The 1000-bot run and play session needed to turn it on by default are not published yet, so try it on a test server first.

| Key | Default | Effect |
| --- | --- | --- |
| `Network.SendQueueEnabled` | `false` | Turns the queue on |
| `Network.MaxPendingBytes` | `8388608` | Unsent bytes one connection may hold before it is disconnected |
| `Network.LargeThresholdBytes` | `1400` | Packets at or above this size go out alone instead of being coalesced |
| `Network.CoalesceLimitBytes` | `65536` | Largest size of one coalesced send |

## Timer resolution (Windows only)

On Windows the default timer resolution is about 15.6 ms, which caps a server near 25 TPS. Stratum sets it to 1 ms at startup.

| Key | Default | Effect |
| --- | --- | --- |
| `TimerResolution.Enabled` | `true` | Applies the 1 ms setting. Has no effect on Linux |
| `TimerResolution.PeriodMs` | `1` | Timer resolution in milliseconds |

The timer is set at startup, so restart the server after you change these values.

## Physics and simulation distance

| Key | Default | Effect |
| --- | --- | --- |
| `Physics.Enabled` | `true` | Turns physics tick throttling and activation ranges on |
| `Physics.EntityTrackingHysteresisEnabled` | `true` | Keeps an entity tracked until it passes an outer radius 10% beyond the tracking range, so entities on the edge do not flip between tracked and untracked. `false` restores the vanilla hard cutoff |
| `Physics.ClientListReuseEnabled` | `true` | Reuses the client list between physics ticks. `false` restores the vanilla allocation on every tick |
| `SimulationDistance.Enabled` | `true` | Turns the distance limits on |
| `SimulationDistance.RandomTickDistanceBlocks` | `96` | Random ticks run only within this distance of players |
| `SimulationDistance.BlockGameTickListenerDistanceBlocks` | `128` | Block game tick listeners run only within this distance of players |

## Where to start

1. Run `/stratum timings` to see where tick time goes.
2. If chunk sending dominates, look at `ChunkSending` and `Join`.
3. If entity simulation dominates, read [Entity tick throttling](/docs/configuration/entity-ticking).
4. If autosave spikes come at a regular interval, look at `AutoSave`.
5. If block ticks dominate on a large or heavily farmed world, look at `BlockTicks`.
6. If the server is busy when nobody is online, check `Pregen`.
7. If cave-ins or sand collapses cause the spike, look at `FallingBlocks`. If many loose items pile up, look at `ItemClump`.
