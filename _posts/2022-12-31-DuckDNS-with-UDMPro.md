---
title: DuckDNS with UDM Pro
except: "Configuring the UDM PRO to use DuckDNS"
header:
  overlay_filter: 0.5
    overlay_image: assets/images/linux_stock.png
    teaser: assets/images/linux_stock.png
categories:
  - Homelab
tags:
  - DynamicDNS
---

## DuckDNS with Unifi Dream Machine

I recently got a UDM Pro and have just managed to get DuckDNS working.

There is many other suggestions on the internet, for using the webui with 'dyndns' and a variety of settings that should worke.
None did for me as of today, on UDM-Pro software version 1.11.0.

UDM currently uses the software inadyn to provide its ddns capabilities - and it has more providers avaliable that what is exposed in the WebUI.
To get it working I edited the `inadyn.conf` manually via SSH.
(Google `UDM SSH Setup` if you need to setup SSH access to your UDM)

If there is no Dynamic DNS setup in the UDM WebUI it disables the servers, so for now I have created a dummy service in the UDM WebUI

Once SSH'd into the UDM, check what config the file your UDM is running:

```bash
# ps aux | grep inadyn
12799 root     /usr/sbin/inadyn -n -s -C -f /run/ddns-eth8-inadyn.conf
```

In this case, my config file is located at `/run/ddns-eth8-inadyn.conf`

Edit the file (using VI) and add a duckdns config block:
(VI by default doesnt allow editing, press `i` to enter 'interactive' mode to turn it into a text editor)

```text
provider duckdns.org:2 {
    username         = <YOUR_TOKEN>
    password         = noPasswordForDuckdns
    hostname         = <YOUR_DOMAIN>.duckdns.org
}
```

Save and close.  On VI, `ESC` will exit interactive mode, then `:wq` will 'Write and Quit'

This has successfuly got me running with Duck on UDM - but hacky, and wont be part of a backup
