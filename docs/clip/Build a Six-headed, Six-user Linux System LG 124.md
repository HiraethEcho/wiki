---
title: "Build a Six-headed, Six-user Linux System LG #124"
date: 2025-07-25
dg-publish: true
---
- description: 
- source: [url](https://linuxgazette.net/124/smith.html)
- author: Bob Smith

## ai摘要

This tutorial demonstrates how to build a six-headed, six-user Linux system using standard USB keyboards and mice, leveraging Xorg's multi-seat capability. The setup involves selecting and installing hardware, installing Linux, recording hardware configurations, modifying `xorg.conf`, and adjusting `gdm.conf`. The system offers advantages like cost savings, reduced noise, and lower power consumption, making it ideal for schools, Internet cafes, and libraries. The article provides detailed steps, hardware recommendations, and troubleshooting tips, though it notes instability issues during logout.

---

...making Linux just a little more fun!

## Build a Six-headed, Six-user Linux System

**By**

## Introduction

![Six users on one computer](https://linuxgazette.net/124/misc/smith/sixusers.jpg)

**A Multi-Seat Linux Box:** This tutorial shows how to build a multi-head, multi-user Linux box using a recent distribution of Linux and standard USB keyboards and mice. Xorg calls this arrangement a "multi-seat" system.

**Advantages of a Multi-Seat System:** The advantages of multi-seat systems in schools, Internet cafes, and libraries include more than just saving money. They include much lower noise pollution, much less power consumption, and lowered space requirements. For many applications, power and noise budgets are as important as initial cost.

**Requirements:** To build a multi-seat system you need a video adapter, keyboard, and mouse for each seat. For six seats, you'll also need a motherboard with an AGP slot and five available PCI slots. In our test system we used USB keyboards and mice exclusively, but you can use a PS/2 keyboard and mouse for one of the seats if you wish.

Xorg 6.9 or later is required, but this already ships with many of the major distributions. Our test system uses the free version of Mandriva 2006 and we did not rebuild the kernel or install any additional packages.

## Overview

We divide the implementation of a multi-seat system into five main steps:
1. Select and Install the Hardware
2. Install Linux
3. Record Hardware Configuration
4. Modify xorg.conf
5. Modify gdm.conf
After installing the hardware and installing Linux, we read the hardware configuration from the lspci command or from the /proc/bus/input/devices file. Most of the effort in setting up a multi-seat system is in transcribing the hardware information into the xorg.conf file.

## Step 1: Select and Install the Hardware

**Selecting the Hardware:** There are few set rules dictating what hardware to use in your multi-seat system. Of necessity, some of the keyboards and mice need to use USB, but there is no minimum CPU or memory requirements. We suggest building and testing a multi-seat system using a computer that you already have, and using the test results to help scale your hardware requirements. You may be surprised how modest the CPU and memory requirements are for a multi-seat system that is used only for web browsing.

If possible, try to use accelerated video cards, but for increased reliability, avoid video cards with on-board fans. Use recent video cards; older video cards often have a problem sharing the PCI bus. We've had good luck with nVidia cards but you can try recent cards from other manufacturers too.

**Hardware for our test system:** For our system we chose to use video cards based on the nVidia MX4000 chipset. They are accelerated, have no fans, and it was nice having one driver for all six video cards. The downside of nVidia is that the driver is closed source and you need to download and install it. If you use an nVidia card, be sure to check their web site for the recommended BIOS settings for your cards.

[![A typical PC](https://linuxgazette.net/124/misc/smith/hydra_small.jpg)](http://www.linuxtoys.org/multiseat/hydra_big.jpg) We used an ECS 755-A2 motherboard with an AMD64-3200 processor and 1 GB of RAM. Our power supply is a CoolMax 140mm Power Supply and the CPU heat sink is a Thermaltake "Sonic Tower". During our testing we added a low noise fan to cool the video cards. Airflow is in at the bottom, past the video cards, up past the CPU cooler and out through the power supply. This airflow seemed to work pretty well. At quiescence, the CPU temperature was 31C, rising to only 38C after fifteen minutes of kernel compile. The current from the mains at quiescence was 0.25 amps, and during a kernel compile it was 0.35 amps.

You will probably need some USB hubs to connect all of the keyboards and mice. One problem to think about before permanently installing the hardware is cable management. Seven power cords, six monitor cables, three USB hubs, six keyboard cables, and six mice cables: that is a lot of cabling!

## Step 2: Install Linux

Multi-seat capability is provided by Xorg 6.9/7.0 which already ships with most of the major distributions. When you install Linux, you might want to install all of the window managers including fluxbox and twm. If you are going to use the nVidia drivers, be sure to install the kernel source too.

Do the installation with all of the hardware connected and powered up. Mandriva did a great job detecting and configuring all six of our video heads. Select a default run level of 3 so that X does not start automatically after boot. You can check the installation by logging in and running startx. If all has gone well you should be able to move your mouse across all six monitors.

Mandriva allows up to ten entries in the /dev/input directory. We needed twelve since we had six keyboards and mice. We increased the limit to sixteen by changing the line in /etc/udev/ruled.d/50-mdk.rules from:  
  
KERNEL=="event\[0-9\]\*", NAME="input/%k", MODE="0600"  
to:  
KERNEL=="event\[0-9a-f\]\*", NAME="input/%k", MODE="0600"

## Step 3: Record Hardware Configuration

All hardware in our computer has a name that distinguishes it from similar hardware in the computer. In this step we record the names for each of our video heads, keyboards, and mice. Let's start with the video cards.

Video cards are identified by their address on the PCI bus. We can list the hardware on the PCI buses using the lspci command. On our test system, the lspci command gives the following result:

```
lspci | grep VGA
00:09.0 VGA compatible controller: nVidia Corporation NV18 [GeForce4 MX 4000 AGP 8x] (rev c1)
00:0a.0 VGA compatible controller: nVidia Corporation NV18 [GeForce4 MX 4000 AGP 8x] (rev c1)
00:0b.0 VGA compatible controller: nVidia Corporation NV18 [GeForce4 MX 4000 AGP 8x] (rev c1)
00:0c.0 VGA compatible controller: nVidia Corporation NV18 [GeForce4 MX 4000 AGP 8x] (rev c1)
00:0d.0 VGA compatible controller: nVidia Corporation NV18 [GeForce4 MX 4000 AGP 8x] (rev c1)
01:00.0 VGA compatible controller: nVidia Corporation NV18 [GeForce4 MX 4000 AGP 8x] (rev c1)
```
The bus address is the first field in the lines above. The number before the colon identifies which PCI bus (computers often have more than one), and the second number gives the card address on the bus. You will need to know these addresses to build the xorg.conf configuration file.

The mice are easy to locate. Each mouse has an entry in the /dev/input directory. An ls can identify the mice.

```
ls /dev/input/mouse*
/dev/input/mouse0  /dev/input/mouse2  /dev/input/mouse4
/dev/input/mouse1  /dev/input/mouse3  /dev/input/mouse5
```
The keyboards are identified as a /dev/input/eventN file. Do a more of /proc/bus/input/devices. Each keyboard will have an entry that specifies the event file. The following two entries are for the first two keyboards in our system.
```
more /proc/bus/input/devices

I: Bus=0003 Vendor=046e Product=530a Version=0001
N: Name="BTC Multimedia USB Keyboard"
P: Phys=usb-0000:00:03.3-4.2.1/input0
H: Handlers=kbd event6 
B: EV=120003 
B: KEY=1000000000007 ff87207ac14057ff febeffdfffefffff fffffffffffffffe 
B: LED=1f 

I: Bus=0003 Vendor=046e Product=530a Version=0001
N: Name="BTC Multimedia USB Keyboard"
P: Phys=usb-0000:00:03.3-4.4.1/input0
H: Handlers=kbd event7 
B: EV=120003 
B: KEY=1000000000007 ff87207ac14057ff febeffdfffefffff fffffffffffffffe 
B: LED=1f
```

A table is a nice way to view all of the above information.

| Seat | Video Card | Keyboard   (/dev/input/) | Mouse   (/dev/input/) |
| --- | --- | --- | --- |
| 0 | 00:09:0 | event6 | mouse0 |
| 1 | 00:10:0 | event7 | mouse1 |
| 2 | 00:11:0 | event8 | mouse2 |
| 3 | 00:12:0 | event9 | mouse3 |
| 4 | 00:13:0 | event10 | mouse4 |
| 5 | 01:00:0 | event11 | mouse5 |

  

Note the slight change in how the video cards are addressed. Also, you'll find the numbering of the keyboards and mice easier if you plug each mouse into the same hub as its corresponding keyboard. Don't worry too much about matching the video head to the keyboard. After setting everything up you can move the monitors or the keyboards around as needed.

## Step 4: Build xorg.conf

The xorg.conf file has sections to describe keyboards, mice, video cards, monitors, screens, and seats. Most of the work in setting up a multi-seat system is correctly copying the information in the above table into the appropriate section of the xorg.conf file. Shown below is our configuration for seat 5. You should be able to use this configuration as a prototype for your additional seats. Note the places where the keyboard, mouse, and video card information is located. Since we were borrowing monitors for our test, we forced all of the monitors to be flat panel displays with a 1024 by 768 resolution.
```
# Seat 5
Section "InputDevice"
    Identifier     "Keyboard5"
    Driver         "evdev"
    Option         "Device" "/dev/input/event11"
    Option         "XkbModel" "pc105"
    Option         "XkbLayout" "us"
    Option         "XkbOptions" "compose:rwin"
EndSection

Section "InputDevice"
    Identifier     "Mouse5"
    Driver         "mouse"
    Option         "Protocol" "ExplorerPS/2"
    Option         "Device" "/dev/input/mouse5"
    Option         "ZAxisMapping" "6 7"
EndSection

Section "Device"
    Identifier     "device5"
    Driver         "nvidia"
    VendorName     "NVIDIA Corp."
    BoardName      "NVIDIA GeForce4 (generic)"
    BusID          "PCI:0:13:0"
EndSection

Section "Monitor"
    Identifier     "monitor5"
    ModelName      "Flat Panel 1024x768"
    HorizSync       31.5 - 48.5
    VertRefresh     40.0 - 70.0
    ModeLine       "768x576" 50.0 768 832 846 1000 576 590 595 630
    ModeLine       "768x576" 63.1 768 800 960 1024 576 578 590 616
EndSection

Section "Screen"
    Identifier     "screen5"
    Device         "device5"
    Monitor        "monitor5"
    DefaultDepth    24
    SubSection     "Display"
        Virtual     1024 768
        Depth       24
    EndSubSection
EndSection

Section "ServerLayout"
    Identifier     "seat5"
    Screen      0  "Screen5" 0 0
    InputDevice    "Mouse5" "CorePointer"
    InputDevice    "Keyboard5" "CoreKeyboard"
EndSection
```
There is a simple trick to help verify that all the numbers in the xorg.conf file are right -- pass the file through sort and uniq.  
```
sort /etc/X11/xorg.conf | uniq
```

\[ 'sort xorg.conf|uniq -d' would also be helpful - just in case you had mistakenly repeated any of the device strings. -- Ben \]

The output of the above command string will make obvious any errors in numbering the various keyboards and such.

**Testing Your Xorg.conf File:** It is a good idea to test your configuration and to sort out the keyboards and mice by bringing up the heads one at a time. Login remotely so that you are not using any of the video heads. Enter the following commands for each of the six heads (0 to 5). (The commands below are for head 5.)  

```
X -novtswitch -sharevts -nolisten tcp -layout seat5 :5 & 
xterm -display :5 &
```
If the above command fails, examine the error messages and check the xorg.conf file. If the command succeeds, use the xterm to help identify which keyboard and mouse go to which head. The keyboards, mice, and video cards are enumerated in the same order on every boot, so you will only have to move things around during the initial set up.

The above commands might be sufficient if you don't need user logins. For example, a six headed kiosk might need only X and a web browser on each head.

## Step 5: Modify gdm.conf

If you want user logins you will need to modify the configuration for your preferred display manager. The directions given here are for gdm but the changes are very similar for kdm, or for the X display manager, xdm.

Modify the \[servers\] section near the bottom of the /etc/X11/gdm/gdm.conf file to tell gdm which X servers to start. The lines should be:

```
0=Standard0
1=Standard1
2=Standard2
3=Standard3
4=Standard4
5=Standard5
```
You need to tell gdm how to start the X server on each head. The lines to do this are:
```
[server-Standard5]
name=Standard server
command=/usr/X11R6/bin/X -nolisten tcp -novtswitch -sharevts -layout seat5
flexible=true
```
You'll need a section like the above for each head. The server name, "Standard5" in the above example, must match the name given in the \[servers\] section. Customize the X command line options to meet the requirements of your particular system.

Once everything is configured, you should be able to start graphical logins by switching to runlevel 5.

```
telinit 5
```
If everything works, make the default runlevel 5 by editing /etc/inittab or by setting it using drakconf.

## Test Results, Costs, and Problems

**Performance Results:** Between resets, we found performance to be excellent for six users doing typical PC tasks, including web browsing, email, word processing, and games. The accelerated graphics cards seemed to do most of the work so that even arcade style games and web-based video did not put much of a load on the CPU. If "3200" is an accurate assessment of the performance of the AMD64-3200, then a CPU with a performance of "1600" would have been more than sufficient.

**Cost:** Not including the monitor, each seat in our system cost about $67. This includes $40 for the MX4000 based video card, $20 for a USB keyboard, $5 for a USB mouse, and $2 for half of a USB hub. Our test system used expensive keyboards that have a built-in USB hub which we intended for per-user flash drives or audio players.

The shared part of our system cost about $520. This includes $180 for the CPU, $50 for the motherboard, $90 for RAM, and $50 for the CPU heat sink. The case, power supply, and disk drive had a combined cost of about $150.

We give these prices just for comparison. You may find lower prices that these and we'd certainly recommend that you replace our $230 CPU and motherboard with an Athlon 2800+ set that costs about $80. We have not included the cost of the monitors since these prices are in free fall and your particular needs and tastes may dictate what you spend.

**Problems:** Did you catch the phrase "between resets" above? While the system worked very well, it was extremely unstable. In particular, we got a kernel oops fairly often when we logged out. A syslog trace of one such oops is available [here](http://www.linuxtoys.org/multiseat/hydra_hang.txt). We've tried several things to fix this problem including:  

- turning APIC off and on
- reducing the number of heads
- trying the 'nv' and 'vesa' drivers
- using NoInt10
- upgrading to the official X11R6.9 release
- upgrading to the 2.6.15 kernel
- using xdm and fvwm instead of gdm and Gnome
The problem persists. Please let bsmith at linuxtoys dot org know if you have any ideas that might help fix this problem.

A much less severe problem is that some programs assume that there is a single user on the PC. Screen savers can take a lot of CPU power and both KDE and Gnome complain if they don't have audio output. Any shared resource, such as audio or a CD burner, can be a problem.

As a longer-term concern, we will need to address security issues surrounding multi-seat computers. Whether from students or cafe patrons, these systems are going to come under deliberate, malicious attack. Can we trust KDE and Gnome to withstand such attacks?

## Summary

A multi-head, multi-user Linux system is now possible using commodity PC hardware and standard Linux distributions. Multi-seat Linux PCs seem inevitable given the potential savings in cost, noise, and power.

## Further Reading

**Chris Tyler's page:** Chris Tyler provided support at almost every step of the way in this project. His web site has a HOWTO that also describes how to set up a multi-seat system. Chris is something of an expert in X and I'm looking forward to his next book which will contain some of the material presented here. Chris' web site is at:  
[http://blog.chris.tylers.info/](http://blog.chris.tylers.info/)

**Xorg man pages:** Xorg provides a full set of manual pages that describe the xorg.conf file and all of the commands used in getting X-Windows to run. The manual page for xorg.conf is at:  
[http://wiki.x.org/X11R6.9.0/doc/html/xorg.conf.5.html](http://wiki.x.org/X11R6.9.0/doc/html/xorg.conf.5.html)

The manual pages for the X commands are at:  
[http://wiki.x.org/X11R6.9.0/doc/html/manindex1.html](http://wiki.x.org/X11R6.9.0/doc/html/manindex1.html)

Talkback: [Discuss this article with The Answer Gang](https://linuxgazette.net/124/)

---

*Bob is an electronics hobbyist and Linux programmer. He is one of the authors of "Linux Appliance Design" to be published by No Starch Press.*

  

![Tux](https://linuxgazette.net/gx/tux_86x95_indexed.png)