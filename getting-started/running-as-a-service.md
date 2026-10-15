---
title: "Running as a service"
description: "Run Stratum under systemd so it starts at boot and restarts after a crash."
category: "Getting Started"
categoryOrder: 1
order: 4
---

This page shows how to run Stratum as a background service on Linux, so it starts at boot and restarts after a crash. The examples use systemd.

## Create the user and set permissions

Create a system user for the server, then give it the install directory and the data directory:

```bash
sudo useradd --system --home /opt/stratum --shell /usr/sbin/nologin stratum
sudo mkdir -p /opt/stratum /srv/stratum/data
sudo unzip "stratum-<version>-linux-x64.zip" -d /opt/stratum
sudo chmod +x /opt/stratum/StratumServer
sudo chown -R stratum:stratum /opt/stratum /srv/stratum
```

Replace `<version>` with the release tag without its leading `v`. The first start downloads and prepares the matching Vintage Story files, so either prepare the install as the service user first or wait for the network to be online before starting the service. `network-online.target` waits only when the system has an enabled wait-online service.

The first start writes patched files into the install directory, so the service user must be able to write there.

`systemctl stop stratum` sends a graceful stop signal and the server saves before it exits. Set `TimeoutStopSec` high enough for your world to save; if systemd's stop timeout expires, it can terminate the process before the save finishes. The example uses five minutes.

## Service file

Create `/etc/systemd/system/stratum.service`:

```ini
[Unit]
Description=Stratum Vintage Story Server
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=stratum
WorkingDirectory=/opt/stratum
ExecStart=/opt/stratum/StratumServer --dataPath /srv/stratum/data --stratum-no-banner
Restart=on-failure
RestartSec=10
TimeoutStopSec=300
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```

Replace `/opt/stratum` with your install directory, `/srv/stratum/data` with your data path, and `stratum` with the system user that should run the server.

Enable and start the service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable stratum
sudo systemctl start stratum
```

Check the status and follow the logs:

```bash
sudo systemctl status stratum
sudo journalctl -u stratum -f
```

Vintage Story also writes files under `Logs` in the data folder, including `stratum-audit.log`. Check those files if you need records beyond the systemd journal.

## Console access

Stratum does not provide an interactive console under systemd. To run commands on a running server, use the in-game staff commands. For one-off console commands, stop the service and run the server by hand:

```bash
sudo systemctl stop stratum
cd /opt/stratum
sudo -u stratum ./StratumServer --dataPath /srv/stratum/data
sudo systemctl start stratum
```

## Scheduled restarts

Run `/restart 30` in game to stop the server after a 30-minute countdown. The command needs the `stratum.restart` privilege. Roles with auto-grant, normally `admin`, do not receive it, so grant it with `/roles grant admin stratum.restart` if admins should use it. The command exits the server process; configure the service's restart behavior if the process should start again.

Thirty seconds before a scheduled restart, players are warned to pick up their items. The items still on the ground are removed at the moment of the restart. This is on by default, and you can turn it off with `Performance.Restart.ClearGroundItemsBeforeStop`.

By default the server exits with code `0` on restart. systemd treats a clean exit as success, so `Restart=on-failure` does not start the server again after `/restart`. To have `/restart` bring the server back, set `Performance.Restart.ExitCode` in `stratum-performance.json` to a non-zero value, such as `1`, and keep `Restart=on-failure`. `Restart=always` also starts the server after a clean stop, which you may not want. The default countdown warns at 300, 120, 60, 45, 30, 15, and 5 seconds and clears ground items 30 seconds before shutdown.

## Firewall

Open the server port. The default port is `42420`, over both TCP and UDP:

```bash
sudo ufw allow 42420/tcp
sudo ufw allow 42420/udp
```

Vintage Story uses TCP for the connection and can use UDP for game traffic. Open both protocols for the server port.

If you changed the port, open that port instead.
