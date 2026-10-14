---
title: "Privacy and outbound requests"
description: "What Stratum sends over the network, what it does not send, and how to turn each part off."
category: "Configuration"
categoryOrder: 2
order: 7
---

Read this page before you expose your server to the internet. It covers two things. The first is the outbound requests Stratum makes on its own: the statistics report and the update check. The second is the player privacy controls, which are off by default.

## Server statistics

Stratum sends anonymous statistics to `https://my.stratumvs.dev/stratum-stats.php`. Reporting is on by default. The first report goes out shortly after startup, and then one report goes out every 30 minutes.

Each report is a JSON POST with these fields:

- `serverId`: a random ID for the install. It is generated once and saved as `ServerStats.ServerId` in `stratum.json`.
- `stratumVersion` and `gameVersion`: the Stratum and Vintage Story versions.
- `players`, and `playerCounts` with `apiOnline` and `admittedClients`: how many players are online and admitted.
- `maxPlayers`: the configured player limit.
- `os`: `windows`, `linux`, `macos`, or `other`.
- `reportedAtUtc`: the UTC time of the report.

Reports do not include player names, the server name, the server address, or the world name. The request still reaches the endpoint from your server's IP address, as any web request does. It also sends a `User-Agent` header with the Stratum version.

To turn reporting off:

```
/stratum set ServerStats.Enabled false
```

You can also set `"Enabled": false` in the `ServerStats` section of `stratum.json`, then restart. The first report goes out soon after startup, so a server that starts with reporting on sends one report before you can turn it off.

Generate a new ID by deleting the `ServerId` value in `stratum.json` and restarting the server.

| Key | Default | Effect |
| --- | --- | --- |
| `ServerStats.Enabled` | `true` | Turns reporting on |
| `ServerStats.ReportUrl` | `https://my.stratumvs.dev/stratum-stats.php` | Endpoint for the report. An empty value turns sending off |
| `ServerStats.IntervalMinutes` | `30` | Minutes between reports |
| `ServerStats.TimeoutSeconds` | `10` | Seconds to wait for the endpoint |

## Update check

Stratum checks GitHub for a newer release at startup and every 12 hours. When a newer release exists, online staff get an in-game message.

The check makes one GET request to `https://api.github.com/repos/StratumServer/Stratum/releases/latest`. The request carries no data about your server, but GitHub sees your server's IP address and the standard request headers, including a `User-Agent` with the Stratum version.

To turn the check off:

```
/stratum set UpdateChecker.Enabled false
```

| Key | Default | Effect |
| --- | --- | --- |
| `UpdateChecker.Enabled` | `true` | Turns the check on |
| `UpdateChecker.CheckOnStartup` | `true` | Checks once at startup |
| `UpdateChecker.CheckIntervalHours` | `12` | Hours between checks |
| `UpdateChecker.NotifyStaffInGame` | `true` | Tells online staff when a release is available |

## Player privacy controls

Off by default. These controls hide map pins and round the coordinates other players see. Nothing else in the `PlayerPrivacy` section has an effect until `PlayerPrivacy.Enabled` is `true`.

```
/stratum set PlayerPrivacy.Enabled true
```

| Key | Default | Effect |
| --- | --- | --- |
| `PlayerPrivacy.Enabled` | `false` | Turns the controls on |
| `PlayerPrivacy.HideMapPins` | `true` | Hides players' map pins from each other |
| `PlayerPrivacy.AllowGroupMapVisibility` | `true` | Members of the same group still see each other's pins |
| `PlayerPrivacy.CoordinateSnapBlocks` | `0` | Rounds the coordinates other players see to a grid of this many blocks. `0` means no rounding. It has no effect while `HideMapPins` is `true`, because the pins are hidden |

Staff can bypass these controls. The privilege is `stratum.privacy.bypass`, and it is set under `PlayerPrivacy.StaffOverride`.

## Load testing

`LoadTesting.AllowUnauthenticatedClients` is `false` by default. When it is `true`, the server accepts bot clients that skip the normal login. It exists for controlled load tests. Never enable it on a public server.
