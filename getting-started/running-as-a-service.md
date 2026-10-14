---
title: "Running as a service"
description: "Run Stratum under systemd so it starts at boot and restarts after a crash."
category: "Getting Started"
categoryOrder: 1
order: 4
---

This page shows how to run Stratum as a background service on Linux, so it starts at boot and restarts after a crash. The examples use systemd.

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

## Permissions

The service user must own the install directory and the data directory:

```bash
sudo chown -R stratum:stratum /opt/stratum /srv/stratum
```

The first start writes patched files into the install directory. If the service user cannot write there, the first start fails.

## Console access

Stratum does not provide an interactive console under systemd. To run commands on a running server, use the in-game staff commands. For one-off console commands, stop the service and run the server by hand:

```bash
sudo systemctl stop stratum
cd /opt/stratum
./StratumServer --dataPath /srv/stratum/data
sudo systemctl start stratum
```

## Scheduled restarts

Run `/restart 30` in game to restart the server in 30 minutes. Players get countdown messages.

By default the server exits with code `0` on restart. systemd treats a clean exit as success, so `Restart=on-failure` does not start the server again after `/restart`. To have `/restart` bring the server back, set `Performance.Restart.ExitCode` in `stratum-performance.json` to a non-zero value, such as `1`, and keep `Restart=on-failure`. `Restart=always` also starts the server after a clean stop, which you may not want.

## Firewall

Open the server port. The default port is `42420` over TCP:

```bash
sudo ufw allow 42420/tcp
```

If you changed the port, open that port instead.
