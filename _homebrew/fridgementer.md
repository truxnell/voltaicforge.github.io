---
title: "Fridgementer"
permalink: /projects/homebrew/fridgementer/
header:
    overlay_filter: 0.5
    overlay_image: /assets/images/homebrew_fermenter.png
---

# Why a fermenter fridge?

I always wanted to build a fermenter fridge for my homebrewing.  I brewed for nearly 2 years, with a variety of methods for controlling the temperature of the fermenting wort that weren't great.

Beer fermenter are usually big plastic buckets which hold 10-23 liters of ready to be brewed beer (called wort).  Once the perpetration part of the beer making process is complete, the wort is poured into a fermenter, and yeast is added.

However, yeast is a fickle organism and requires a consistent temperature it's happy with to ferment properly (amongst other things).  Variations in temperature put the yeast under stress, and can make it produce 'off flavours'.  As such, controlling the temperature of the fermenter is important, and is one of the easiest improvements that can be made to a beginners beer making process.

Whilst in Hobart, I used a large bucket of water with an aquarium heater to control temperatures of the fermenter.  This worked OK as it was generally fairly cold down there, and I only required to keep it heated to around the right temperature.

In Melbourne however, its harder to control fermentation temperature as the temperatures are warmer.  You need to start considering cooling it when it gets to warm, and then warming it when it gets too cold.

The general gold standard for this process is converting a fridge or freezer into a temperature controlled chamber.  Using a temperature probe and a temperature controller like the STC-100, which can turn on the fridge when the beer is higher than a set temperature, or turn on a heating device inside the fridge when its too cold.

This is perfectly fine, but a solution is available that panders to the Linux-loving, electronics dork that I am - the [BrewPi](https://www.brewpi.com/) project.  This provides the ability to hack a fridge to control heating/cooling as required, but also uses PID[^PID] to keep the fermentation temperature extremely accurate, whilst providing the ability to have temperature profiles and a web interface.

# Parts

| Part | Source | Price
|-|-|-
| Pi Zero W | Ebay | $37.95 |
| Duinotech Classic (Arduino Uno clone) | Jaycar | $29.95 |
| Inkbird 40A SSR (x2) | Ebay | $21.98 |
| DS18B20 Temperature Sensors (x5) | Ebay | $14.90 |
| Micro Usb-B Male to Usb Female | Spare | Free |
| Fridge | Ebay | $120 |
| 4 core wire | Jaycar (WB1590) | $2 |
| Mic Panel Connector Male | Jaycar (PP2010) | $6 |
| Mic Panel Connector Female | Jaycar (PS2012) | $6 |

**Note**: Only need 2-3 sensors, but packs of 5 are common

# Install BrewPi

## Install Raspbain to Pi

Download and install [Raspbian](https://www.raspberrypi.org/downloads/raspbian/) to a SD card.  I won't cover this as there is a million tutorials on the net for this.  Offical installation instructions [here](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)

I use the Cinnamon Disk Image Writer under Linux, or `dd` under a terminal.  Under Windows [Win32DiskImager](https://sourceforge.net/projects/win32diskimager/)

{% capture imagesrc %}raspbian_install.png{% endcapture %}
{% capture imagetitle %} Installing Raspbian using Cinnamon Disk Image Writer {% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

## Setup Wifi

As I am using a Raspberry Pi W which has built in WiFi, I want to setup Wifi out of the box.  Thankfully, you can setup Wifi directly into the SD card before you even fire it up, meaning you can skip the tedium of having to plug it into Ethernet, SSH into it, setup Wifi, then move it to it's headless loaction.

To do this, ensure the SD card with your fresh copy of Raspbian is plugged in and mounted.  Go into the boot partition and create a file called `wpa_supplicant.conf`.  This file is referenced in the `/etc/network/interfaces` file, meaning you can enter the credentials into this `wpa_supplicant.conf` file and it will automatically use them on boot.

In this file, enter the following lines, ensuring to replace the `ssid` and `psk` variables with your own Wifi's SSID and WPA password.

*wpa_supplicant.conf*

```
network={
    ssid="SSID"
    psk="password"
    key_mgmt=WPA-PSK
}
```

## Enable SSH

Currently, SSH is disabled by default on a fresh Raspbian install, which is a good security measure.  However, we will want this enabled to connect to our headless device.

Simply create a new file on the boot partition named `ssh`.  This will ensure SSH is enabled when you boot up.

{% capture imagesrc %}setup_wifi_ssh_raspi_filebrowser.png {% endcapture %}
{% capture imagetitle %} Files for Wifi and SSH enabling in Raspi zero W boot folder {% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

## Log into the PI

Unmount the SD card, plug it into your Pi, and power it on via the usb power cable (or GPIO if thats your plan).

After it finishes booting you will be able to SSH into it via `ssh pi@raspi-ip-address` or using [PuTTY](http://www.putty.org/) for Windows.  You could check your Router's DHCP lease to find its IP, or try using `raspi`.  We will give it a static IP address in a subsequent section.

{% capture imagesrc %}raspi_zero_w.jpg {% endcapture %}
{% capture imagetitle %} Rasberry Pi Zero W {% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

## Update system

Now to update the Rasbian OS to latest, to ensure we are up to date.  Always update a fresh Linux OS as soon as you boot it, even if its a fresh download!

```sudo apt update && sudo apt dist-upgrade```

{% capture imagesrc %}raspi_apt_update_apt_dist-upgrade.png {% endcapture %}
{% capture imagetitle %}Updating a Raspberry Pi with sudo apt update && sudo apt dist-upgrade. {% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

## Set static Wlan IP (Optional)

This is an optional step, however currently the Pi is setup for DHCP, and the address may change.  This may be frustrating if you want to open it up to the outside world or want to ensure your bookmark for it stays the same.  You may prefer to use your router's DHCP system to give it a static reservation instead of the below.

If you wish to proceed and force the Pi to have a static IP that doesn't change., edit the `/etc/network/interfaces` file to change the correct `iface` line to static, and give it static details as below.

To ensure you get the right wlan entry if you have multiple entries, you can use `ifconfig` to see which wlan is being used.

Replace the line `iface wlan0 inet manual` with the below details, substituting the IP's with your desired static address (in the address line) and the correct netmask and gateway for your network.  It's best to pick a IP outside your router's DHCP range if possible to avoid potential conflicts.

Open the `/etc/network/interfaces` file, with `sudo nano /etc/network/interfaces`.

*/etc/network/interfaces*
```
iface wlan0 inet static
    address 192.168.2.200
    netmask 255.255.255.0
    gateway 192.168.2.254
```

For example, my full `interfaces` file is as follows.

```
# interfaces(5) file used by ifup(8) and ifdown(8)

# Please note that this file is written to be used with dhcpcd
# For static IP, consult /etc/dhcpcd.conf and 'man dhcpcd.conf'

# Include files from /etc/network/interfaces.d:
source-directory /etc/network/interfaces.d

auto lo
iface lo inet loopback

iface eth0 inet manual

allow-hotplug wlan0
iface wlan0 inet static
    address 192.168.2.200
    netmask 255.255.255.0
    gateway 192.168.2.254
    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf

allow-hotplug wlan1
iface wlan1 inet manual
    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
```

## Change Pi password (Optional, Recommended)

Next, change the Pi's default password to a new one.  With a default password and SSH enabled, the Pi is less secure and potentially a victim to automated hacking attemps utilizing default password tables.  Changing the password is one step that absolutely should be done on any Pi regardless of use, as well as any system that ships with a default password.

To change it, simply run `passwd` and enter the default password `raspberry` and your new password.

{% capture imagesrc %}raspi_ssh_passwd.png {% endcapture %}
{% capture imagetitle %} Raspberry Pi login to SSH and use PASSWD to change default password for security {% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

### Setup SSH passkey & disable password access (Optional, Recommended)

It is best to secure SSH using a passkey, and then setup the Pi to accept a login using it.

I wont rehash how to create one here, as there are ample guides on the net for creating one locally.  Note these guides are run on your *local* computer, not the Pi!

**Linux / Mac**
(https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets)

**Windows**
(https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-putty-on-digitalocean-droplets-windows-users)

Then the next step is to get the passkey onto the Pi.

**Windows**

Log into the Pi with PuTTY, and then execute

```
mkdir -p ~/.ssh
cd ~/.ssh
sudo nano ~/.ssh/authorized_keys
```

Then copy the passkey from your `.pub` file from your local comptuer into the Pi's remote Putty window direct into `nano`.

**Linux/Mac**
From your local computer
```
ssh-copy-id pi@raspi-ip-address
```
If using mac, you may need to install ``ssh-cop[y-id`` using Homebrew `brew install ssh-copy-id`.

{% capture imagesrc %}ssh-copy-id.png {% endcapture %}
{% capture imagetitle %} Using SSH-COPY-ID to copy a ssh key onto a remote server {% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

You will now be able to log into the Raspi without using a password.

Once you confirm that it is working, disable SSH password autentication into the PI

On the Pi, modify the `/etc/sshd/ssh-config` file.  There will be an entry for password authentication already there.  Simply remove the comment and change the 'yes' to 'no'.

*/etc/ssh/ssh_config*
Change the line `#   PasswordAuthentication yes` to `PasswordAuthentication no`

![Animation of disabling SSH password autentication]({{ site.url }}/assets/images/sshd_disable_pasword_authentication.gif)
{: .text-center}

### Setup fail2ban (security)

* only if using password ssh!

```
sudo apt-get install fail2ban

sudo nano /etc/fail2ban/jail.local

[ssh]

enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
bantime = 900
banaction = iptables-allports
findtime = 900
maxretry = 3
```



## Unattended upgrades (security)

```
sudo apt-get install unattended-upgrades

sudo nano /etc/apt/apt.conf.d/50unattended-upgrades

add:
"o=Raspbian,n=jessie,l=Raspbian-Security";

```

file example
```
Unattended-Upgrade::Origins-Pattern {
// Codename based matching:
// This will follow the migration of a release through different
// archives (e.g. from testing to stable and later oldstable).
"o=Raspbian,n=jessie,l=Raspbian-Security";

// Archive or Suite based matching:
// Note that this will silently match a different release after
// migration to the specified archive (e.g. testing becomes the
// new stable).
// "o=Raspbian,a=stable";

};


```

# Installing BrewPi onto Raspbian

## Change hostname

```
sudo nano /etc/hosts

---replace

127.0.1.1	raspberrypi

with 

127.0.1.1   brewpi
```

```
sudo nano /etc/hostname

replace with brewpi

sudo nano /etc/init.d/hostname.sh

sudo reboot
```

might be reboot before hostname.sh?  

## Install brewpi

Instructions from http://docs.brewpi.com/en/latest/index.html
Always check install scripts!

```
sudo apt install git

git clone https://github.com/BrewPi/brewpi-tools.git ~/brewpi-tools

sudo ~/brewpi-tools/install.sh

sudo ~/brewpi-tools/updater.py --ask

`select legacy branch

```

{% capture imagesrc %}brewpi_webui_startup.png {% endcapture %}
{% capture imagetitle %} BrewPI's defauly WebUI {% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

## Reboot

```
sudo reboot
```

## (Linux) Add to .ssh/config file

For neatness

```
nano .ssh/config


+++
host brewpi
 hostname 192.168.2.200
 port 22
 user pi

```

## flash hex

latest legacy : https://github.com/BrewPi/firmware/releases/tag/0.2.10

may need to `sudo echo 'E\n' > /dev/ttyACM0` before it will run
also may need to `sudo apt-get install arduino-core`

{% capture imagesrc %}brewpi_program_legacy_arduino.png {% endcapture %}
{% capture imagetitle %} Programming a Arduino with a legacy brewpi version {% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}



[^wort]: Wort is 
[^PID]: 'proportional–integral–derivative', a method of using a control loop feedback to learn how a system reacts to keep a setpoint constant.
