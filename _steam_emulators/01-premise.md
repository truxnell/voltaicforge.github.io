---
title: "Setting up emulators in Steam"
permalink: /guides/steam_emulators/premise/
header:
    overlay_filter: 0.5
    overlay_image: /assets/images/steam_bigpicutre_retrogaming.png
comments:true
---

## Premise

I have setup my main television at home to be an all-in-one entertainment solution.  It has Youtube, Plex, and live TV, which covers 99% of my viewing needs.
To round this out, I've set up a [Steam Link] to run my games from my gaming PC.  This is an amazing setup which removes almost all reasons for owning a current gen console (except exclusive, but it's a small price to pay)

But what about old consoles, Super Nintendo, Atari 2600, etc?

## Outcome

By the time we're done, we will have the ability to add any old consoles games to steam as an individual game, by a non-steam shortcut.  This means that we will have a game listing for each individual game in steam, which we can then launch.  It will drop into the game fullscreen and ready to play, neat, tidy, using Steam's existing interface.

This, of course, means that it also works in big picture and steam link, meaning you can sit on your couch with a Xbox/[Steam Controller] and drop into any game of your choosing.  You can also use categories to filter the steam games list by console type, and also have grid art for steam for each game.

{% capture imagesrc %}steam_bigpicutre_retrogaming.png{% endcapture %}
{% capture imagetitle %}Steam Big Picture list with many super nintendo games{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

We can do this by using a variety of emulators, with a GitHub project [Ice] as the program that automatically adds all the games to steam.  You need only maintain a folder with the ROM's you want to have added to steam in them, and when run Ice will scan for games in the folders, and update shortcuts in steam with entries for each game.  It will build for each steam shortcut the correct command to launch the emulator and pass the correct arguments to launch the game.

This guide is aimed at Windows, however, I believe the [Ice] utility will run on Linux as well.

For setup, we need to:

* Install Ice
* Install Emulators
* Customise the Ice config for each game system and emulator
* Copy ROM's to folder structure
* Run Ice with steam closed
* Play old games using the Steam frontend.


[Ice]: http://scottrice.github.io/Ice/
[Steam Link]: {% post_url 2016-07-24-steam-link-ultimate-console %}
[Ice Github]: https://github.com/scottrice/Ice
[RetroArch]: https://www.libretro.com/
[LaunchBoxForumPost]:https://forums.launchbox-app.com/topic/28762-command-line-parameters-arguments/
[PCSX2]: http://pcsx2.net/
[Dolphin]: https://dolphin-emu.org
[Steam Controller]: {% post_url 2016-09-18-steam-controller-worthy-addition %}