---
title: "Config reference"
description: "The main Stratum settings, their defaults, and what each one does."
category: "Configuration"
categoryOrder: 2
order: 4
---

This page lists the main settings in `stratum.json` and the defaults that a fresh install writes. Use the dotted path with `/stratum get` and `/stratum set`, for example `/stratum set PlayerPrivacy.Enabled true`.

Performance settings are in `stratum-performance.json`, and command settings are in `stratum-commands.json`. [Performance tuning](/docs/configuration/performance-tuning) covers the first file.

## Diagnostics

| Key | Default | Effect |
| --- | --- | --- |
| `Diagnostics.LogStartupSummary` | `true` | Prints a summary of the active features at startup |
| `Diagnostics.RunStartupPreflight` | `true` | Runs the startup checks before the tick loop starts |

## UpdateChecker

Checks GitHub for a newer release. See [Privacy and outbound requests](/docs/configuration/privacy-and-outbound-requests).

| Key | Default | Effect |
| --- | --- | --- |
| `UpdateChecker.Enabled` | `true` | Turns the check on |
| `UpdateChecker.CheckOnStartup` | `true` | Checks once at startup |
| `UpdateChecker.CheckIntervalHours` | `12` | Hours between checks |
| `UpdateChecker.NotifyStaffInGame` | `true` | Tells online staff when a release is available |

## ServerStats

Sends anonymous statistics to the Stratum statistics endpoint. See [Privacy and outbound requests](/docs/configuration/privacy-and-outbound-requests).

| Key | Default | Effect |
| --- | --- | --- |
| `ServerStats.Enabled` | `true` | Turns reporting on |
| `ServerStats.IntervalMinutes` | `30` | Minutes between reports |
| `ServerStats.TimeoutSeconds` | `10` | Seconds to wait for the endpoint |

## Hardening

| Key | Default | Effect |
| --- | --- | --- |
| `Hardening.InventoryGuards` | `true` | Hides inventory contents from players who have not opened the container |
| `Hardening.PreserveVanillaProtocol` | `true` | Protocol compatibility switch. Leave at the default |
| `Hardening.PacketMonitoring` | `true` | Monitors packet traffic. `/stratum packets` shows the current state |
| `Hardening.BlockBreakGuards` | `true` | Master switch for block break validation. Its settings are in `BlockBreakGuards` |
| `Hardening.EntityGuards` | `false` | Shown in the status output. This build applies no entity guard, so the value has no effect |
| `Hardening.EntityPacketRangeGuard` | `true` | Limits entity packets to players in range |
| `Hardening.BlockEntityPacketRangeGuard` | `true` | Limits block entity packets to players in range |

## PacketLimits

Caps the packet rate per client. On by default.

| Key | Default | Effect |
| --- | --- | --- |
| `PacketLimits.Enabled` | `true` | Turns the limit on |
| `PacketLimits.KickViolations` | `true` | Kicks clients that break the limit |
| `PacketLimits.KickAfterViolations` | `8` | Violations inside the window before a kick |
| `PacketLimits.WindowSeconds` | `5` | Length of the violation window |

## PacketBackPressure

Caps how many packets the main thread drains per tick. Off by default. Turn it on only when the main thread spends too long draining packets.

| Key | Default | Effect |
| --- | --- | --- |
| `PacketBackPressure.Enabled` | `false` | Turns the cap on |
| `PacketBackPressure.MaxMillisecondsPerTick` | `25` | Time budget for draining packets per tick |
| `PacketBackPressure.MaxPacketsPerClientPerTick` | `32` | Packets drained per client per tick |
| `PacketBackPressure.MaxQueueDepthPerClient` | `2000` | Queue depth at which a client is kicked |

## BlockBreakGuards

| Key | Default | Effect |
| --- | --- | --- |
| `BlockBreakGuards.Enabled` | `true` | Checks block break progress on the server before accepting a break |
| `BlockBreakGuards.DropViolations` | `true` | Rejects suspicious breaks without a message |

## Anticheat

Off by default. Turn it on with `Anticheat.Enabled`. Nothing else in this section runs while it is off.

| Key | Default | Effect |
| --- | --- | --- |
| `Anticheat.Enabled` | `false` | Turns the checks on |
| `Anticheat.StaffAlerts` | `true` | Tells online staff when a violation is found |

## ClientModPolicy

Off by default. Controls which client mods a player may join with.

| Key | Default | Effect |
| --- | --- | --- |
| `ClientModPolicy.Enabled` | `false` | Turns the policy on |
| `ClientModPolicy.StrictWhitelist` | `true` | Sends Stratum's allow list to the client, which disables unlisted client mods for that session. It does not disconnect the player. |
| `ClientModPolicy.IncludeServerUniversalMods` | `true` | Adds the mods the server marks as universal to the whitelist |
| `ClientModPolicy.AllowModIds` | `[]` | Extra mod IDs to allow |
| `ClientModPolicy.LogPolicyOnStartup` | `true` | Logs the policy state at startup |

## LoginProtection

Gives players temporary damage immunity when they join.

| Key | Default | Effect |
| --- | --- | --- |
| `LoginProtection.Enabled` | `true` | Turns the protection on |
| `LoginProtection.ProtectionSeconds` | `5` | Length of the immunity |
| `LoginProtection.CancelOnHorizontalMove` | `true` | Ends the immunity when the player moves |
| `LoginProtection.CancelInFireOrLava` | `true` | Ends the immunity in fire or lava |

## PlayerPrivacy

Off by default. Hides map pins and rounds coordinates. Nothing else in this section has an effect until `PlayerPrivacy.Enabled` is `true`.

| Key | Default | Effect |
| --- | --- | --- |
| `PlayerPrivacy.Enabled` | `false` | Turns the privacy controls on |
| `PlayerPrivacy.HideMapPins` | `true` | Hides players' map pins from each other |
| `PlayerPrivacy.AllowGroupMapVisibility` | `true` | Lets members of the same group see each other's pins |
| `PlayerPrivacy.CoordinateSnapBlocks` | `0` | Rounds the coordinates other players see to a grid of this many blocks. `0` means no rounding. It has no effect while `HideMapPins` is `true`. Staff, and group members when `AllowGroupMapVisibility` is on, see exact coordinates |

## CrowdSpawn

Spreads new players away from a crowd at spawn.

| Key | Default | Effect |
| --- | --- | --- |
| `CrowdSpawn.Enabled` | `true` | Turns the spread on |
| `CrowdSpawn.MinimumNearbyPlayers` | `24` | Players near spawn before the spread starts |
| `CrowdSpawn.DetectionRadiusBlocks` | `48` | Radius used to count nearby players |
| `CrowdSpawn.SpreadRadiusBlocks` | `64` | How far from spawn new players can be placed |
| `CrowdSpawn.SafePositionTries` | `15` | Safe-position attempts before placement |
| `CrowdSpawn.IncludeReturningPlayers` | `false` | Applies the spread to returning players too |

## MobSpawning

| Key | Default | Effect |
| --- | --- | --- |
| `MobSpawning.HostileEnabled` | `true` | Natural spawning of hostile mobs |
| `MobSpawning.NeutralEnabled` | `true` | Natural spawning of neutral mobs |
| `MobSpawning.PassiveEnabled` | `true` | Natural spawning of passive mobs |
| `MobSpawning.NaturalSpawnMultiplier` | `1` | Multiplier on natural spawn rates, clamped from `0` to `1` |

`/mobs` controls natural spawning at runtime.

## Worldgen

| Key | Default | Effect |
| --- | --- | --- |
| `Worldgen.SplitTerrainPass` | `true` | Runs terrain generation as a split pass |
| `Worldgen.AutoDisableSplitForMods` | `true` | Turns the split pass off when a mod that conflicts with it is loaded |

Turn the split pass off by hand if a mod conflicts with it.

## Combat and groups

| Key | Default | Effect |
| --- | --- | --- |
| `CoopCombat.Enabled` | `false` | Lets several players hit the same creature. `/pvecombat` toggles it |
| `FriendlyFire.AllowGroupDamage` | `true` | Lets group members damage each other. `/friendlyfire` toggles it |
| `CombatLog.Enabled` | `false` | Penalties for disconnecting during PvP |
| `Groups.Enabled` | `true` | Turns the group administration features on |
| `Groups.RelationsEnabled` | `true` | Lets groups set relations with each other |
| `Groups.AlliesCountAsSameSide` | `true` | Allied groups count as one side for friendly fire |
| `Groups.ShowStateToPlayers` | `true` | Shows group kind, tag, and relations to regular players in `/group info` |

## Backup

See [Backups and restore](/docs/configuration/backups-and-restore).

| Key | Default | Effect |
| --- | --- | --- |
| `Backup.Enabled` | `false` | Turns scheduled backups on |
| `Backup.IntervalMinutes` | `360` | Minutes between backups |
| `Backup.RetainCount` | `5` | Backup files to keep |
| `Backup.LogBackups` | `true` | Logs backup start and completion |

## Announcements

| Key | Default | Effect |
| --- | --- | --- |
| `Announcements.Enabled` | `false` | Turns timed announcements on |
