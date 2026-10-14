---
title: "Performance tuning"
description: "Find the setting that matches a symptom, and check the server before you change anything."
category: "Configuration"
categoryOrder: 2
order: 5
---

This page covers `stratum-performance.json`, the file that holds the load settings. Most servers can leave these at their defaults. Start with `/stratum timings` and `/stratum performance`. They show where the server spends its time, so you change the setting that matches the problem.

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
