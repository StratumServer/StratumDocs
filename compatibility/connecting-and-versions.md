---
title: "Connecting and versions"
description: "Why a player cannot join, and how the server and client versions have to line up."
category: "Compatibility"
categoryOrder: 4
order: 3
---

Players do not install anything for Stratum. They connect with a normal Vintage Story client. The client has to be on the same game version as the server, and the server has to run the Stratum release built for that version.

## Two versions to match

There are two version numbers, and they fail in different ways.

The first is the Vintage Story version. Stratum is built against one version, such as `1.22.7`. A player whose client is on another version cannot join, the same as on a stock server. Update the client to the server's version.

The second is the Stratum version. Each Stratum release is tagged with the Vintage Story version it is built against, and a counter for its own releases, for example `1.22.7-stratum.2`. Run the release whose game version matches your server. If your server runs a release for an older game version, update the server, not the players.

## Check the server version

The startup log names the running version. The `/stratum version` command shows the same details while the server runs.

When a newer release exists, the log warns you once at startup and then again every 12 hours by default, with a link to the release. The interval is set by `UpdateChecker.CheckIntervalHours`.

## Check the port

Players connect to the server port, which defaults to `42420`. The server listens on both TCP and UDP on that port. If a player cannot connect, check that your firewall and router allow both protocols. For a server behind a router, forward both protocols to the server machine.

## Check the whitelist

A new server is invite-only by default. The startup log says so. A player who is not on the whitelist is turned away even with the right version. Add players with the whitelist commands of the server before you tell them to connect.

## Mods

Most server mods work with Stratum. Stratum patches deeper server code than a regular mod, so test your mod list before you move a live server over. Some mods are known to conflict. See [Compatibility overview](/docs/compatibility/overview), and [Client mods policy](/docs/compatibility/client-mods-policy) for controlling which client mods can join.
