---
title: Network UPS with Synology
except: "Configuring Synology with USB to communicate with Proxmox nodes"
header:
  overlay_filter: 0.5
    overlay_image: assets/images/linux_stock.png
    teaser: assets/images/linux_stock.png
categories:
  - Homelab
tags:
  - UPS
---

## Network UPS with synology

Port 3493

On proxmox, install nut-tools

```bash
apt install nut
```

Synology username and password for ups is:
Device: ups
Username: monuser
Password: secret

Find and uncomment line and fill out details

## /etc/nut/upsmon.conf

```bash
MONITOR ups@192.168.2.253 1 monuser secret slave
```

## /etc/nut/nut.conf

```bash
MODE=netclient
```

Note: Found on the synology in /usr/syno/etc/ups/upsd.user
