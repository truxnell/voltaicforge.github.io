
---
title: "Proxmox VE install"
excerpt: "My steps for installing proxmox VE"
header:
    overlay_filter: 0.5
    overlay_image: /assets/images/
    teaser: /assets/images/
categories:
   - Homelab
tags:
   - Proxmox
---

## R710

### Fan Mod

### Update drivers

### Fix Virtual Console


## Installing Proxmox

###  Setup passwordless ssh

https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2

local pc
`ssh-keygen`
copy local .ssh/id_rsa.pub into file




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

### Updating container list

```
pveam update

```

Runs once a day anyway


### Container list

Local (pve) -> Content -> Templates


### Logging into container

* vnc
* pct (cli)
* webui cli

## VM's

### Controlling VM.

```
qm start 100
qm stop 100
qm shutdown 100
qm reset 100
qm resume 100
qm suspend 100
qm destroy 100
```


### Downloading Templates


### Creating Containers


- unprivelaged

### Controlling containers

```pct start/stop/etc/destroy```

### Auto update lxc

https://github.com/morph027/pve-lxc-scripts
```
cd /opt/
git clone https://github.com/morph027/pve-lxc-scripts
cp lxc* /usr/local/sbin/
```

run 'lxc-update-all'


### Setting up stress test

```
apt install lm-sensors
sensors-detect
watch sensors
```

create container, crank up to 11


```
apt install stress stress-ng

stress-ng --cpu 8 --io 2 --vm 1 --vm-bytes 4096 --timeout 600s --metrics-brief

### Setting up docker

Install Ubuntu 16.04 vm

install htop, glances, etc

put in nfs mounts

install docker

add user to docker group (optional)

add dockers (linuxserver.io)

### Setting up Sophos


create vm (2cpu, 4096 ram, 70gb hdd)

probs leave e1000
ensure linux 2.4?  not top one, hdd won't detec.

install sophos

have fun :/

## setting up unifi controller

LXC - Ubuntu 16.04 template
1 core, 1 cpu, 512mb ram, 5gb hdd


https://help.ubnt.com/hc/en-us/articles/220066768

/etc/apt/sources.list.d/100-ubnt.list
```
## Debian/Ubuntu
# stable => unifi4
# deb http://www.ubnt.com/downloads/unifi/debian unifi4 ubiquiti
# deb http://www.ubnt.com/downloads/unifi/debian unifi5 ubiquiti

deb http://www.ubnt.com/downloads/unifi/debian stable ubiquiti

# oldstable => unifi3
# deb http://www.ubnt.com/downloads/unifi/debian unifi3 ubiquiti
# deb http://www.ubnt.com/downloads/unifi/debian oldstable ubiquiti
```

`sudo apt-key adv --keyserver keyserver.ubuntu.com --recv 06E85760C0A52C50`
`sudo apt update`
`sudo apt install unifi`

## Install mongodb

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
sudo echo "deb http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.3 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.3.list
sudo apt-get update
sudo apt-get install mongodb
```


Check running

`systemctl status unifi`

`netstat -tulpn`

lxc -> start at boot -> yes

Browse to ip:8443

If reinstalling, AP might dissapear - finish setup and 'adopt' it.





### Setting up bind mounts

```pct set 101 -mp0 /media/downloads/,mp=/media/downloads```


## Setting up nzbget

follow instructions at  https://github.com/nzbget/nzbget/wiki/Installation-on-Linux
bind downloads folder
edit config

http://forum.nzbget.net/viewtopic.php?f=8&t=2709
/lib/systemd/system/nzbget.service
```
[Unit]
Description=NZBGet Daemon
Documentation=http://nzbget.net/Documentation
After=network.target

[Service]
User=<replace_with_the_user_you_want>
Group=<replace_with_the_group_you_want>
Type=forking
ExecStart=</path/to/nzbget/nzbget> -D
ExecStop=</path/to/nzbget/nzbget> -Q
ExecReload=</path/to/nzbget/nzbget> -O
KillMode=process
Restart=on-failure

[Install]
WantedBy=multi-user.target

----

systemctl enable nzbget
systemctl start nzbget
```

## Setting up Radarr

https://github.com/Radarr/Radarr/wiki/Installation#manually-install-radarr

```
apt update && apt install libmono-cil-dev curl mediainfo

find link for latest release: https://github.com/Radarr/Radarr/releases

cd /opt
wget <LATEST_RELEASE_URL>
tar -xvzf Radarr.develop.0.2.0.99.linux.tar.gz


## Setup systemctl

/lib/systemd/system.Radarr.service
```
[Unit]
Description=Radarr Daemon
After=syslog.target network.target

[Service]
User=user
Group=group

Type=simple
ExecStart=/usr/bin/mono --debug /opt/Radarr/Radarr.exe --nobrowser
TimeoutStopSec=20
KillMode=process
Restart=always

[Install]
WantedBy=multi-user.target

---

systemctl enable Radarr
systemctl start Radarr

```

## Setting up plex

```apt install git```
grab this - https://github.com/mrworf/plexupdate
run it, y to pretty much all

plex will be installed for you

plexpy - 

https://github.com/JonnyWong16/plexpy/

setup as daemon

/lib/systemd/system/plexpy.service
https://github.com/JonnyWong16/plexpy/blob/master/init-scripts/init.systemd

### Setting up phpIPAM

setup (LAMP)
https://techpolymath.com/2015/04/06/how-to-setup-phpipam-on-ubuntu-14-04/

then automate scans
https://phpipam.net/automatic-host-availability-check/

### Glances


### Cockpit

http://www.itzgeek.com/how-tos/linux/ubuntu-how-tos/install-cockpit-on-ubuntu-16-04.html

```apt install software-properties-common 

sudo add-apt-repository ppa:cockpit-project/cockpit


sudo apt-get update


sudo apt-get -y install cockpit


sudo systemctl start cockpit
sudo systemctl enable cockpit
```