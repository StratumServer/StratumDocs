---
title: "Installation"
description: "Download Stratum and get a server running."
category: "Getting Started"
categoryOrder: 1
order: 1
---

Stratum ships as a small launcher. You download the release for your platform, extract it, and run it. On the first start it fetches the matching vanilla server files for you.

## Requirements

- A host with the .NET 10 runtime installed. The launcher is framework dependent, so the runtime has to be present.
- The Stratum release that matches your server's Vintage Story version.

## Download and run

1. Open the [releases page](https://github.com/StratumServer/Stratum/releases) and download the zip for your platform. Windows builds are named `stratum-<version>-win-x64.zip` and Linux builds are `stratum-<version>-linux-x64.zip`.
2. Extract it wherever you want the server to live.
3. Start it.

On Windows:

```bash
StratumServer.exe
```

On Linux:

```bash
./StratumServer
```

The first run downloads and unpacks the matching vanilla server build, then starts the patched runtime on top. This only happens once. See [First launch](/docs/getting-started/first-launch) for the details.

## Data folder

If you do not pass `--dataPath`, Stratum stores its world and settings in a `Data` folder next to the launcher. To use a different location, pass `--dataPath` with the path you want.
