# Running Skyrim/Requiem/Ultimate under Linux/Wine

After years away from Skyrim and Requiem, I had a hankering to play again, but as a Linux gamer and developer I realised that I'd never actually play if I ever had to reboot to Windows. I've been successful in this quest, and have managed to get a very successful and stable Requiem install running. I imagine this would also be useful for other mods involving running SkyProc patchers, such as SkyRe or PerMa.

Here I record the steps I've taken and the victories I've had, so that those who come after me don't have to repeat all the mistakes I've made. Since the person most likely to need this will be myself in two years time, this is pretty much a love letter to my future-self.

While this was originally a Requiem guide, I've expanded it to also cover BelmontBoy's [Ultimate Skyrim](http://ultimateskyrim.bitballoon.com).

## Skyrim under Linux with wine: What I have working:

- Skyrim with all the DLC.
- ModOrganizer.
- Requiem, including the Reqtificator and all dependencies.
- TES5Edit v3.1.3
- FNIS
- RealShelter
- Automatic Variants
- Ultimate Skyrim
- LOOT
- ENB (not documented here)
- Numerous various mods and enhancements (not documented here).

Everything is stable, solid, and feature complete, with only a couple of exceptions that I'll detail below.

I've not tried to get DynDOLOD or other tools running. I've heard others have had success with them, but I have no experience to share here.

## Vorgen's Guide

If you haven't found it, /u/Vorgen's [guide](https://www.reddit.com/r/skyrimrequiem/comments/4mm3gr/how_to_get_the_reqtificator_and_presumably_other/) was immensely useful to me getting everything running. While this guide aims to be complete, you may also wish to check Vorgen's guide if you encounter troubles.

## Skyrim

I installed Skyrim using PlayOnLinux as my base. PlayOnLinux provides a solid Skyrim install, but disables a number of libraries (like Dwrite) which can make everything else harder later on, but it's how I started, and how I suspect many who will find this post will have started.

If you just want to play Vanilla skyrim, then it works perfectly out of the box with Wine 1.7.45. However **I suggest using Wine 2.13**, as older versions of Wine can have difficulty running some of the programs needed for a modded install.

If you have choppy sound issues under Wine 2.x, try starting PoL with `PULSE_LATENCY_MSEC=50 playonlinux`. This environment variable fixed all the sound issues for me.

If you are using wine 3.x and find there is no music or voices, run `winetricks xact` and then set a library override for `xaudio2_6` to `native, builtin`.

The only thing Skyrim seems to screw up on is that it leaves a hanging process around after exiting to desktop. Having a way to switch desktops will allow you to run a `killall TESV.exe` to remove the hanging process.

### Sticking Keys

If you find that your movement keys in Skyrim are 'sticky', and your character keeps moving in a direction for a while, simply run `xset -r`. It turns off the key repeat which is the source of the problem. When you're done playing, `xset r` restores key autorepeat.

### Performance

If you want the best performance, then using wine-staging with CSMT is the way to go. I got a *significant* performance boost by using wine-staging 2.13, enabling CSMT under the "Staging" tag of winecfg, and adding an override for `nvapi` in the libraries tab, and then using the 'edit' button to change that to "disabled".

More recent versions of wine 3.x have CSMT enabled by default.

## ModOrganizer

I'm *really* impressed by ModOrganizer, and was delighted to get it running under Linux. It has a few graphical hiccups when doing things like dragging mods around on the build order, but nothing that really affects functionality.

If using the built-in LOOT-based sort tool (which *will* screw up the order of Requiem, but can be useful in non-requiem games) then ModOrganizer will run the sort, tell you it's "Done", and then leave the interface locked. Closing and re-opening ModOrganizer will start it up with the re-sorted list.

I almost certainly had to install extra libraries and tricks to get ModOrganizer working, scroll to the end to see what my set-up looks like.

I never got ModOrganizer to handle `nxp://` links, but downloading into MO's download folder worked great, and it talks to Nexus just fine.

## TES5Edit

While TES5Edit isn't required for Requiem, it *is* required for some derived mods like Ultimate Skyrim. There are no special instructions for getting TES5Edit running. Just add it to the ModOrganiser run menu like you would on any other system. Everything worked out of the box with Wine 2.13.

## FNIS

FNIS was one of the more challenging tools to get running. I eventually had success by:

- Installing FNIS through ModOrganiser.
- Opening a PlayOnLinux shell
- Running `winetricks dotnet40`
- Copying the contents of the FNIS `Data/tools` directory from the downloaded archive into my Skyrim `Data` directory. `Data/tools` now exists both on the real filesystem, as well as inside MO.
- Adding `GenerateFNISforUsers.exe` from the real filesystem to ModOrganiser.
- Running FNIS through MO like any other tool.

Having FNIS installed through MO means you get `FNIS.esp` and other files, and can disable FNIS on profiles you don't want it. My reason for installing the tools on the real filesystem is that I would receive an `ERROR(5): File not found` when just running FNIS through MO on an otherwise clean Skyrim install.

FNIS would claim it couldn't find a legal Skyrim install, even though I'd purchased and installed it via Steam, and have previously run the vanilla game. This appears to just be a warning, and can be safely ignored.

[This post](https://www.reddit.com/r/wine_gaming/comments/4sdgn0/skyrim_fnis_through_mod_organizer/) is what allowed me to finally solve my FNIS issues.

Despite getting FNIS running, it can be a little tempremental. FNIS would sometimes fail on anything but the first run. Removing all its generated ('overwrite') files and running it afresh when working with new mods consistently solved this issue for me.

Rarely when FNIS would start there would be no text visible. Exiting FNIS (and choosing 'Cancel' if the shortcut dialog box appeared) and restarting it would solve this for me. (Sometimes a few restarts are required.)

## RealShelter

RealShelter has a `TES5Edit` script which is normally run from a batch file, but this didn't work for me. To get this running, I:

- Copied the contents of the `R.S.Patcher` directory into the `TES5Edit/Edit Scripts` directory.
- Started TES5Edit through ModOrganiser.
- Made a cup of tea while it loaded all the assets.
- Selected all mods by clicking on `[00] skyrim.esm` then scrolling down and shift-clicking on the last mod
- With all mods selected, right-click, select "Apply Script", and select "RSPatcher" from the drop-down.
- Let the script run, and accept any pop-ups wanting to add masters.

The same steps can be used for other TES5Edit scripts you may wish to run.

## Reqtificator

Getting the reqtificator running reliably was *really* hard. /u/vorgen's [guide](https://www.reddit.com/r/skyrimrequiem/comments/4mm3gr/how_to_get_the_reqtificator_and_presumably_other/) helped a lot by mentioning that Java 1.8.0u5 was apparently the only version to be stable under wine, and that's what I'm using. However I suspect modern wine releases will work with modern Java 1.8 runtimes when using the adjustments detailed below. My big advice is to grab the `.tar.gz` version of the java release rather than the installer, which I had trouble getting to run.

I made two changes to how the Reqtificator was called that made it *much* more stable on my system. The first was to skip the step where the Reqtificator starts a sub-process, instead choosing to do that directly. And the second was to turn off D3D acceleration which would cause it to glitch and crash more than 50% of the time on my system.

The command I use to run the Reqtificator on my system, as you'd enter them into ModOrganizer:

- Binary: `C:\path\to\jre1.8.0_05\bin\java.exe`
- Arguments: `-Xmx1024m -Dsun.java2d.d3d=false -jar "C:\path\to\ModOrganizer\mods\Requiem - The Roleplaying Overhaul\SkyProc Patchers\Requiem\Reqtificator.jar" -REQMYMEMORY`

Obviously replace `\path\to\...` with the relevant paths on your system.

With this set-up, the reqtificator and other SkyProc patchers run reliably on my system.

## Automatic Variants

Once I figured out how to get the Reqtificator running, AV was a dream:

- Binary: `C:\path\to\jre1.8.0_05\bin\java.exe`
- Arugments: `-Xmx1024m -Dsun.java2d.d3d=false -jar "C:\path\to\ModOrganizer\mods\Automatic Variants\SkyProc Patchers\Automatic Variants\Automatic Variants.jar" -REQMYMEMORY`

Again, replace `\path\to\...` with the relevant path for your system.

## Steam

Skyrim requires Steam to be already started, and I've found that letting ModOrganizer start steam before running skse was giving me grief, so I've made my own PlayOnLinux shortcut on the same virtual drive to just start Steam first. I'm running steam with `-no-dwrite -no-cef-sandbox` which works around bugs in both the Dwrite library (which I've enabled for everything else) and the chromium rendering engine (which would result in no text disabled anywhere).

`steamwebhelper.exe` isn't reliable with some combinations of Steam and wine, but unless you're planning to browse the store, it can be safely ignored.

## LOOT

Thanks to [a comment by wyrde](https://github.com/loot/loot/issues/610#issuecomment-275945928) I've successfully got LOOT v0.13.1 running from ModOrganizer. My set-up was:

- Override `libcef` (native, builtin) and `dwrite` (native, builtin). I had to manually add `libcef`.
- Start LOOT with `--no-sandbox` (may work okay without).
- Wine 2.13
- Windows XP emulation.

LOOT may give an error when exiting, but it can still download and sort mod lists, so the error can be safely ignored.

*Note that overriding `libcef` may cause steam to no longer be able to render the store.*

## Winetricks

I can't remember how many additional things I installed using, but it seemed like a few! Using PlayOnLinux to navigate to `Configure -> Misc -> Open a Shell` made things much easier for me as I practically live on the command-line.

[Winetricks](https://github.com/Winetricks/winetricks) is invaluable for installing libraries, and is frequently updated. However in some cases it would hit snags. I found that if winetricks failed, I could sometimes just run the downloaded file directly from the `~/.cache/winetricks/` directory.

See the album below for all the things I had installed.

## Imgur Album

Here's [an album](http://imgur.com/gallery/b6UdD) with screenshots of various things I needed to configure, so if you don't want to read the wall of text above you can hopefully spot the one thing that will make your Requiem under Linux experience complete!

## License

This entire guide is [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/) pjf. You should feel to share, adapt, and modify this post under the terms of this license. That includes linking in sidebars, including in larger works, adding to other guides, correcting, publishing, redistributing, or otherwise using it to make the world a better place. No additional permission is necessary, and attribution can just be made to 'pjf'.

This guide is [available on github](https://github.com/pjf/skyrim-requiem-wine). Patches and updates are *very* welcome, and I recommend checking out the github version as it tends to be the most up-to-date.

Thanks for reading along, and may you visit Tamriel using Linux today!
