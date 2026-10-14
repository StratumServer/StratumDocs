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
sudo mkdir -p /srv/stratum/data
sudo chown -R stratum:stratum /opt/stratum /srv/stratum
```

The first start writes patched files into the install directory, so the service user must be able to write there.

## Service file

Create `/etc/systemd/system/stratum.service`:

```ini
[Unit]
Description=Stratum Vintage Story Server
After=network.target

[Service]
Type=simple
User=stratum
WorkingDirectory=/opt/stratum
ExecStart=/opt/stratum/StratumServer --dataPath /srv/stratum/data --stratum-no-banner
Restart=on-failure
RestartSec=10
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

## Console access

Stratum does not provide an interactive console under systemd. To run commands on a running server, use the in-game staff commands. For one-off console commands, stop the service and run the server by hand:

```bash
sudo systemctl stop stratum
cd /opt/stratum
./StratumServer --dataPath /srv/stratum/data
sudo systemctl start stratum
```

## Scheduled restarts

Run `/restart 30` in game to restart the server in 30 minutes. Players get countdown messages. The command needs the `stratum.restart` privilege. Roles with auto-grant, normally `admin`, do not receive it, so grant it with `/roles grant admin stratum.restart` if admins should use it.

By default the server exits with code `0` on restart. systemd treats a clean exit as success, so `Restart=on-failure` does not start the server again after `/restart`. To have `/restart` bring the server back, set `Performance.Restart.ExitCode` in `stratum-performance.json` to a non-zero value, such as `1`, and keep `Restart=on-failure`. `Restart=always` also starts the server after a clean stop, which you may not want.

## Firewall

Open the server port. The default port is `42420` over TCP:

```bash
sudo ufw allow 42420/tcp
sudo ufw allow 42420/udp
```

The server uses UDP for game traffic. Clients fall back to TCP without it, so open both.

If you changed the port, open that port instead.
