---
title: "Installing Proxmox"
excerpt: "Guide to adding emulators to your Steam Link or Big Picture setup."
header:
    overlay_filter: 0.5
    overlay_image: /assets/images/steam_bigpicutre_retrogaming.png
    teaser: /assets/images/steam_bigpicutre_retrogaming.png
categories:
   - Games
tags:
   - Steam
   - Emulation
---

## Initial configurations

After setup, there is a few things that should be attended to before we go too far into usage.


###  Setup passwordless ssh

By default, Proxmox will accept root logins.  A good first step is to disable root login without a ssh key.  Even better might be to disallow root login and require login to a default user with sudo privelage, but I'm not that concerned with a home lab setup as it is already behind a firewall.

If you dont already have a private key setup on your local PC, you can generate a pair by running the following command in the terminal:

*Local PC*
`ssh-keygen -t rsa`

{% capture imagesrc %}ssh_keygen.png{% endcapture %}
{% capture imagetitle %}SSH Keygen{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

Follow the prompts to generate the pair.  Whilst its more secure to add a password to the key, it is quite convenient to have enter no password so you have complete passwordless entry.  You can choose depending on your security requirements.


This will generate a private and public key pair.  By default, it will be saved to ~/.ssh/, and you will find you now have two new files, `id_rsa` and `id_rsa.pub`

Then, you can copy your ssh public key to the server by running the following command (replacing <PROXMOX-IP> with your servers IP)

`ssh-copy-id root@<PROXMOX-IP>`

Once you enter the root password for Proxmox, this will copy the public pair of your key to the server.  From then, you can login with your ssh key via `ssh root@<PROXMOX-IP>`, again replacing the IP address with your servers.



Digital Ocean have a good tutorial for more information on ssh key pairs.
[https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2]

### netselect-apt
to speed up slow downloads?

```
apt install netselect-apt
netselect-apt
```

### Adding no-subscription repo

/etc/apt/sources.list
```
deb http://ftp.debian.org/debian jessie main contrib

# PVE pve-no-subscription repository provided by proxmox.com,
# NOT recommended for production use
deb http://download.proxmox.com/debian jessie pve-no-subscription

# security updates
deb http://security.debian.org jessie/updates main contrib
```

then update

```
apt update && apt dist-upgrade -y
```

add to .ssh/config
host pve
 user root
 hostname 192.168.2.200
 port 22


 ### fail2ban

 https://forum.proxmox.com/threads/hardening-proxmox-security-best-practises.19286/