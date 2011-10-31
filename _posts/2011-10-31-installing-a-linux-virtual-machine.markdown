---
title: Installing a Linux Virtual Machine
layout: post
reddit_community: technology
---

If you're interested in operating systems (and you're on a blog about operating systems, so you really should be), you owe it to yourself to use one that's open source, and I'm not talking about Android. I'm talking about *free software*. I'm talking about Linux. I'm talking about FreeBSD, OpenBSD, DragonFly BSD, NetBSD, OpenSolaris, Plan 9 From Bell Labs -- there's [an entire page of them on Wikipedia](http://en.wikipedia.org/wiki/Category:Free_software_operating_systems)!

Imagine if, instead of operating systems, we were talking about the human body, and we got to discussing our favorite bits of anatomy. Imagine that, when I'm talking about my love for kidneys, you said something like: "Kidneys? What are kidneys?" and then I say something like "Well, they're an organ that..." -- then you cut me off with -- "An organ! You mean there's another one, besides the skin?"

Being interested in operating systems and only using proprietary ones is just like being interested in the human body and only studying what you can see from the outside. There's an entire world hidden from you!

If I were you, the first thing that I'd do is backup any important files and then I'd install Linux. Simple as that. 

Of course, maybe you need Windows for work, or for whatever reason you can't afford to just wipe everything and install Linux. Does that mean you just don't get to experience the world that's beneath the skin? 

The answer is, gentle reader, there's still a way that you, too, can experience free software. In fact, I've written this entire article just for you. You can run Linux as a virtual machine, which is almost as good as running it natively.

### What You'll Need

There are many different flavors of Linux, known as distributions, but for our virtual machine, we'll be installing the x86_64 version of Fedora. You'll need a copy of the install image, available [here](http://download.fedoraproject.org/pub/fedora/linux/releases/test/16-Beta/Fedora/x86_64/iso/Fedora-16-Beta-x86_64-DVD.iso).

Neither OSX nor Windows ship with the software necessary to construct a virtual machine, so you'll need to download that, too. Fortunately, VirtualBox is free to download. You can get it [here for Windows users](http://download.virtualbox.org/virtualbox/4.1.4/VirtualBox-4.1.4-74291-Win.exe) and [here for OSX users](http://download.virtualbox.org/virtualbox/4.1.4/VirtualBox-4.1.4-74291-OSX.dmg).

Installing VirtualBox is a straightforward affair, just follow the onscreen instructions.

### Creating Our Virtual Machine

1. Open VirtualBox and choose "New." The virtual machine wizard will appear.

    ![virtual machine wizard virtualbox](http://os-blog.com/img/linux-vm-osx-1.jpg)

2. Enter a name for your virtual machine, such as "OS Dev Machine." Under the "Operating System" drop down menu, choose "Linux." For the "Version" drop down menu, choose "Fedora (64 bit)." Then, select "Continue."

    ![naming your virtual machine virtualbox](http://os-blog.com/img/linux-vm-osx-2.jpg)

3. How much memory you decide to dedicate to your virtual machine depends on your personal preference and on how much memory your host machine has. I reccommend you allocate at least 512 MB. You can always change this setting later. Once you've decided, press "Continue."

    ![deciding how much memory to give your virtual machine virtualbox](http://os-blog.com/img/linux-vm-osx-3.jpg)

4. The default settings are sufficient, choose "Continue."

5. You should now be presented with the virtual disk creation wizard. Click "Continue."

    ![virtual disk creation wizard virtualbox](http://os-blog.com/img/linux-vm-osx-4.jpg)

6. Make sure "Dynamically allocated" is selected and then "Continue."

    ![virtual disk storage details virtualbox](http://os-blog.com/img/linux-vm-osx-5.jpg)

7. The size of your virtual machine's hard disk depends on how much you intend to use your virtual machine and what for. If you intend to use the virtual machine just for OS development, you can get away with using a smaller virtual disk than if you want to try out everything that Linux has to off. 

    A 10 GB virtual disk offers a happy medium, but if you don't plan to use your newly created virtual machine very often or you're strapped for disk space, you can get away with a smaller virtual disk. If you intend to store video or music files on your virtual disk, plan accordingly.
    
    Once you've chosen the right size for your virtual machine, click "Continue."
    
    ![virtual disk file location and size virtualbox](http://os-blog.com/img/linux-vm-osx-6.jpg)
    
8. Verify that your settings are correct, then choose "Create."

    ![summary virtualbox](http://os-blog.com/img/linux-vm-osx-7.jpg)

9. Make sure that your settings are correct and then click "Create." You can always modify these later.

    ![summary virtualbox](http://os-blog.com/img/linux-vm-osx-8.jpg)

10. Go to "Settings" > "System" > "Processor" and increase your virtual machine's available processors to the number of CPUs in your host machine.

    ![processor settings virtualbox](http://os-blog.com/img/linux-vm-osx-9.jpg)

11. Now go to "Display." Increase video memory to at least 32 MB if possible. Select "Enable 3D Acceleration" and "Enable 2D Video Acceleration." If VirtualBox warns you with "Non-optimal settings detected," just ignore it. Click "Ok."

    ![display settings virtualbox](http://os-blog.com/img/linux-vm-osx-10.jpg)

12. Select "Start." Read the message on how to leave keyboard-capture mode and then press "Ok." The "First Run Wizard" should appear. Press "Continue" and then click the folder icon. Find the Fedora disk image that you downloaded, select it, and press "Open."

    ![open fedora 16 disk image virtualbox](http://os-blog.com/img/linux-vm-osx-11.jpg)
    
13. Now, your virtual machine will start running. At the prompt, select "Install or upgrade Fedora" and press the enter key.
    
    ![fedora install prompt](http://os-blog.com/img/linux-vm-osx-12.jpg)

14. If you want to test the integrity of the installation media, select "OK" and press enter. Otherwise, choose "Skip."

15. If you'd prefer to use a language other than the default, English, select that language now. Press "Next."
    
    ![fedora install select language prompt](http://os-blog.com/img/linux-vm-osx-13.jpg)

16. Select your keyboard layout and then click "Next."

    ![fedora install select keyboard layout prompt](http://os-blog.com/img/linux-vm-osx-14.jpg)

17. Make sure "Basic Storage Devices" is selected and then choose "Next."

18. Select "Yes, discard any data." Note: this only applies to your *virtual* hard disk, not your real one. 

    ![fedora install storage device warning](http://os-blog.com/img/linux-vm-osx-15.jpg)

19. Choose a name for your virtual machine and then click "Next." I name all my machines after countries, so I will be calling mine "Spain."

    ![fedora install name this computer](http://os-blog.com/img/linux-vm-osx-16.jpg)

20. Select your timezone or city and then select "Next."
    
    ![fedora install select timezone](http://os-blog.com/img/linux-vm-osx-17.jpg)

21. Enter your desired root password twice. Press "Next."

22. Select "Use All Space." If you so desire, you can also check "Encrypt system." Choose "Next."

    ![fedora install partition](http://os-blog.com/img/linux-vm-osx-18.jpg)

23. Press "Write All Changes To Disk."

24. Press "Next."

    ![fedora install choose software](http://os-blog.com/img/linux-vm-osx-19.jpg)

25. Fedora will now begin the installation process. This can take a while depending on the speed of your hard disk. Grab a cup of coffee and [subscribe to our RSS feed](http://feeds.feedburner.com/os-blog/H) or [email newsletter](http://eepurl.com/gIQ-P) while you wait.

    ![fedora installation process](http://os-blog.com/img/linux-vm-osx-20.jpg)

26. Click "Reboot." 

    ![fedora completed installation](http://os-blog.com/img/linux-vm-osx-21.jpg)

27. Once your machine is finished rebooting, Fedora will present you with a wizard that will guide you through creating a user, setting your system's time and, optionally, sending a hardware profile to Fedora. Once you make it through the wizard, you can start using your brand new virtual machine.

28. Congratulations! You just installed a Linux virtual machine. Welcome to the brave new world of open source software!

    ![fedora default desktop](http://os-blog.com/img/linux-vm-osx-22.jpg)
