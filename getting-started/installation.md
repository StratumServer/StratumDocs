---
title: "Installation"
description: "Download Stratum and get a server running."
category: "Getting Started"
categoryOrder: 1
order: 1
---

Stratum ships as a small launcher. You download the release for your platform, extract it, and run it. On the first start it prepares the matching vanilla server files for you.

## Requirements

- The .NET 10 runtime on the host. The launcher is framework dependent, so the runtime must be installed. Check it with `dotnet --list-runtimes`. The output should include `Microsoft.NETCore.App 10.x`.
- The Stratum release that matches your server's Vintage Story version.

## Download

Each release has one zip per platform, named `stratum-<version>-<platform>.zip`:

| Platform | File |
| --- | --- |
| Windows x64 | `stratum-<version>-win-x64.zip` |
| Linux x64 | `stratum-<version>-linux-x64.zip` |
| Linux ARM64 | `stratum-<version>-linux-arm64.zip` |

Get the zip for your platform from the [releases page](https://github.com/StratumServer/Stratum/releases).

## Run

1. Extract the zip wherever you want the server to live.
2. Start the launcher.

On Windows:

```bash
StratumServer.exe
```

On Linux:

```bash
./StratumServer
```

The first run prepares the vanilla server build that matches the release, then starts the patched server. Preparation runs once. [First launch](/docs/getting-started/first-launch) describes the steps.

## Data folder

Without `--dataPath`, the launcher uses a `Data` folder next to `StratumServer` and prints the path at startup. Pass `--dataPath` to choose another location. The launcher passes any flag it does not recognize to the server unchanged.
