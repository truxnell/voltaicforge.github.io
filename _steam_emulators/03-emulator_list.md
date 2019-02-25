---
title: "Setting up emulators in Steam"
permalink: /guides/steam_emulators/emulator_list/
header:
    overlay_filter: 0.5
    overlay_image: /assets/images/steam_bigpicutre_retrogaming.png
---

#### Emulator list

Note, this is only a guide for some suggestions - there is a selection of emulators and your favourite game may work better in a less popular emulator.  Don't feel bound by this list.


| Console     | Emulator        | Command Arguments |
|-------------|-----------------|-----------------|
| NES | [RetroArch] (nestopia UE core)| -f -L d:\Emulation\RetroArch\cores\nestopia_libretro.dll %r | 
| SNES | [RetroArch] (bsnes balanced Core) | -f -L d:\Emulation\RetroArch\cores\bsnes_balanced_libretro.dll %r |
| Gamecube | [Dolphin] |  --batch --exec=%r | 
| Wii | [Dolphin] |  --batch --exec=%r | 
| PS1 | [RetroArch] (mednafen psx core)| -f -L d:\Emulation\RetroArch\cores\mednafen_psx_libretro.dll %r |
| PS2 | [PCSX2] | --nogui --fullscreen --fullboot %r|
| SEGA Genesis | [RetroArch] (genesis plus core) | -f -L d:\Emulation\RetroArch\cores\genesis_plus_gx_libretro.dll %r |
| N64 | [RetroArch] (mupen64plus core) | -f -L d:\Emulation\RetroArch\cores\mupen64plus_libretro.dll %r |
| Atari 2600 | [RetroArch] (stella core) | -f -L d:\Emulation\RetroArch\cores\stella_libretro.dll %r |
| Atari 7800 | [RetroArch] (prosystem core) | -f -L d:\Emulation\RetroArch\cores\prosystem_libretro.dll %r |

Another list can be found at the [BigBox forums][LaunchBoxForumPost].


[Ice]: http://scottrice.github.io/Ice/
[Steam Link]: {% post_url 2016-07-24-steam-link-ultimate-console %}
[Ice Github]: https://github.com/scottrice/Ice
[RetroArch]: https://www.libretro.com/
[LaunchBoxForumPost]:https://forums.launchbox-app.com/topic/28762-command-line-parameters-arguments/
[PCSX2]: http://pcsx2.net/
[Dolphin]: https://dolphin-emu.org
[Steam Controller]: {% post_url 2016-09-18-steam-controller-worthy-addition %}