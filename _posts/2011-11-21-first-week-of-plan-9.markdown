
---
title: First Week of Plan 9
layout: post
---

![Plan 9 From Bell Labs, acme text editor](http://os-blog.com/img/plan9.jpg)

I wrote last week that [I'm going to use Plan 9 for one month](http://os-blog.com/one-month-of-plan-9/) to get real work done and that I'm going to blog about my experience. It's been a week, so it's time for my first progress report.

The first step was, obviously, to install Plan 9 as a virtual machine. I downloaded the install ISO from [the Bell Labs website](http://cm.bell-labs.com/plan9/) and booted it in VirtualBox on my MacBook. I made it only part way through the installation before the screen was filled with disk errors. I've since observed that helpful error messages are not the Plan 9 way.

I figured that I'd have better luck with QEMU. I did a search for pre-built binaries, but only found the [Q project](http://www.kju-app.org/), which doesn't exactly look like it's thriving. I'm not even sure if the project is actively maintained.  Further Googling revealed surprisingly few results regarding QEMU on OS X, which didn't bode well.

So, I was resigned to building QEMU from source, but I figured that homebrew would handle any of the heavy lifting for me, assuming that it had a formula for QEMU. Turns out homebrew does have a formula for QEMU but, upon running `brew install qemu`, I was greeted with a warning that QEMU builds are known to fail at runtime when compiled with LLVM and that I should build with the flag `--use-gcc`. Then, homebrew does the sensible thing and cancels the build, right? No, *it continues with the known broken build.*

I then tried building with `--use-gcc`, but apparently the flag does nothing on Lion, not that homebrew will bother, y'know, warning you about this. The `--use-clang` flag does work, but QEMU won't even compile with clang, let alone segfault. At this point, I'd pretty much exhausted all of homebrew's available options. 

Still, I wasn't ready to give up on QEMU quite yet since that would have meant I'd be stuck using VMWare, proprietary software. I grabbed the QEMU development branch and tried to compile that, but ultimately I couldn't get it working either. So, it was time to try VMware.

After downloading and installing VMWare Fusion, I was once again ready to install Plan 9, but I ran into some difficulty trying to find a list of VMWare's supported VESA modes. Under VirtualBox, I had added a custom mode by modifying my virtual machines configuration file. Finding nothing on Google, I asked if there was some way to query available VESA modes under Plan 9 in #plan9 on Freenode.

It turns out, you can use `aux/vga -m vesa -p`. However, if you try to do that under VMWare Fusion, the virtual machine dies. It stops responding and eats all of your available CPU. Great. On IRC, Cinap Lenrek suggested that I either install realemu or use 9front, a Plan 9 fork which had solved this particular issue.

I decided to go with 9front. I went and downloaded a 9front ISO, booted that, and proceeded with the install. Part of the way through, my VM *ran out of diskspace*. Apparently, to install 9front, you need 12 GB of free space. To give you an idea of how ridiculous that 12 GB requirement is, my entire vanilla Plan 9 VM image -- now that I've got it installed -- takes up a little less than 1.5 GB, so it's about 10.5 GB of ridiculous. If the 9front project's goal is "Let's add a bunch of bloat to Plan 9!", well, they're on the right track.

At any rate, 12 GB was more space than I could afford to allocate on my paltry 80 GB SSD, so that was the end of the line for 9front. I'm kind of glad things didn't work out with 9front, as the project's humor is just [weird and offputting](http://code.google.com/p/plan9front/wiki/propaganda). 

Luckily, though, you don't actually need working graphics in order to install Plan 9. There is a text installer which I ended up using. This time, thankfully, the install completed without a hitch. I rebooted and even managed to get the GUI working, albeit with a resolution at 800x600. Things were looking up.

Adding a user account and granting it administrative privileges was a breeze. Figuring out where I could find a copy of realemu, so that I could query and set VESA modes without crashing my VM, proved more difficult, but ultimately I found that there is a repository of [user-contributed Plan 9 software](http://plan9.bell-labs.com/wiki/plan9/contrib_index/). When I tried to access it (after setting up networking), I was greeted by "DNS failure" The message assured me that it was temporary problem. It wasn't.

I could ping servers, but I couldn't resolve hostnames. An unfamiliar system, it took me hours of debugging to solve the problem. Eventually, after asking on IRC, I discovered how to override the DNS server received via DHCP, which fixed my problems. (You just have to run the command `DNSSERVER=8.8.8.8` before running `ndb/dns -r`.) For reasons unknown, either my router or Comcast's DNS servers don't play nice with Plan 9 systems.

realemu compiled and installed with any fuss and, indeed, it allowed me to query the available VESA modes. When I tried to switch to *any* of the VESA modes, though, the virtual machine's screen went dark. It still responded to commands, but it wasn't displaying anything. I asked around on IRC and they suggested that I set up my Plan 9 VM as a CPU server so that I'd be able to `drawterm` into the machine and read any error output.

The [instructions to run Plan 9 as a CPU server](http://plan9.bell-labs.com/wiki/plan9/configuring_a_standalone_cpu_server/index.html) are intimidating, so, I decided to put that off for a time while I tried other things. 

In the end, I never did manage to get Plan 9's VESA driver working under VMWare Fusion, but I did find a different driver that *almost* supports my desired resolution, 1280x800. (You can find all of Plan 9's drivers and the resolutions that they support by running `cat /lib/vgadb`.) I ended up going with `aux/vga -m cinema -l 1280x768` which, as you can see, is close enough to my monitor's native resolution. I can live with it, anyways, and that's significantly more than I can say about the 800x600 resolution that I was using (if you can even call such a hell "using").

Thusly, I had my Plan 9 install in a state where I could get real work done. I had a sensible resolution, I had functional networking, I was good to go. So, I wrote this post.

Yes, it took me an entire week to get Plan 9 installed and functioning. You'll have to wait until next week for impressions related to, y'know, actually using the system.
