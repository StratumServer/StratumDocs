---
title: "Backups and restore"
description: "Turn on scheduled world backups, find the backup files, and restore one."
category: "Configuration"
categoryOrder: 2
order: 6
---

Stratum can make world backups on a schedule. Backups are off by default.

## Turn backups on

```
/stratum set Backup.Enabled true
```

Or edit `stratum.json`:

```json
"Backup": {
  "Enabled": true,
  "IntervalMinutes": 360,
  "RetainCount": 5
}
```

## How backups work

A backup uses the SQLite backup API of the server, so it copies a consistent snapshot of the world. It runs on a background thread, and the server keeps ticking while it runs. If a backup is still running when the next one is due, the next one is skipped.

The first backup runs about 1 to 1.5 minutes after the server starts. The scheduler checks every 30 seconds, so the timing can vary by that much. After that, one backup runs every `IntervalMinutes`.

Backup files go to the `Backups` folder in your data folder, not next to the world file. Each file is named after the world and the time it was made:

```
stratum-backup-<worldname>-<timestamp>.vcdbs
```

For example, for a world named `default`: `stratum-backup-default-2026-10-14_18-30-00.vcdbs`.

After each successful backup, Stratum deletes the oldest of its own backup files beyond `RetainCount`. It does not touch backup files it did not create. Stratum sorts backups by their creation time, so do not copy or move files inside the `Backups` folder. A copied file can look newer than it is, and then the wrong backup is deleted.

With the defaults (`IntervalMinutes` of `360` and `RetainCount` of `5`), you keep about 30 hours of history.

## Config reference

| Key | Default | Effect |
| --- | --- | --- |
| `Backup.Enabled` | `false` | Must be `true` for backups to run |
| `Backup.IntervalMinutes` | `360` | Minutes between backups |
| `Backup.RetainCount` | `5` | Backup files to keep |
| `Backup.LogBackups` | `true` | Logs backup start and completion |

## Restore a backup

1. Stop the server.
2. Open the `Backups` folder and find the backup you want.
3. In the `Saves` folder of your data folder, move the current world file out of the way, for example rename `default.vcdbs` to `default.vcdbs.bak`. Also move any `default.vcdbs-wal` and `default.vcdbs-shm` files that sit beside it, because they belong to the old world. Keep the old files until you have confirmed the restore.
4. Copy the backup into the world's place, with the world's original name:

   ```bash
   cp stratum-backup-default-2026-10-14_18-30-00.vcdbs default.vcdbs
   ```

5. Start the server.

## What a backup covers

A backup holds the world database: terrain, block entities, and the player data stored in the world file. It does not hold these files:

- The config files: `stratum.json`, `stratum-commands.json`, and `stratum-performance.json`.
- The mod files and the Vintage Story server files.
- Logs.
- Player data kept outside the world file.

For a full copy, back up the whole data folder, the one that contains `serverconfig.json`, with an external tool or your host's snapshot feature.
