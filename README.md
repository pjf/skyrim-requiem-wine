# Running Skyrim/Requiem under Linux/Wine

After years away from Skyrim and Requiem, I had a hankering to play again, but as a Linux gamer and developer I realised that I'd never actually play if I ever had to reboot to Windows. I've been successful in this quest, and have managed to get a very successful and stable Requiem install running. I imagine this would also be useful for other mods involving running SkyProc patchers, such as SkyRe or PerMa.

Here I record the steps I've taken and the victories I've had, so that those who come after me don't have to repeat all the mistakes I've made. Since the person most likely to need this will be myself in two years time, this is pretty much a love letter to my future-self.

**Update with Wine 2.13**

The below guide was written before wine 2.x was released. In August 2017 I tested Skyrim under Wine 2.13, and discovered the sound issues were resolved provided one set an appropriate pulseaudio environment varialble: `PULSE_LATENCY_MSEC=50 playonlinux`. This has given me perfect sound, and eliminated the need for separate wine versions for installation and play.

**Requiem under Linux with wine: What's I have working:**

- Skyrim with all the DLC.
- ModOrganizer.
- Requiem, including the Reqtificator and all dependencies.
- Numerous various mods and enhancements (not documented here).

Everything is stable, solid, and feature complete, with only a couple of exceptions that I'll detail below. The only thing I couldn't get running was the graphical version of LOOT.

**Vorgen's Guide**

If you haven't found it, /u/Vorgen's [guide](https://www.reddit.com/r/skyrimrequiem/comments/4mm3gr/how_to_get_the_reqtificator_and_presumably_other/) was immensely useful to me getting everything running. You probably want to open it now.

**Skyrim**

I installed Skyrim using PlayOnLinux as my base. PlayOnLinux provides a solid Skyrim install, but disables a number of libraries (like Dwrite) which can make everything else harder later on, but it's how I started, and how I suspect many who will find this post will have started.

I found that Skyrim would have sound issues under wine 1.9.x, but worked perfectly under 1.7.x. I'm using **wine 1.7.45 for Skyrim**, and the sound and gameplay have been absolutely solid.

The only thing Skyrim seems to screw up on is that it leaves a hanging process around after exiting to desktop. A `killall TESV.exe` removes this nicely on my system.

**ModOrganizer**

I'm *really* impressed by ModOrganizer, and was delighted to get it running under Linux. It has a few graphical hiccups when doing things like dragging mods around on the build order, but nothing that really affects functionality. I've found it runs beautifully under wine 1.7.x and wine 1.9.x, but its built-in sort-tool (which runs `lootcli.exe` underneath), won't run in 1.7.x.

If using the built-in LOOT-based sort tool (which *will* screw up the order of Requiem, but is useful in non-requiem games) I use wine 1.9.24. ModOrganizer will run the sort, eventually tell you "Done", and then leave the interface locked, but closing and re-opening ModOrganizer will start it up with the re-sorted list.

I almost certainly had to install extra libraries and tricks to get ModOrganizer working, scroll to the end to see what my set-up looks like.

I never got ModOrganizer to handle `nxp://` links, but downloading into MO's download folder worked great, and it talks to Nexus just fine.

**Reqtificator**

Getting the reqtificator running reliably was *really* hard. /u/vorgen's [guide](https://www.reddit.com/r/skyrimrequiem/comments/4mm3gr/how_to_get_the_reqtificator_and_presumably_other/) helped a lot by mentioning that Java 1.8.0u5 was apparently the only version to be stable under wine, and that's what I'm using. However I suspect modern wine releases will work with modern Java 1.8 runtimes when using the adjustments detailed below. My big advice is to grab the `.tar.gz` version of the java release rather than the installer, which I had trouble getting to run.

I made two changes to how the Reqtificator was called that made it *much* more stable on my system. The first was to skip the step where the Reqtificator starts a sub-process, instead choosing to do that directly. And the second was to turn off D3D acceleration which would cause it to glitch and crash more than 50% of the time on my system.

The command I use to run the Reqtificator on my system, as you'd enter them into ModOrganizer:

- Binary: `C:\path\to\jre1.8.0_05\bin\java.exe`
- Arguments:  `-Xmx1024m -Dsun.java2d.d3d=false -jar "C:\path\to\ModOrganizer\mods\Requiem - The Roleplaying Overhaul\SkyProc Patchers\Requiem\Reqtificator.jar" -REQMYMEMORY`

Obviously replace `\path\to\...` with the relevant paths on your system.

I've been running the Reqtificator under ModOrganizer under wine 1.9.24 and it's been 100% functional, stable, and feature complete. I have "merge leveled characters" and "merge leveled items" checked, but imagine everything would work without that. I can also start it under wine 1.7.x, but haven't tested that it runs all the way through, but expect it probably does.

**Steam**

Skyrim requires Steam to be already started, and I've found that letting ModOrganizer start steam before running skse was giving me grief, so I've made my own PlayOnLinux shortcut on the same virtual drive to just start Steam first. I'm running steam with `-no-dwrite -no-cef-sandbox` which works around bugs in both the Dwrite library (which I've enabled for everything else) and the chromium rendering engine (which would result in no text disabled anywhere).

Unfortunately `steamwebhelper.exe` doesn't want to run for me with my configuration, but I'm not browsing the store here, so I'm not particularly worried.

Again, using wine 1.7.45 for starting steam, because I'm only doing this when I want to play Skyrim.

**Winetricks**

I can't remember how many additional things I installed using, but it seemed like a few! Using PlayOnLinux to navigate to `Configure -> Misc -> Open a Shell` made things much easier for me as I practically live on the command-line. Getting `vcrun2015` was part of my efforts to try and get LOOT to run. If it turns out you need it, I found that winetricks would have trouble, but then nabbing the file out of `~/.cache/winetricks/vcrun2015` and running it directly under wine worked a treat. See the album below for a list of all the things I installed.

**Imgur Album**

Here's [an album](http://imgur.com/gallery/b6UdD) with screenshots of various things I needed to configure, so if you don't want to read the wall of text above you can hopefully spot the one thing that will make your Requiem under Linux experience complete!

**License**

This entire guide is [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/) pjf. You should feel to share, adapt, and modify this post under the terms of this license. That includes linking in sidebars, including in larger works, adding to other guides, correcting, publishing, redistributing, or otherwise using it to make the world a better place. No additional permission is necessary, and attribution can just be made to 'pjf'.

Thanks for reading along, and may you visit Tamriel using Linux today!
