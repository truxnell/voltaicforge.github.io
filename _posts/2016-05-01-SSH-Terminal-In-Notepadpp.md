---
title: "Docking a PuTTY window into Notepad++"

excerpt: "Docking a PuTTY window into Notepad++ is a big help when developing a Jekyll blog in a Windows environment - when you have outsourced the Jekyll build to a local Linux server"
header:
    overlay_filter: 0.5
    overlay_image: assets/images/putty_teaser.png
    teaser: assets/images/putty_teaser.png
categories:
   - Software
tags: 
   - Jekyll
   - Notepad++
---


## Challenge of developing in Jekyll via remote Linux server

During development of this site, I came across too many limitations with using Jekyll on windows. You can use Jekyll in windows with [PortableJekyll][PortableJekyll], but I found the Windows environment still limited Jekyll. Mainly in regard to ImageMagick for using responsive images, which isn’t worth the hassle of setting up on windows. I decided it was easier to spool up a Debian VM on my virtual server to install Gem/Ruby/Jekyll on. This lets me keep the website code on my NAS (allowing for easy cloud backup), code on Windows, and run Jekyll on Linux.

However, swapping back and forward between a terminal and the website code to keep an eye on the site build was annoying, so I decided to see if I could ‘dock’ a PuTTY terminal into Notepad++

## Adding a Putty Terminal into Notepad++

I’ve been using [Notepad++][Notepadpp] to develop in Windows for quite some time now. It’s a fantastic little program for us Windows people. A docked PuTTY window would greatly improve my workflow.

    
{% capture imagesrc %}putty_terminal_in_npp2.png{% endcapture %}
{% capture imagetitle %}Picture of Notepad++ with attached PuTTY terminal{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

To do the same as I have above, it’s actually pretty simple.

Download:

* [Plink][plink] - PuTTY command line
* [Ansicon][ansicon] - Ansi escape sequence converter (to convert Linux CLI escape codes to windows CLI, else the terminal doesnt work very well)
* [nppconsole][nppconsole] - Notepad++ console plugin.

**Note:** I had to use the older `11.53` ansicon version under Windows 10 x64, the later versions crashed frequently.
{: .notice--info}

The do the following

1.  Extract *NPPConsole* to your Notepad++ plugins directory (Program Files x86/Notepad++/Plugins)
2.  Restart Notepad++ if you had it running
3.  Unzip and copy *plink* and *ansicon* to a folder of your choosing (I put them both in the Notepad++ folder as its where the CLI defaults to and I’m lazy)
4.  Open a CLI at the Notepad++ folder and run `ansicon.exe plink.exe -ssh user@location -pw yourpasword`, replacing the relevant parts with your user/server IP/password.
5.  All going well, you can now replicate that command in Notepad++. Open Notepad++, select *Plugins -> NppConsole* (should be there if you restarted and copied the dll correctly). Run the command and you are set!

If you have a saved profile in PuTTy you can replace the command with  `ansicon plink -ssh SESSION_NAME` , replacing `SESSION_NAME` with your sessions name from the PuTTY dialogue. This works with key authentication as well, which I much prefer to use. This also saves you the risk of keeping a plaintext password in a potentially insecure location.

**Note:** You can use the plugin Nppexec as well instead of NppConsole - but i prefer Nppconsole for this.
{: .notice--info}

You now have a working CLI in Notepad++ that can call an SSH session. I wrote a few batch scripts, so I just have to run `site-dev` in Windows to jump to my Linux VM that contains Jekyll, then  `./site-dev` to run a bash script to call up Jekyll in dev mode. I already have a `site-push`  bash to push the site to Amazon AWS, so I don’t need to leave Notepad++ to do any development work.

**Warning:** I couldn’t get Ctrl-C to work with either NppConsole or NppExec, both terminate the command without sending SIGTERM or similar to the PuTTY. Let me know if you figure out a way around this, as it means it’s difficult to gracefully terminate Jekyll. Workaround below.
{: .notice--warning}

## Workaround for no Ctrl-C to exit Jekyll

My workaround for this has become to have a PuTTY terminal open just for stopping Jekyll. This isn’t as bad as it sounds. Just write a bash script to kill ruby as follows:

```
pgrep ruby | xargs kill
```


**Warning:** This will kill all ruby threads! Note this if you often have other ruby threads often running
{: .note--warning}

I saved it as `site-kill`. My workflow is `site-dev` for the development of this site (with dev flags for config, etc), `site-push` to rebuild the site from scratch and push to AWS. The new  `site-kill` kills off any ruby threads.


{% capture imagesrc %}ctrl_c_workaround.png{% endcapture %}
{% capture imagetitle %}Picture of using external ssh script to kill jekyll window instead of killing notepad++ CLI{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}



## Bonus - Markdown highlighting in Notepad++

The default colours finally made me crack during writing this post. Too much white! The language highlighting file I had for markdown didn’t play ball with custom styles.

I found a language highlighting file for Markdown on [GitHub][markdownhighlight] that had a file for the Zenburn colour scheme. This is what you can see in the screenshots above. So much better!


[portablejekyll]:https://github.com/madhur/PortableJekyll
[notepadpp]:https://notepad-plus-plus.org/
[plink]:http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[ansicon]:https://github.com/adoxa/ansicon/downloads
[nppconsole]:https://sourceforge.net/projects/nppconsole/
[markdownhighlight]:https://github.com/Edditoria/markdown_npp_zenburn