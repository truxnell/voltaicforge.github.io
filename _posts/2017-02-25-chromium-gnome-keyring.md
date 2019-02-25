---
title: "Stop Chromium asking for keyring unlock"
excerpt: "Stopping Chromium asking for gnome-keyring login on launch"
header:
    overlay_filter: 0.5
    overlay_image: /assets/images/chromium-gnome-banner.png
    teaser: /assets/images/chromium-gnome-banner.png
categories:
   - Games
tags:
   - Steam
   - Emulation
---

## Removing keychain login from Chormium

I recently started using (sigh) Chromium over Firefox due to very slow webgl performance (and general overall faster performance)

However, annoyingly it asks for the Gnome keychain to be unlocked upon launch.

A simple fix I found is to simply change the shortcut to


```
chromium --password-store=basic
```

This will drop chromium to a basic password store machanism, so it doesn't use gnome-keyring

Of course, this assumes your not wanting to use the features of chromium to store login details into the OS's password manager. 
However, since you should be using a service like KeePass or LastPass to manage passwords securely, this shouldn't be an issue.

I canged my Cinnamon settings by selecting 'Open Editor' in the Menu options in settings, then finding the chromium shortcut.  I then added the '--password-store=basic' to the shortcut.


![Stopping chromium keyring unlock warning]({{ site.url }}/assets/images/chromium-password-basic.gif)

