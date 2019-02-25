---
title: "SCP upload to Octoprint on Linux"
excerpt: "Uploading via SCP automatically to Octoprint on Linux"
header:
    teaser: "assets/images/octopi.jpg"
    overlay_filter: 0.5
    overlay_image: "assets/images/octopi.jpg"
categories:
   - 3DPrinting
tags:
   - octoprint

---


## Uploading automatically to Octoprint via bash & scp

I've been using [Simplify3d](https://www.simplify3d.com/) for my 3D printing, and [Octoprint](http://octoprint.org/).  Octoprint has a very nice web interface, and between it and the advancement in slicers, its been great to not have to slice, carry SD card over to the printer, load via the LCD, etc.

However, I've always wanted a auto-upload feature.  I've chosen to do it via setting up ssh keys for logging into the Octoprint server, and using ```scp``` to upload the file automatically.

I did get the idea from a post at [https://forum.simplify3d.com/viewtopic.php?f=23&t=1924](https://forum.simplify3d.com/viewtopic.php?f=23&t=1924)

I've setup ssh keys for passwordless entry to my Octoprint server (Google SSH keys for more info, or perhaps check out the [Digital ocean FAQ](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2) )

#### Local PC

When ```ssh-keygen``` asks for input I entered through it to accept defaults, and no passphrase.
```
nat@nat-desktop ~ $ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/nat/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/nat/.ssh/id_rsa.
Your public key has been saved in /home/nat/.ssh/id_rsa.pub.
The key fingerprint is:
<redacted>
The key's randomart image is:
<redacted>
```

We now have a private key in ```~/.ssh/id_rsa``` and a public key in ```~/.ssh/id_rsa.pub```.

I logged into the remote PC (Octoprint) to copy the public key.  I chose to open up the ```~/.ssh/id_rsa.pub``` file in ```nano``` and copy it via clipboard, but you could use ```scp```, ```ftp``` or any other method.

#### Remote PC
```
pi@octopi:~ $ nano .ssh/authorized_keys

<append contents of local ~/.ssh/id_rsa.pub file>

pi@octopi:~ $ chmod 700 .ssh/authorized_keys 

```

Now we should be OK to login with no password to the Octoprint server

```
nat@nat-desktop ~ $ ssh pi@192.168.1.9

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Thu Jan 19 11:52:32 2017 from 192.168.1.7
pi@octopi:~ $ 
```

**Note:** You can setup bookmarks for `ssh` via `ssh_config`.  This enables you to quickly ssh into servers quickly without remembering IP's or usernames, especially coupled with ssh_keys for password less entry.  Check out ```man ssh_config``` or Google for more info.
{: .notice--info}

Once that is setup, we can easily `scp` the file to Octoprint.  Setup a bash file with the one liner below.

#### ~/scripts/putpi.sh
```
scp "$1" pi@192.168.1.9:.octoprint/uploads/
```

Remember to `chmod +x putpi.sh` to ensure the file is executable.  Also, substitute the IP above for your Octoprint's IP (or hostname).

We can ensure it works by running it manually.

```
nat@nat-desktop ~/scripts $ ./putpi.sh ~/Documents/3dPrinting/gcode/nat.gcode 
nat.gcode                                       100% 1477KB   5.9MB/s   00:00
```
And refreshing the file list on the octoprint will make it appear.

{% capture imagesrc %}octoprint_upload_refresh.png{% endcapture %}
{% capture imagetitle %}Octoprint refreshing webui{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

Now, to automate the upload from Simplify3D.  Select your 3D Printers process settings is selected in the list box to the left and go to the 'Edit process settings' tab at the lower left corner of the main screen.  Ensure the advanced settings are visible in the box, if not click the 'Show Advanced' Button.

Here, select the 'Scripts' tab, and in the bottom of the box underneath 'Post Processing' and 'Additional terminal commands for post processing' enter the command to call your bash script.


```
~/scripts/putpi.sh [output_filepath]
```

{% capture imagesrc %}simplify3d_scp_upload_to_octoprint.png{% endcapture %}
{% capture imagetitle %}Simplify3D script for scp upload to Octoprint{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

Then when you save the gcode in Simplify3D, it will call the script and upload the resulting GCode to Octoprint!  Just refresh the file, review and print.

**Note:** You need to actually save the gcode file somewhere on your computer before the post processing (and thus file upload) will occur.
{: .notice--info}