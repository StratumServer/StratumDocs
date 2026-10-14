---
title: "Troubleshooting"
description: "Fix the most common problems: the server will not start, players cannot connect, TPS is low, or a setting did not stick."
category: "Help"
categoryOrder: 6
order: 1
---

Start with the symptom that matches what you see. Most fixes take a minute, and each one says what to check.

## The server stops at startup with "Address already in use"

Another program is using the port. The default is `42420`, and it needs both TCP and UDP. Stop the other program, or start Stratum with a different port:

```bash
./StratumServer --port 42421
```

Then open that port in your firewall and forward it on your router.

## The server does not start after an update

Check the log for the first lines that say what failed. Common causes:

- The launcher files were replaced while the server was running. Stop the server first.
- The install folder is not writable. The first start writes Stratum's files there.
- The release does not match your Vintage Story version. Download the release for your version. See [Updating](/docs/getting-started/updating).

Run `/stratum health` once the server is up. It reports what the runtime checked.

## Players cannot connect

Work through these in order:

1. The client and the server are on the same Vintage Story version. See [Connecting and versions](/docs/compatibility/connecting-and-versions).
2. The player is on the whitelist. A new server is invite-only by default.
3. Port `42420` is open for both TCP and UDP, on the firewall and on the router.
4. A client mod is not blocked by the client mods policy, if you turned it on.

## TPS is low

Check where the time goes before you change anything. Run `/stratum timings start`, let it run during a busy period, then `/stratum timings stop`. The report shows which system takes the most time. [Performance tuning](/docs/configuration/performance-tuning) lists the settings for each system, and [Entity tick throttling](/docs/configuration/entity-ticking) covers entity load.

## A setting did not stick

The config files are rewritten from memory on every save, and several commands save them. An edit you made to a file while the server ran is overwritten at the next save. Run `/stratum reload` after editing a file, so the server reads the change before it writes again. [Configuration](/docs/configuration/overview) explains how the files work.

## The world did not load after a restore

Check that the world file has the name the server expects, `default.vcdbs` for a world named `default`. Restore the backup into that name, and keep the file you replaced until the server starts. [Backups and restore](/docs/configuration/backups-and-restore) has the full steps.

## A mod does not work with Stratum

Some mods conflict with Stratum. Synergy and Tungsten do not work with it, because Stratum already includes their optimizations. See [Compatibility overview](/docs/compatibility/overview).
