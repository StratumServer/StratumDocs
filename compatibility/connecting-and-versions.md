---
title: "Connecting and versions"
description: "Why a player cannot join, and how the server and client versions have to line up."
category: "Compatibility"
categoryOrder: 4
order: 3
---

Players do not install anything for Stratum. They connect with a normal Vintage Story client. Run the Stratum release built for your server's Vintage Story game version. During connection, Stratum compares the client's network protocol version with the server's.

## Game and network versions

The Vintage Story game version identifies the game build, such as `1.22.7`. Each game build also uses a network protocol version. Stratum's join check compares the protocol version, not the game version string. A mismatch screen shows both values for the client and server; use those values to decide which side to update. For example, the server can show game version `1.22.7` with network version `1.22.6`.

The version rejection screen is titled `Version mismatch` and shows the client's game and network versions beside the server's values. Its hint says to use a matching Vintage Story version for the server.

Other common rejection screens tell the player what to fix:

| Screen | What the player sees | What staff should check |
| --- | --- | --- |
| `Whitelist required` | The server allows approved players only. | Add the player with `/whitelist add <player>`, or change `WhitelistMode`. |
| `Whitelist expired` | The whitelist entry's expiration time in UTC. | Review or renew that player's whitelist entry. |
| `Invalid password` | The password was not accepted. | Confirm the server password with the player. |
| `Account banned` | The ban expiry, issuing staff member, and reason. | Review the ban and lift it if it was applied by mistake. |
| `Invalid player name` | Names must use 1 to 16 letters, numbers, underscores, or hyphens. | Have the player correct the name in the client. |
| `Queue is full` or `Server is full` | The queue or server has reached its limit. | Check available player slots and queue settings. |
| `Account verification failed` | The account could not be verified. | Ask the player to check their Vintage Story account session. |
| `Auth server unavailable` | The authentication service could not be reached. | Check `server-main.log` and `server-debug.log` and the server's outbound connectivity. |

## Stratum release version

Each Stratum release tag includes the Vintage Story version it is built against and a Stratum revision, for example `1.22.7-stratum.2`. Run the release whose game version matches the server. If the server runs a release for an older game version, update the server, not the players.

## Check the server version

The startup log names the running version. The `/stratum version` command shows the same details while the server runs.

When GitHub reports a newer release, the log warns at startup and then every 12 hours by default, with a link. The interval is set by `UpdateChecker.CheckIntervalHours`, which is clamped to 1 through 168 hours. The check ignores prereleases and compares game versions first, so its reminder can point to a release built for a newer Vintage Story version. Check the release's game version before updating.

Use these commands to inspect the running build or force an update check:

```
/stratum version
/stratum version check
```

## Check the port

Players connect to the server port, which defaults to `42420`. The server listens on both TCP and UDP on that port. If a player cannot connect, check that your firewall and router allow both protocols. For a server behind a router, forward both protocols to the server machine.

## Check the whitelist

A new dedicated server is invite-only by default. A rejected player sees a `Whitelist required` message and is asked to contact staff. Add a player with `/whitelist add <player>` or set `WhitelistMode` to `Off` in `serverconfig.json` before you tell them to connect.

## Mods

Most server mods work with Stratum. Stratum patches deeper server code than a regular mod, so test your mod list before you move a live server over. Some mods are known to conflict. See [Compatibility overview](/docs/compatibility/overview), and [Client mods policy](/docs/compatibility/client-mods-policy) for controlling which client mods can join.
