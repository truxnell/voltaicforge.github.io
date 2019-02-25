---
title: "Fixing Intel Hardware decoding on Linux (Ubuntu)"

excerpt: "Fixing steams broken libav libraries that break hardware accelerarion on Steam inhome streaming"
header:
    overlay_filter: 0.5
    overlay_image: assets/images/linux_stock.png
    teaser: assets/images/linux_stock.png
categories:
   - Linux
tags:     
   - Steam
   - Linux
   - VAAPI
---    

## Fix for lack of hardware decoding on Linux

So I've switched to Linux recently (Mint 18).  I realised that since I was moving to Steam in-home streaming, I could move my clunky desktop tower out into the living room/server closet and run Linux on my desktop.  The only reason I've kept Windows running for so long was for gaming needs (and not wanting to fiddle around getting games to work on Linux)

Thankfully, with Steams push into the Linux front and throwing its weight behind the Debian side, gaming in Linux is a thing.  But it only supports about ~30% of my library, so my gaming PC needs to stay Windows.

After years of running Debian servers at home (and Freenas for years before that) linux feels more comfortable than Windows.  So I build a new mini-ITX, Intel i5 PC for desktop use, and moved the tower out into the living room.

Unfortunately, my experience with steam streaming was jelly mouse!  A quick check with setting 'Display Performance information' in the in home streaming client area revealed it was not using hardware acceleration.  It was using 'libavcodec', which is a software decoder.

{% capture imagesrc %}Steam_Advanced_Client_Options.png{% endcapture %}
{% capture imagetitle %}Steam Controller Unboxing{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}
**Note:** Check the 'Display performance information box' to enable perfomance info, and press F6 ingame to check.  Also, ensure you have 'Enable Hardware decoding' checked on the client.
{: .notice--info}

This introduced a bit of lag in the decoding (>50ms) which I could definitely notice.

## The fix

After much googling, the problem is that Steam on Ubuntu is bundled with its own libraries for VA-API.  Unfortunately, it is bundled with old drivers that no longer work/are broken.  The solution is to download the drivers from Ubuntu/your distros repo, remove Steams copy and `ln` (symlink) them to the host's updated drivers.

Simply put, drop into terminal and `apt` the below:

```bash
apt install i965-va-driver:i386
apt install libva1:i386
apt install libva-x11-1:i386
apt install libva-glx1:i386
```

This will install the proper drivers into `/usr/lib/i386-linux-gnu`.

Remove the old drivers from Steams folder (or `mv` them if you prefer)

```
rm ~/.steam/ubuntu12_32/steam-runtime/i386/usr/lib/i386-linux-gnu/libva*
```

The last step is to symlink these to Steam's local folder.  You can `ln -s` if you wish, but a kind folk has created a bash script to do it for us!

Grab the gist below (kindly created by Github user [Benleov](https://gist.github.com/benleov)).  Alternatively, `wget` it with the below shell command:

```
wget https://gist.githubusercontent.com/benleov/dc44ca9505a807e7dcc9/raw/673a82ff3d2356d27c9355088f58335bc89ab46b/fix_steam_links && chmod +x fix_steam_links
./fix_steam_links
```
**Warning:** Always check out the code that you are downloading code before executing to ensure its not malicious!
{: .notice--info}

{% gist dc44ca9505a807e7dcc9 %}



